# Agentic RL 在 API/Tool-use 场景下的奖励函数设计：系统性研究综述与工程参考

## 核心观点

Agentic RL 的工具调用能力不能只靠一个“任务完成”分数来训练，也不能把 JSON、AST、HTTP `200`、单元测试通过或 LLM judge 高分直接等同于真实成功。本综述把 reward design 拆成三层：

```text
syntax correctness  !=  execution success  !=  semantic / task success
```

更实用的 reward 不是单项分数，而是按 oracle 可靠性分层组合：

```text
R_total =
  w_task   * R_task_success
+ w_schema * R_schema
+ w_exec   * R_execution
+ w_proc   * sum_t R_process_t
+ w_safe   * R_policy_compliance
- lambda_c * cost(tokens, calls, retries)
- beta     * KL(pi, pi_ref)
```

这个公式不是建议所有项都加满。相反，关键是知道每个信号能证明什么、不能证明什么，以及它在训练中会被怎样钻空子。

## 第 1 部分：领域地图

| 范式 | 代表工作 | reward / optimization 信号 | 适用场景 | 主要限制 |
|---|---|---|---|---|
| PPO / GRPO / REINFORCE 类 RL | Search-R1 (Jin et al., 2025)、R1-Searcher (2025)、ReTool (Feng et al., 2025)、ARTIST (Singh et al., 2025)、RAGEN (Zhang et al., 2025)、ToolRL (Liu et al., 2025)、MUA-RL (2025) | final answer、task outcome、tool correctness、search/F1 reward | search、code execution、multi-turn user/tool interaction | outcome sparse，长轨迹 credit assignment 难 |
| Dense / turn-level credit RL | Turn-Level Credit Assignment (2025)、CM2 (2026)、Task Reasoning GRPO (2025) | turn-level retrieval、format、checklist、expert-trajectory reward | multi-turn search、task planning、multi-step tool use | verifier / judge 噪声会被 dense reward 放大 |
| Preference / DPO-like optimization | DMPO (Chen et al., 2024)、EPO (Zhao et al., 2024) | preferred vs rejected trajectories | multi-turn language agents、environment-feedback agents | KTO / SimPO 在当前 tool-use source set 中覆盖不足 |
| Process reward model | AgentPRM (Xi et al., 2025)、ToolPRMBench (2026)、CM2 (2026)、SWiRL (2025) | Q(s,a)、step labels、checklist、correct-vs-plausible-wrong | 长链 tool-use、PRM-guided search | PRM hacking、distribution shift、label cost |
| SFT / data pipeline / action model | Toolformer (Schick et al., 2023)、ToolLLM/ToolBench (Qin et al., 2023)、xLAM (Liu et al., 2024)、ToolACE (Liu et al., 2024)、APIGen (Liu et al., 2024)、APIGen-MT (Prabhakar et al., 2025) | likelihood filtering、data verification、function-call benchmark accuracy | tool-use data construction、function calling | 多数不是 RL training，不能误写成 reward optimization |
| Benchmark-only / evaluation | API-Bank (Li et al., 2023)、AgentBench (Liu et al., 2023)、WebArena (Zhou et al., 2023)、SWE-bench (Jimenez et al., 2023)、tau-bench (Yao et al., 2024)、BFCL (Patil et al., 2025) | API correctness、functional success、unit tests、pass^k、AST/executable metrics | evaluation and oracle design | benchmark metric 只是 reward proxy，不等于部署成功 |

本领域最容易混淆的是“训练方法”和“评估方法”。ReAct、Reflexion、ToolBench、BFCL、tau-bench、SWE-bench 等都对 reward design 很重要，但不应因此被写成 RL 训练论文。BFCL 的稳定 id 是 `bfcl_2024`，但 canonical paper 是 Patil et al. (2025) 的 OpenReview / ICML 2025 版本。

## 第 2 部分：奖励函数与惩罚函数盘点

| Reward 类型 | 公式 / 伪代码 | 正向信号 | 负向信号 | 代表工作 | 典型失败 |
|---|---|---|---|---|---|
| final correctness | `R = 1[answer == gold]`、`R = 1[unit_tests_pass]`、`R = 1[db_state_ok] * 1[user_output_ok]` | 最终答案、数据库终态、测试通过 | 错答案、测试失败、任务失败 | Search-R1、ReTool、SWE-bench、tau-bench、MUA-RL | 稀疏，不能定位中间错误 |
| format / schema compliance | `R_schema = 1[valid_json && required_fields && type_ok]` | JSON/AST/schema 合法 | 解析失败、缺字段、类型错 | ToolRL、APIGen、R1-Searcher、BFCL | 学会外壳，不学语义 |
| execution-based reward | `R_exec = 1[tool_runs && no_runtime_error]` | API/code/tool 成功运行 | runtime error、invalid call | APIGen、ReTool、SWE-bench、WebArena、tau-bench | 执行成功不等于用户目标成功 |
| step / process reward | `R_t = verifier(s_t,a_t,o_t)`、`Q(s,a)=E[sum gamma^k r]` | 当前动作推进子目标 | 当前动作误导轨迹 | AgentPRM、ToolPRMBench、CM2、SWiRL | verifier 噪声、PRM hacking |
| efficiency / cost penalty | `R = R_task - lambda_steps*n_steps - lambda_api*n_calls` | 少量必要调用 | 冗余调用、过度搜索 | R1-Searcher、Turn-Level Credit Assignment、tau-bench | 惩罚过强会鼓励过早停止 |
| safety / policy compliance | `R_safe = 1[policy_ok] - 1[forbidden_call]` | 拒绝危险调用、遵守 domain policy | 越权写入、缺确认、违规操作 | tau-bench、APIGen-MT、BFCL、MUA-RL | 真实 harmful API reward 覆盖不足 |
| KL / regularization | `J = E[R] - beta * D_KL(pi, pi_ref)` | 策略改善但不剧烈漂移 | reward hacking、policy drift | AgentPRM、DMPO、MUA-RL | KL 是训练约束，不是任务 reward |
| preference / contrastive | DPO-style loss 或 `R = 1[rank(correct) > rank(wrong)]` | preferred trajectory、correct action | rejected trajectory、plausible wrong action | DMPO、EPO、ToolPRMBench、RLAIF API-code | preference 噪声和局部 credit 不足 |

最强的设计原则来自 APIGen：format check 之后还需要 actual execution，execution 之后还需要 semantic verification。ToolRL 把 `R_format` 与 correctness 拆开也说明 format reward 不能独自承担训练目标。tau-bench 的 `r = r_action * r_output` 进一步说明 API transaction 至少要同时看外部状态和用户可见输出。

## 第 3 部分：级联漂移

“级联漂移”不是普通 hallucination 的同义词。它指 agent 在多步工具交互中，由早期错误、遗漏、误读或状态污染引发后续决策持续偏离，而每一步表面上仍可能格式正确、局部合理、甚至工具执行成功。

| 机制 | 例子 | 为什么 terminal reward 不够 | 可用 reward / verifier |
|---|---|---|---|
| early wrong API call contaminates context | 先查错用户，再对错订单退款 | 最终失败不能告诉是哪一步错 | entity verifier、first-error penalty |
| stale memory / stale observation | 用户已改地址，agent 继续用旧地址 | API/cache success 仍可能过期 | freshness check、state consistency reward |
| tool result misread | 检索到否定证据，却写成肯定结论 | 检索执行成功掩盖证据误读 | citation-to-claim consistency |
| subgoal decomposition wrong | 先执行动作，后确认资格 | 每个 executor step 可能局部正确 | checklist dependency reward |
| irreversible external action | 未确认就删除、付款、发邮件 | 文本解释无法恢复真实状态 | two-phase commit、postcondition verification |
| retry loop amplifies error | 重复相似 query 或 API call | 最终 0/1 不说明浪费和污染 | redundant-call penalty、stop-and-ask reward |

Prompt 7 补充源给出了长程 credit 的几条路线：HiPER (Peng et al., 2026) 用 planner/executor 层级 credit；ELPO (Liang et al., 2026) 定位 first irrecoverable step；HCAPO (Tan et al., 2026) 用 hindsight critic 修正 step-level Q；GiGPO (Feng et al., 2025) 用 anchor-state group 做 micro advantage；ReBel (Tang et al., 2026) 奖励 belief consistency；UProp (Duan et al., 2025) 估计继承而来的不确定性。覆盖限制是：这些工作多在 ALFWorld、WebShop、search 或 tool-integrated reasoning 中验证，不等于成熟的生产 API side-effect reward。

## 第 4 部分：静默错误

静默错误的危险在于它能通过表层检查。典型形式包括：

| 错误类型 | 表面上为什么像对的 | 实际错在哪里 | 需要的 oracle |
|---|---|---|---|
| JSON valid but wrong argument | 字段和类型都合法 | 参数值不满足用户意图 | 参数语义 checker |
| correct endpoint but wrong entity | API 返回成功 | 用户、订单、文件或时间窗口错 | 实体解析和跨工具一致性 |
| correct retrieval but irrelevant evidence | 文档存在，格式正确 | 证据不支持结论 | evidence-to-claim entailment |
| tool returns success but business goal fails | `success=true` 或 HTTP `200` | 业务终态或 policy prerequisite 错 | DB snapshot、policy invariant |
| unit tests pass but hidden requirement fails | 可见测试全绿 | 隐藏需求未覆盖 | hidden tests、人工 review |
| hallucinated tool result | 文本像工具返回 | 没有真实调用或篡改结果 | append-only tool log |
| stale/cached result treated as current | 缓存命中 | 状态已经变化 | freshness oracle |
| unsafe action formatted correctly | schema 合法 | 缺权限、确认或安全边界 | two-phase commit、human confirmation |

AgentHallu (Liu et al., 2026) 支持把 hallucination 归因到多步 agent 轨迹中的负责步骤；Internal Tool Hallucination (Healy et al., 2026) 把错误工具选择、坏参数和 tool bypass 作为检测对象；Butterfly Toolchains (Xiong et al., 2025) 说明参数填充失败会在工具链中放大；ToolGate (Liu et al., 2026) 提供 precondition / postcondition / verified state commit 的运行时验证方向；Reasoning Trap (Yin et al., 2026) 提醒 reasoning enhancement 可能放大 no-tool / distractor-tool 场景中的工具幻觉。

静默错误的 reward 设计不应只奖励“工具跑通”。更稳的模板是：

```text
if R_semantic == 0:
  R_total = min(R_format + R_exec, cap_surface_only) - penalty_false_success
else:
  R_total = R_format + R_exec + w_sem * R_semantic
```

## 第 5 部分：调参技巧与工程实践

工程上必须把 reward component 和训练健康指标分开看。最小 dashboard：

| Metric | 为什么记录 |
|---|---|
| final success | 任务终局是否完成 |
| schema pass | 格式层是否稳定 |
| semantic pass | 是否满足用户意图和领域约束 |
| tool error rate / invalid call rate | 工具和参数层是否退化 |
| avg steps / redundant calls | 是否过度调用或过早停止 |
| judge disagreement | LLM judge / PRM 是否稳定 |
| KL / entropy | policy drift 和 mode collapse |
| reward variance | GRPO/PPO 是否有可学信号 |
| false-success rate | 执行成功但语义失败比例 |
| unsafe commit rate | 高风险写操作是否缺确认 |
| pass^k | 多次试验是否稳定可靠 |

常见症状与修复：

| 症状 | 可能原因 | 修复 |
|---|---|---|
| schema pass 升、semantic pass 不动 | format reward 主导 | 降低 format 权重；语义失败 cap 总分 |
| reward 快涨、真实成功率不涨 | KL 太低或 judge 被钻空子 | 提高 KL；加 held-out judge calibration |
| all rewards identical | group size 或任务难度不合适 | 增加 rollout、分桶采样、加 step verifier |
| invalid call rate 上升 | LR 太高、parser 宽松 | 降 LR、strict schema、SFT refresh |
| avg calls 上升但成功率不升 | 奖励了 tool frequency | 奖励 utility，惩罚 redundant calls |
| early stopping 增加 | cost/length penalty 过强 | stop-when-done verifier，必要步骤 checklist |

最小训练前 sanity checks 包括 parser smoke test、reward component unit tests、golden trace replay、plausible-wrong negative set、tiny-batch overfit、group reward variance check、judge calibration set、hidden-test dry run、irreversible-action gate 和 offline-to-online canary。

## 第 6 部分：Gap / 反模式 / 大家踩过的坑

| 反模式 | 为什么吸引人 | 真实风险 | 更好的实验 / reward |
|---|---|---|---|
| pure outcome reward | 客观便宜 | 长轨迹不知道错在哪 | final + step verifier + first-error penalty |
| format reward dominating semantic reward | 容易自动打分 | 学会模板，不学目标 | surface reward cap + plausible-wrong negatives |
| LLM judge without calibration | 覆盖开放语义 | judge pleasing / judge hacking | calibration set + raw tool log + disagreement metric |
| execution success mistaken as task success | 工具确实跑了 | 错实体、错业务状态、隐藏需求失败 | DB oracle + hidden tests + semantic checker |
| static benchmark overfitting | 排行榜清晰 | 线上动态环境不涨 | hidden split + live/snapshot eval |
| rewarding tool frequency | 鼓励主动查证 | API spam、成本上升 | reward useful call, not call count |
| ignoring irreversible actions | sandbox 简化 | 真实副作用无法文本修复 | two-phase commit + undo log |
| treating all tool errors equally | 实现简单 | retry 策略错误 | error taxonomy + typed recovery reward |

最有论文价值的 gap：

1. **schema-valid but semantically wrong function calls**：构造错实体、错时间、错 policy 的 hard negatives。
2. **rollback / recovery reward**：训练 `detect -> rollback -> retry`，而不是只惩罚失败。
3. **calibrated abstention / no-tool decision**：奖励高不确定场景中的 ask / abstain。
4. **side-effect safety for real APIs**：写操作必须有 precondition、confirmation、postcondition。
5. **dynamic environment reward**：把 API version、freshness、cache 和 live state 纳入评价。

覆盖不足：KTO、SimPO、RLOO 在当前 source set 中没有强 tool-use 核心证据；token/API cost reward 和真实 harmful-call refusal reward 也仍然偏薄。

## 第 7 部分：可借鉴的相邻领域

| 相邻领域 | 已有 reward idea | agentic tool-use 可迁移版本 | 为什么还没充分迁移 |
|---|---|---|---|
| classical RL reward shaping | potential-based shaping、curriculum、exploration bonus | 用 API workflow predicates 表示中间进展 | LLM agent state 多是文本和外部状态，potential 难验证 |
| HER / goal relabeling | failed trajectory reuse、subgoal relabeling | 从失败轨迹中学习缺失 precondition 或已完成子目标 | 真实用户目标不能随意重标，错实体不能当成功 |
| robotics long-horizon manipulation | sparse-to-dense predicates、recovery policy、irreversible action handling | entity identified、permission checked、state committed、rollback succeeded | API side effect 和业务 policy 比物理谓词更开放 |
| code generation RL | unit tests、hidden tests、repair loop | tool trace replay、hidden policy tests、mutation tests | unit tests 不覆盖全部业务语义 |
| math reasoning PRM | step verifier、process supervision | 查询必要性、参数证据、observation 读取是否正确 | API 语义比数学步骤更依赖外部上下文 |
| formal methods / program synthesis | invariant、contract、differential testing | ToolGate-style pre/postconditions，cross-tool consistency | invariant 写不全，多工具一致也可能共同错误 |

这些只是迁移假设，不是已在 agentic tool-use 中充分证明的结论。最可落地的路线不是直接搬公式，而是把相邻领域想法转成 verifier、contract、hidden test、rollback protocol 和 calibrated process reward。

## References

1. Schick et al. (2023). Toolformer: Language Models Can Teach Themselves to Use Tools. https://arxiv.org/abs/2302.04761
2. Yao et al. (2022). ReAct: Synergizing Reasoning and Acting in Language Models. https://arxiv.org/abs/2210.03629
3. Shinn et al. (2023). Reflexion: Language Agents with Verbal Reinforcement Learning. https://arxiv.org/abs/2303.11366
4. Qin et al. (2023). ToolLLM: Facilitating Large Language Models to Master 16000+ Real-world APIs. https://arxiv.org/abs/2307.16789
5. Li et al. (2023). API-Bank: A Comprehensive Benchmark for Tool-Augmented LLMs. https://arxiv.org/abs/2304.08244
6. Liu et al. (2023). AgentBench: Evaluating LLMs as Agents. https://arxiv.org/abs/2308.03688
7. Zhou et al. (2023). WebArena: A Realistic Web Environment for Building Autonomous Agents. https://arxiv.org/abs/2307.13854
8. Jimenez et al. (2023). SWE-bench: Can Language Models Resolve Real-World GitHub Issues?. https://arxiv.org/abs/2310.06770
9. Yao et al. (2024). tau-bench: A Benchmark for Tool-Agent-User Interaction in Real-World Domains. https://arxiv.org/abs/2406.12045
10. Gou et al. (2023). ToRA: A Tool-Integrated Reasoning Agent for Mathematical Problem Solving. https://arxiv.org/abs/2309.17452
11. Zeng et al. (2023). AgentTuning: Enabling Generalized Agent Abilities for LLMs. https://arxiv.org/abs/2310.12823
12. Chen et al. (2023). FireAct: Toward Language Agent Fine-tuning. https://arxiv.org/abs/2310.05915
13. Liu et al. (2024). xLAM: A Family of Large Action Models to Empower AI Agent Systems. https://arxiv.org/abs/2409.03215
14. Liu et al. (2024). ToolACE: Winning the Points of LLM Function Calling. https://arxiv.org/abs/2409.00920
15. Liu et al. (2024). APIGen: Automated Pipeline for Generating Verifiable and Diverse Function-Calling Datasets. https://arxiv.org/abs/2406.18518
16. Prabhakar et al. (2025). APIGen-MT: Agentic Pipeline for Multi-Turn Data Generation via Simulated Agent-Human Interplay. https://arxiv.org/abs/2504.03601
17. Feng et al. (2025). ReTool: Reinforcement Learning for Strategic Tool Use in LLMs. https://arxiv.org/abs/2504.11536
18. Singh et al. (2025). Agentic Reasoning and Tool Integration for LLMs via Reinforcement Learning. https://arxiv.org/abs/2505.01441
19. Zhang et al. (2025). RAGEN: Understanding Self-Evolution in LLM Agents via Multi-Turn Reinforcement Learning. https://arxiv.org/abs/2504.20073
20. Jin et al. (2025). Search-R1: Training LLMs to Reason and Leverage Search Engines with Reinforcement Learning. https://arxiv.org/abs/2503.09516
21. Liu et al. (2025). ToolRL: Reward is All Tool Learning Needs. https://arxiv.org/abs/2504.13958
22. Patil et al. (2025). The Berkeley Function Calling Leaderboard (BFCL): From Tool Use to Agentic Evaluation of Large Language Models. https://openreview.net/forum?id=2GmDdhBdDk
23. Xi et al. (2025). Process Reward Models for LLM Agents: Practical Framework and Directions. https://arxiv.org/abs/2502.10325
24. Chen et al. (2024). Direct Multi-Turn Preference Optimization for Language Agents. https://arxiv.org/abs/2406.14868
25. Zhao et al. (2024). EPO: Hierarchical LLM Agents with Environment Preference Optimization. https://arxiv.org/abs/2408.16090
26. unknown (2024). Applying RLAIF for Code Generation with API-usage in Lightweight LLMs. https://arxiv.org/abs/2406.20060
27. unknown (2026). CM2: Reinforcement Learning with Checklist Rewards for Multi-Turn and Multi-Step Agentic Tool Use. https://arxiv.org/abs/2602.12268
28. unknown (2025). MUA-RL: Multi-turn User-interacting Agent Reinforcement Learning for agentic tool use. https://arxiv.org/abs/2508.18669
29. unknown (2025). Reinforcing Multi-Turn Reasoning in LLM Agents via Turn-Level Credit Assignment. https://arxiv.org/abs/2505.11821
30. unknown (2025). Training Task Reasoning LLM Agents for Multi-turn Task Planning via Single-turn Reinforcement Learning. https://arxiv.org/abs/2509.20616
31. unknown (2026). ToolPRMBench: Evaluating and Advancing Process Reward Models for Tool-using Agents. https://arxiv.org/abs/2601.12294
32. unknown (2025). R1-Searcher: Incentivizing the Search Capability in LLMs via Reinforcement Learning. https://arxiv.org/abs/2503.05592
33. unknown (2025). Synthetic Data Generation and Multi-Step RL for Reasoning and Tool Use. https://arxiv.org/abs/2504.04736
34. Duan et al. (2025). UProp: Investigating the Uncertainty Propagation of LLMs in Multi-Step Agentic Decision-Making. https://arxiv.org/abs/2506.17419
35. Peng et al. (2026). HiPER: Hierarchical Reinforcement Learning with Explicit Credit Assignment for Large Language Model Agents. https://arxiv.org/abs/2602.16165
36. Liang et al. (2026). Learning from the Irrecoverable: Error-Localized Policy Optimization for Tool-Integrated LLM Reasoning. https://arxiv.org/abs/2602.09598
37. Tan et al. (2026). Hindsight Credit Assignment for Long-Horizon LLM Agents. https://arxiv.org/abs/2603.08754
38. Feng et al. (2025). Group-in-Group Policy Optimization for LLM Agent Training. https://arxiv.org/abs/2505.10978
39. Tang et al. (2026). Rewarding Beliefs, Not Actions: Consistency-Guided Credit Assignment for Long-Horizon Agents. https://arxiv.org/abs/2605.20061
40. Liu et al. (2026). AgentHallu: Benchmarking Automated Hallucination Attribution of LLM-based Agents. https://arxiv.org/abs/2601.06818
41. Liu et al. (2026). ToolGate: Contract-Grounded and Verified Tool Execution for LLMs. https://arxiv.org/abs/2601.04688
42. Healy et al. (2026). Internal Representations as Indicators of Hallucinations in Agent Tool Selection. https://arxiv.org/abs/2601.05214
43. Xiong et al. (2025). Butterfly Effects in Toolchains: A Comprehensive Analysis of Failed Parameter Filling in LLM Tool-Agent Systems. https://arxiv.org/abs/2507.15296
44. Yin et al. (2026). The Reasoning Trap: How Enhancing LLM Reasoning Amplifies Tool Hallucination. https://arxiv.org/abs/2510.22977
