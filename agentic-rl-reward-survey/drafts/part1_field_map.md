# 第 1 部分：领域地图

## 1.1 按训练范式分类

| 训练范式 | 代表工作 | 核心做法 | reward/optimization 信号 | 适用任务 | 局限 |
|---|---|---|---|---|---|
| PPO / GRPO / REINFORCE 类端到端 RL | [Search-R1](https://arxiv.org/abs/2503.09516), [R1-Searcher](https://arxiv.org/abs/2503.05592), [ReTool](https://arxiv.org/abs/2504.11536), [ARTIST](https://arxiv.org/abs/2505.01441), [RAGEN](https://arxiv.org/abs/2504.20073), [ToolRL](https://arxiv.org/abs/2504.13958), [MUA-RL](https://arxiv.org/abs/2508.18669) | 在 rollout 中接入 search、code interpreter、API/database 或模拟用户，用 RL 优化工具调用策略。 | 多为 final answer/task outcome；ToolRL 加入 format/correctness 分解；MUA-RL 用二值 task completion。 | 搜索问答、代码/数学工具使用、多轮用户交互、通用 tool learning。 | outcome reward 稀疏，长链 credit assignment 难；GRPO 在搜索场景可能不稳定；格式奖励容易带来 proxy overfitting。 |
| Turn-level / dense-credit RL | [Reinforcing Multi-Turn Reasoning via Turn-Level Credit Assignment](https://arxiv.org/abs/2505.11821), [CM2](https://arxiv.org/abs/2602.12268), [Training Task Reasoning LLM Agents via Single-turn RL](https://arxiv.org/abs/2509.20616) | 把长轨迹拆成 turn、step 或专家轨迹上的单步决策，给更细粒度的 advantage / reward。 | turn-level exact match、retrieval correctness、format reward、checklist reward、专家轨迹 verifiable reward。 | multi-turn search、multi-step tool use、任务规划。 | dense signals 更有信息量，但 judge noise、采样成本和专家轨迹依赖仍然存在。 |
| DPO / preference optimization | [Direct Multi-Turn Preference Optimization](https://arxiv.org/abs/2406.14868), [EPO](https://arxiv.org/abs/2408.16090) | 把 agent trajectory 或环境反馈排序转成 chosen/rejected 数据，直接优化偏好目标。 | trajectory-level preference pairs；环境反馈 reward model 排名。 | 多轮 language agents、embodied / environment-feedback agents。 | KTO、SimPO 在本 source set 中只有相邻引用，没有核心 tool-use source，故此处覆盖不足。 |
| Process reward model | [AgentPRM](https://arxiv.org/abs/2502.10325), [ToolPRMBench](https://arxiv.org/abs/2601.12294), [CM2](https://arxiv.org/abs/2602.12268) | 用 PRM / checklist 给中间 action 或 criterion 打分，辅助训练或 reward-guided search。 | Q(s,a) 式 process reward、correct-vs-plausible-wrong step labels、binary checklist criteria。 | 长链 tool-using agents、ALFWorld / API tool trajectories、多轮工具任务。 | PRM 容易 reward hacking；step labels 需要验证；LLM judge/checklist 的稳定性仍是瓶颈。 |
| Reward shaping / verifier shaping | [ToolRL](https://arxiv.org/abs/2504.13958), [CM2](https://arxiv.org/abs/2602.12268), [APIGen](https://arxiv.org/abs/2406.18518), [APIGen-MT](https://arxiv.org/abs/2504.03601) | 把工具调用拆成格式、函数名、参数、执行、语义或 checklist 条件。 | format check、execution check、semantic checker、tool-name/parameter matching、checklist pass/fail。 | function calling 数据生成、tool learning、multi-turn data synthesis。 | 容易把 syntax correctness 当成 semantic success；LLM semantic checker 质量会限制上限。 |
| Curriculum / synthetic data pipeline | [Toolformer](https://arxiv.org/abs/2302.04761), [ToolACE](https://arxiv.org/abs/2409.00920), [APIGen](https://arxiv.org/abs/2406.18518), [APIGen-MT](https://arxiv.org/abs/2504.03601), [SWiRL](https://arxiv.org/abs/2504.04736) | 先构造或筛选工具使用数据，再用 SFT 或 offline step-wise RL 学习。 | likelihood improvement、verification filters、process-filtered synthetic trajectories、generative reward model。 | function calling、multi-turn API use、search/calculator tool use。 | 真正 curriculum 设计证据仍不足；多数工作是数据 pipeline，而非显式课程奖励。 |
| Inference-time optimization / self-reflection / search | [ReAct](https://arxiv.org/abs/2210.03629), [Reflexion](https://arxiv.org/abs/2303.11366), [ToRA](https://arxiv.org/abs/2309.17452) | 在推理时交替 reasoning/action/observation，或把反馈写入 reflection memory。 | 环境 observation、task feedback、verbal feedback；通常不更新模型权重。 | QA、决策环境、代码/数学工具推理。 | 不能直接当成 RL training；错误 reflection 或早期错误 action 会沿轨迹传播。 |
| SFT / agent instruction tuning / action-model training | [ToolLLM/ToolBench](https://arxiv.org/abs/2307.16789), [AgentTuning](https://arxiv.org/abs/2310.12823), [FireAct](https://arxiv.org/abs/2310.05915), [xLAM](https://arxiv.org/abs/2409.03215), [ToolACE](https://arxiv.org/abs/2409.00920), [ToRA](https://arxiv.org/abs/2309.17452) | 用 tool-use / agent trajectories 做监督微调或 action model 训练。 | benchmark accuracy、ToolEval、QA/math correctness、function-calling score。 | API use、search QA、function calling、数学工具推理。 | 主要学习 imitation；缺少 online exploration 和失败恢复的 reward signal。 |
| Benchmark-only / evaluation framework | [API-Bank](https://arxiv.org/abs/2304.08244), [AgentBench](https://arxiv.org/abs/2308.03688), [WebArena](https://arxiv.org/abs/2307.13854), [SWE-bench](https://arxiv.org/abs/2310.06770), [tau-bench](https://arxiv.org/abs/2406.12045), [BFCL](https://openreview.net/forum?id=2GmDdhBdDk), [ToolPRMBench](https://arxiv.org/abs/2601.12294) | 定义任务、环境和 metric，用于评估 tool-use / agent 能力。 | API correctness、environment success、unit tests、pass^k、AST matching、step-level PRM accuracy。 | benchmark taxonomy、metric design、failure analysis。 | 不能写成训练方法；benchmark metric 可能只覆盖 reward 的一部分代理目标。 |

## 1.2 按奖励信号来源分类

| 奖励信号来源 | 形式 | 代表工作 | 优点 | 失败模式 |
|---|---|---|---|---|
| outcome-based | final answer correctness、task completion、database final state、unit tests | [ReTool](https://arxiv.org/abs/2504.11536), [ARTIST](https://arxiv.org/abs/2505.01441), [Search-R1](https://arxiv.org/abs/2503.09516), [MUA-RL](https://arxiv.org/abs/2508.18669), [SWE-bench](https://arxiv.org/abs/2310.06770), [tau-bench](https://arxiv.org/abs/2406.12045) | 客观、便宜、容易规模化；适合有明确终局的任务。 | sparse reward；早期错误无法定位；测试通过不一定等价于语义正确。 |
| process-based | Q(s,a)、turn-level reward、step-level labels、checklist criteria | [AgentPRM](https://arxiv.org/abs/2502.10325), [ToolPRMBench](https://arxiv.org/abs/2601.12294), [CM2](https://arxiv.org/abs/2602.12268), [Turn-Level Credit Assignment](https://arxiv.org/abs/2505.11821), [SWiRL](https://arxiv.org/abs/2504.04736) | 更利于 credit assignment；能在长轨迹中提前纠错。 | PRM / judge noise；reward hacking；标注和验证成本高。 |
| execution-based | API execution、code interpreter、database state transition、unit tests | [APIGen](https://arxiv.org/abs/2406.18518), [ReTool](https://arxiv.org/abs/2504.11536), [SWE-bench](https://arxiv.org/abs/2310.06770), [WebArena](https://arxiv.org/abs/2307.13854), [tau-bench](https://arxiv.org/abs/2406.12045), [APIGen-MT](https://arxiv.org/abs/2504.03601) | 比纯文本匹配更接近真实任务；能发现不可执行调用。 | execution success 仍可能不满足用户意图；环境维护成本高。 |
| LLM-as-judge | ToolEval、semantic checker、checklist evaluator、review committee | [ToolLLM/ToolBench](https://arxiv.org/abs/2307.16789), [APIGen](https://arxiv.org/abs/2406.18518), [APIGen-MT](https://arxiv.org/abs/2504.03601), [CM2](https://arxiv.org/abs/2602.12268), [Turn-Level Credit Assignment](https://arxiv.org/abs/2505.11821), [RLAIF API-code](https://arxiv.org/abs/2406.20060) | 能评价开放式语义、轨迹质量和 checklist criteria。 | judge variance、prompt sensitivity、潜在 judge hacking；强 judge 成本高。 |
| self-consistency / self-reflection | verbal feedback memory、multiple attempts、reflection-guided retry | [Reflexion](https://arxiv.org/abs/2303.11366), [ReAct](https://arxiv.org/abs/2210.03629) | 不需要更新权重，适合快速迭代和调试型任务。 | 反馈弱时会强化错误解释；不能替代训练 reward。 |
| contrastive / preference | trajectory preference、environment-feedback ranking、chosen/rejected step/action | [DMPO](https://arxiv.org/abs/2406.14868), [EPO](https://arxiv.org/abs/2408.16090), [ToolPRMBench](https://arxiv.org/abs/2601.12294), [RLAIF API-code](https://arxiv.org/abs/2406.20060) | 避免手写标量 reward；适合比较复杂轨迹或候选动作。 | 偏好来源可能噪声大；本综述对 KTO / SimPO 的 tool-use evidence 此处覆盖不足。 |

## 1.3 按任务类型分类

| 任务类型 | 代表 benchmark/work | 单轮/多轮 | 是否真实执行 | 主要难点 |
|---|---|---|---|---|
| single-turn function call | [BFCL](https://openreview.net/forum?id=2GmDdhBdDk), [API-Bank](https://arxiv.org/abs/2304.08244), [xLAM](https://arxiv.org/abs/2409.03215), [ToolACE](https://arxiv.org/abs/2409.00920), [ToolRL](https://arxiv.org/abs/2504.13958) | 主要单轮，也含 parallel / multiple calls | 部分真实执行；BFCL 也用 AST proxy | 函数名、参数名、参数值、格式和语义正确性容易被混在一个分数里。 |
| multi-turn tool use | [tau-bench](https://arxiv.org/abs/2406.12045), [MUA-RL](https://arxiv.org/abs/2508.18669), [CM2](https://arxiv.org/abs/2602.12268), [APIGen-MT](https://arxiv.org/abs/2504.03601), [BFCL](https://openreview.net/forum?id=2GmDdhBdDk) | 多轮 | 有 API/database 或模拟 tool execution | 用户意图逐步显露，domain policy 和 tool state 共同决定成功。 |
| web / OS / software engineering agents | [AgentBench](https://arxiv.org/abs/2308.03688), [WebArena](https://arxiv.org/abs/2307.13854), [SWE-bench](https://arxiv.org/abs/2310.06770) | 多轮/长轨迹 | WebArena / SWE-bench 有环境或测试执行 | 状态空间大，终局 reward 稀疏，silent failure 难靠表层动作发现。 |
| search agents | [Search-R1](https://arxiv.org/abs/2503.09516), [R1-Searcher](https://arxiv.org/abs/2503.05592), [Turn-Level Credit Assignment](https://arxiv.org/abs/2505.11821), [FireAct](https://arxiv.org/abs/2310.05915), [ReAct](https://arxiv.org/abs/2210.03629) | 多轮检索/推理 | 搜索引擎或检索器返回外部信息 | query quality 与 final answer 之间 credit assignment 弱；检索噪声会诱发 plausible wrong answer。 |
| code/math execution agents | [ReTool](https://arxiv.org/abs/2504.11536), [ToRA](https://arxiv.org/abs/2309.17452), [SWiRL](https://arxiv.org/abs/2504.04736), [RLAIF API-code](https://arxiv.org/abs/2406.20060), [SWE-bench](https://arxiv.org/abs/2310.06770) | 从单题到长轨迹都有 | code interpreter、calculator、unit tests 或 executability pipeline | execution feedback 强但不完备；测试/答案匹配可能漏掉语义错误。 |
| API transaction / customer-service agents | [tau-bench](https://arxiv.org/abs/2406.12045), [MUA-RL](https://arxiv.org/abs/2508.18669), [APIGen-MT](https://arxiv.org/abs/2504.03601), [CM2](https://arxiv.org/abs/2602.12268) | 多轮 | 数据库 API、MCP server 或 LLM-simulated tools | 用户确认、policy compliance、数据库写入和用户可见答复必须同时正确。 |

## 1.4 代表工作一句话速览

- **Toolformer** — 用 likelihood-based filtering 自动构造 API-call 数据，是 tool-use training 的早期代表，但不是 RL。链接：[paper](https://arxiv.org/abs/2302.04761)。
- **ReAct** — 把 reasoning、action、observation 交织成 inference-time agent trajectory，为后续 reward granularity 提供轨迹框架。链接：[paper](https://arxiv.org/abs/2210.03629)。
- **Reflexion** — 把 scalar/verbal feedback 写入 reflection memory，属于 inference-time verbal feedback，不应误写成 gradient RL。链接：[paper](https://arxiv.org/abs/2303.11366)。
- **ToolLLM / ToolBench** — 构建大规模真实 API tool-use 数据和 ToolEval，核心信号是 pass rate / win rate，而非 RL reward。链接：[paper](https://arxiv.org/abs/2307.16789)。
- **API-Bank** — API-use benchmark，用于区分 API-call correctness 与 task completion，需和 ToolBench / APIBench 命名分开。链接：[paper](https://arxiv.org/abs/2304.08244)。
- **AgentBench** — 多环境 agent benchmark，适合放在 long-horizon evaluation 和 failure taxonomy 中。链接：[paper](https://arxiv.org/abs/2308.03688)。
- **WebArena** — 真实网站环境中的 web-agent benchmark，强调 functional task success。链接：[paper](https://arxiv.org/abs/2307.13854)。
- **SWE-bench** — 用真实 GitHub issue 与 repository tests 评估软件工程 agent，是 execution-based reward/metric 的关键类比。链接：[paper](https://arxiv.org/abs/2310.06770)。
- **tau-bench** — 用 LM-simulated users、API databases 和 domain policy 评估客服式工具 agent，并提出 pass^k reliability metric。链接：[paper](https://arxiv.org/abs/2406.12045)。
- **ToRA** — 工具集成数学推理的 SFT/trajectory baseline，说明 execution feedback 与最终答案正确性如何结合。链接：[paper](https://arxiv.org/abs/2309.17452)。
- **AgentTuning** — agent instruction tuning baseline，用于和 RL / reward-driven methods 区分。链接：[paper](https://arxiv.org/abs/2310.12823)。
- **FireAct** — 搜索 QA agent 的 trajectory fine-tuning 工作，适合作为 Search-R1 等 RL 搜索 agent 的非 RL 对照。链接：[paper](https://arxiv.org/abs/2310.05915)。
- **xLAM** — action model family，代表 function-calling/action-model 训练路线。链接：[paper](https://arxiv.org/abs/2409.03215)。
- **ToolACE** — 自动生成 function-calling 数据，提醒本综述警惕 benchmark-oriented 数据质量与格式过拟合。链接：[paper](https://arxiv.org/abs/2409.00920)。
- **APIGen** — 用 format check、actual execution 和 semantic checker 生成 verifiable function-calling 数据，是三层 reward 分解的强证据。链接：[paper](https://arxiv.org/abs/2406.18518)。
- **APIGen-MT** — 把 APIGen 扩到多轮 human-agent interplay，强调 blueprint、execution check 和 review committee。链接：[paper](https://arxiv.org/abs/2504.03601)。
- **ReTool** — 在代码执行环境中用 outcome RL 学习何时、如何调用工具。链接：[paper](https://arxiv.org/abs/2504.11536)。
- **ARTIST** — 用 outcome-based agentic RL 学习 reasoning、tool integration 和 environment interaction。链接：[paper](https://arxiv.org/abs/2505.01441)。
- **RAGEN** — 提出 StarPO / RAGEN 系统，重点暴露 multi-turn agent RL 的 Echo Trap 和稳定性问题。链接：[paper](https://arxiv.org/abs/2504.20073)。
- **Search-R1** — 用 PPO/GRPO 风格 outcome reward 训练 search-engine tool use，并避免 process reward。链接：[paper](https://arxiv.org/abs/2503.09516)。
- **ToolRL** — 系统研究 tool-use reward design，把 format reward 与 correctness reward 拆开。链接：[paper](https://arxiv.org/abs/2504.13958)。
- **BFCL** — function-calling benchmark，从 AST matching 扩展到 multi-turn 和 agentic evaluation；本仓库用 OpenReview/ICML 2025 版本作 canonical source。链接：[paper](https://openreview.net/forum?id=2GmDdhBdDk)。
- **AgentPRM** — 把 agent PRM 视为 Q(s,a) 式 process reward，并分析 reward hacking 与 reward shaping。链接：[paper](https://arxiv.org/abs/2502.10325)。
- **DMPO** — 把 DPO 扩展到 multi-turn agent trajectories，直接优化 preferred vs dispreferred trajectory。链接：[paper](https://arxiv.org/abs/2406.14868)。
- **EPO** — 用 multimodal environment feedback 训练 reward model，再构造 preference data 优化 hierarchical agents；更偏相邻领域。链接：[paper](https://arxiv.org/abs/2408.16090)。
- **RLAIF API-code** — 用大模型 AI feedback 训练 reward model，改善轻量模型的 API-usage code generation。链接：[paper](https://arxiv.org/abs/2406.20060)。
- **CM2** — 用 evidence-grounded checklist rewards 代替不可得的 verifiable rewards，适合 open-ended multi-turn tool use。链接：[paper](https://arxiv.org/abs/2602.12268)。
- **MUA-RL** — 把 LLM-simulated users 放入 RL loop，并用最终 task completion 的二值 reward 避免直接奖励格式。链接：[paper](https://arxiv.org/abs/2508.18669)。
- **Turn-Level Credit Assignment** — 在 search-agent RL 中显式比较 trajectory-level 与 turn-level reward/advantage。链接：[paper](https://arxiv.org/abs/2505.11821)。
- **Task Reasoning GRPO** — 把 multi-turn task planning 转成 expert-trajectory 上的 single-turn GRPO，提供 credit assignment 的另一条路线。链接：[paper](https://arxiv.org/abs/2509.20616)。
- **ToolPRMBench** — 用 correct action vs plausible wrong action 构造 step-level PRM benchmark，直接服务 tool-using PRM 评估。链接：[paper](https://arxiv.org/abs/2601.12294)。
- **R1-Searcher** — 两阶段 outcome-based RL：先奖励检索调用和格式，再用 F1 answer reward 优化答案。链接：[paper](https://arxiv.org/abs/2503.05592)。
- **SWiRL** — offline step-wise RL，把多步 tool-use trajectory 拆成 action-level sub-trajectory，并用 generative reward model 打分。链接：[paper](https://arxiv.org/abs/2504.04736)。
