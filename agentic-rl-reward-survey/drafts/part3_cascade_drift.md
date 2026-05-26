# 第 3 部分：级联漂移

这里的“级联漂移”不是普通 hallucination 的同义词。普通 hallucination 可以发生在单次回答里；级联漂移特指 **agent 在多步工具交互中，由某个早期错误、遗漏、误读或状态污染引发后续决策持续偏离，而最终轨迹仍可能局部合理、格式正确、甚至工具执行成功**。它对 API/tool-use reward design 的挑战是：terminal reward 常常只能告诉我们“最后错了”，却不能告诉我们“从哪一步开始不可恢复”。

## 3.1 定义对齐

- **error propagation**：一个 step 的错误被后续上下文当成事实继续使用。例如搜索结果误读之后，后续所有 query 和答案都围绕错误实体展开。它是级联漂移的最宽泛说法。
- **compounding error**：早期小偏差随着多轮 action-observation 累积成大偏差。DMPO 明确把 multi-turn agent 中 behavior cloning 的 compounding errors 作为动机之一。
- **trajectory drift**：整条轨迹的目标、状态或策略逐步偏离原任务。它强调“轨迹整体方向变了”，不一定要求某一步可判定为硬错误。
- **state drift**：agent 内部维护的状态和真实环境状态不一致。API transaction、web task、database update 中尤其危险，因为环境状态可能已经被写入或修改。
- **long-horizon credit assignment failure**：延迟奖励无法把成败归因到关键中间动作。Turn-Level Credit Assignment、GiGPO、HCAPO、HiPER、ELPO 都是在不同层次上缓解这个问题。
- **distributional shift across tool calls**：policy 更新、工具返回分布或对话状态变化后，PRM/verifier 面对的 step 分布不再像训练时。AgentPRM 的 PRM distribution shift 和 reward hacking 风险属于这一类。
- **observation contamination**：错误工具结果、过期观察或模型误读被写入上下文、memory、belief state，成为后续动作依据。ReBel 把部分可观测环境中的 belief drift 作为核心问题。
- **irreversible action error**：某个外部动作一旦执行就无法通过后续文本修复，如错误退款、错误删除、错误下单、错误数据库写入。ELPO 的“first irrecoverable step”给了一个可操作的近邻定义。

## 3.2 典型发生机制

| 机制 | 例子 | 为什么 final reward 难发现 | 可观测信号 | 可惩罚点 |
|---|---|---|---|---|
| early wrong API call contaminates later context | 客服 agent 先查错用户，再围绕错误订单执行退款 | 终局只显示任务失败，不能区分是查错用户、选错订单还是退款动作本身错 | tool call 参数与用户约束不一致；后续 action 引用同一个错误实体 | `-1[entity_mismatch]`；关键槽位 verifier；first-error penalty |
| wrong intermediate state but plausible final answer | SWE agent 改了能过局部测试的错误文件，最终解释也很像成功 | unit test 不完整时，final answer 或可见 diff 可能很合理 | replay 中状态转移和 issue requirement 不匹配；hidden tests fail | state invariant check；semantic issue verifier；patch-level replay penalty |
| stale memory / stale observation | 多轮用户已更改偏好，但 agent 继续使用旧地址、旧库存或旧 policy | final reward 只在任务终点失败，早期 memory retrieval 看似合理 | memory timestamp 旧；观察版本落后；新旧用户约束冲突 | `-1[uses_stale_state]`；recency-aware retrieval reward |
| tool result misread | 搜索返回否定证据，agent 摘成肯定结论 | 检索执行成功，final answer 也可能匹配表面关键词 | answer 与 retrieved evidence entailment 不一致 | evidence-grounded verifier；citation-to-claim consistency reward |
| subgoal decomposition wrong | planner 把“先确认资格再办理”拆成“直接办理” | executor 每步都可能格式正确、执行成功，但整体 policy 错 | subgoal 缺少必要 precondition；domain rule 未满足 | checklist dependency reward；subgoal-precondition penalty |
| irreversible external action | 未确认就提交退款、删除记录、发送邮件 | 后续道歉或解释不能恢复真实世界状态 | action type 属于 write/commit；缺少 confirmation evidence | `-1[unsafe_commit_without_confirmation]`；two-phase commit reward |
| retry loop amplifies error | 查询失败后不断换相似 query，重复 API call 增加成本和噪声 | 最终失败只给 0，无法说明浪费预算还是信息不足 | repeated low-novelty calls；same error code repeated | redundant-call penalty；novelty threshold；stop-and-ask reward |
| search/query drift | 搜索 agent 从原问题漂到相邻实体或相邻日期 | final answer 可能包含真实信息，但不是原问题答案 | query terms 与 original intent overlap 下降；retrieved docs topic shift | query-intent alignment reward；retrieval relevance penalty |

## 3.3 检测方法

| 方法 | 检测什么 | 可用信号 | 局限 |
|---|---|---|---|
| trace comparison | 同一任务多条轨迹在哪一步开始分叉，成功轨迹和失败轨迹差异在哪里 | winning vs losing trajectories、action diff、state diff | 需要多 rollout；差异不一定因果 |
| step-level verifier | 每个 action 是否满足局部约束、工具参数是否语义正确 | rule checker、retrieval checker、LLM judge、ToolPRMBench-style correct-vs-wrong pairs | verifier 噪声会变成 reward 噪声；open-ended API 语义难全覆盖 |
| process reward model | 估计 `Q(s,a)` 或 step usefulness，而不只看最终成败 | AgentPRM、CM2 checklist、SWiRL generative reward | PRM 可能被 reward hacking；distribution shift 下可靠性下降 |
| state consistency check | agent belief / scratchpad / tool observation 是否与真实环境状态一致 | database snapshot、web DOM、retrieved evidence、belief state | 需要环境可重放或可查询；真实 API 写入不总是可逆 |
| cross-trajectory self-consistency | 多条轨迹对同一中间状态或答案是否一致 | repeated anchor states、grouped rollouts、majority / variance | 一致也可能共同错；不适合一次性高成本 API 调用 |
| rollback / replay | 把轨迹放回环境或模拟器，观察关键 action 是否可恢复 | simulator、sandbox、event log、checkpoint | 真实外部工具常缺少 rollback；模拟器可能绕开真实 side effect |
| oracle intermediate state | 用人工标注、专家轨迹或任务 blueprint 给中间状态答案 | expert trajectory、unit tests、APIGen-MT blueprint、Task Reasoning GRPO | 昂贵；可能只覆盖 benchmark，不覆盖真实长尾 |
| invariant checking | 域约束永远不能被破坏，如“退款前必须确认资格” | policy unit tests、schema invariants、database constraints | invariant 写不全时会漏掉 silent semantic error |

UProp 给了一个有用补充：不要只估当前 step 的不确定性，还要估“从前面继承来的不确定性”。对级联漂移来说，这相当于给污染风险加一条早期预警线。

## 3.4 奖励设计

- **Dense step reward**

```text
R_total = R_final + alpha * sum_t verifier(s_t, a_t, o_t)
```

适合有 step verifier、retrieval checker、checklist criteria 的工具任务。CM2、ToolPRMBench、Turn-Level Credit Assignment 和 SWiRL 都在不同形式上使用这类细粒度信号。风险是 verifier 噪声被训练过程放大。

- **Discounted trajectory reward**

```text
Q(s_t, a_t) = E[sum_{k=t..T} gamma^(k-t) * r_k]
R_t = Q_hat(s_t, a_t)
```

适合 AgentPRM / HCAPO 式的过程价值估计。它能把终局 reward 回传到中间动作，但如果 `Q_hat` 是模型估计，仍会出现 hindsight rationalization 或 PRM hacking。

- **Explicit intermediate error penalty**

```text
if first_irrecoverable_step == t:
  R_t = -1
  R_suffix = R_suffix - lambda
```

ELPO 的方向最贴近这个设计：先定位 first irrecoverable step，再强化关键 step 和后缀的 corrective update。API 场景里，这可对应“第一次选错实体”“第一次越权写入”“第一次污染 belief state”。

- **Backtracking / reflection reward**

```text
R_backtrack = 1[detect_error && rollback_to_valid_state && new_path_succeeds]
R_false_reflect = -1[reflection_preserves_wrong_premise]
```

Reflexion 支撑“失败反馈写入 reflection memory”的思路，但系统化 rollback reward 在当前 source set 覆盖不足。对真实 API agent，必须区分文本反思和环境级回滚：能承认错误，不等于外部状态已经恢复。

- **Subgoal decomposition reward**

```text
R_subgoal = sum_j w_j * 1[subgoal_j_done && preconditions_j_met]
R_plan = -1[missing_required_subgoal]
```

HiPER、Task Reasoning GRPO、CM2 checklist 和 APIGen-MT blueprint 都给了可借鉴路径：先把任务拆成有依赖的子目标，再奖励满足依赖的进展。危险点是错误 decomposition 会把后续 executor 带到错误任务。

- **State-consistency reward**

```text
R_state = 1[belief_state matches env_state] - 1[belief_observation_mismatch]
```

ReBel 直接把 belief-observation discrepancy 变成 dense self-supervised signal。对 API transaction，类似设计可以检查 agent 内部 slots 是否和数据库、用户最新约束、工具返回一致。

- **Uncertainty-aware penalty**

```text
R = R_task - lambda_u * propagated_uncertainty(trace_prefix)
if uncertainty_high:
  reward(ask_clarifying_question or verify_before_commit)
```

UProp 不是 reward paper，但它提示：高风险 step 的惩罚不应只看当前 token entropy，还要看历史决策继承来的 uncertainty。适合 irreversible action 前的“先确认再提交”。

## 3.5 代表工作如何处理

| 工作 | 场景 | drift 处理方式 | reward / verification 机制 | 局限 |
|---|---|---|---|---|
| [ReAct](https://arxiv.org/abs/2210.03629) | reasoning + acting inference-time agent | 通过 action-observation 显式暴露中间状态 | 环境 observation 和任务成功评价 | 不训练 reward；错误 observation 仍会沿轨迹传播 |
| [Reflexion](https://arxiv.org/abs/2303.11366) | 多次尝试、verbal feedback | 失败后写入 reflection memory | scalar/verbal feedback | reflection 可能固化错误解释；不是环境 rollback |
| [tau-bench](https://arxiv.org/abs/2406.12045) | 多轮 API transaction | 最终数据库状态和用户输出共同判定成功 | `r_action * r_output`，pass^k | sparse terminal reward，难定位哪个 API call 导致失败 |
| [WebArena](https://arxiv.org/abs/2307.13854) / [SWE-bench](https://arxiv.org/abs/2310.06770) | web / software engineering agent | 通过真实环境或测试暴露终局失败 | functional success、unit tests | 终局稀疏；测试/网页状态可能漏掉语义 drift |
| [ToolRL](https://arxiv.org/abs/2504.13958) | tool selection and application | 把工具调用拆成 format 和 correctness | format reward + tool-name/parameter matching | 更偏 call-level；长程 state drift 覆盖有限 |
| [Search-R1](https://arxiv.org/abs/2503.09516) / [R1-Searcher](https://arxiv.org/abs/2503.05592) | search-agent RL | 通过 RL 学搜索行为，部分使用 retrieval/format/answer reward | final correctness、retrieval invocation、format/F1 reward | query drift 和 evidence misread 仍难用 final answer 定位 |
| [Turn-Level Credit Assignment](https://arxiv.org/abs/2505.11821) | multi-turn reasoning/search RL | 直接比较 trajectory-level 和 turn-level credit | turn-level retrieval、format、search-count、LLM judge reward | LLM judge 噪声；一般 K-turn 情况采样复杂度高 |
| [AgentPRM](https://arxiv.org/abs/2502.10325) | agent process reward modeling | 把 PRM 解释为 `Q(s,a)`，为中间动作估值 | Monte Carlo rollout target + KL-regularized policy update | PRM score 可被 hacking；policy shift 下 PRM 失真 |
| [ToolPRMBench](https://arxiv.org/abs/2601.12294) | tool-using PRM evaluation | 用 correct action vs plausible wrong action 测 step-level 判断 | binary PRM accuracy，多 LLM 验证过滤 | 主要是 benchmark；弱 PRM 可能在 reward-guided search 中放大错轨迹 |
| [CM2](https://arxiv.org/abs/2602.12268) | open-ended multi-turn tool use | checklist criteria 把任务进展拆成 evidence-grounded 条件 | binary checklist reward + LLM-as-judge | dense assignment 会放大 judge noise；checklist 覆盖不全会漏 drift |
| [SWiRL](https://arxiv.org/abs/2504.04736) | synthetic multi-step reasoning/tool use | step-wise generative reward model 评价 action/sub-trajectory | generative reward per step | synthetic data 和 reward model 质量决定上限 |
| [GiGPO](https://arxiv.org/abs/2505.10978) | ALFWorld/WebShop/search QA agent RL | anchor state grouping 产生 micro step advantage | macro trajectory advantage + micro step advantage | 可比状态识别错误会制造伪 credit |
| [HCAPO](https://arxiv.org/abs/2603.08754) | long-horizon WebShop/ALFWorld agents | hindsight critic 修正 step-level Q，补足 intermediate baseline | post-hoc critic + multi-scale advantage | hindsight critic 可能合理化错误步骤 |
| [ELPO](https://arxiv.org/abs/2602.09598) | tool-integrated reasoning | 定位 first irrecoverable step 并强化关键 step update | binary-search rollout tree + hierarchical advantage attribution | 工具推理 setting 与真实 API write side effect 仍有距离 |
| [HiPER](https://arxiv.org/abs/2602.16165) | long-horizon interactive LLM agents | planner/executor 两层 credit | hierarchical advantage estimation | 依赖有意义的 subgoal 分解 |
| [ReBel](https://arxiv.org/abs/2605.20061) | partially observable long-horizon agents | 显式建模 belief state，奖励 belief consistency | dense belief-consistency supervision + belief-aware grouping | 很新；belief state 本身可能被错误抽取 |
| [UProp](https://arxiv.org/abs/2506.17419) | multi-step decision uncertainty | 估计从历史决策继承来的 uncertainty | diagnostic uncertainty estimator, not reward | 需要校准后才能变成惩罚或人工复核策略 |

## 3.6 未解决问题

- **几乎没人系统处理真实不可逆外部动作。** 现有 benchmark 多在 sandbox、simulator、search、web 或代码测试里；真实 API 的付款、删除、发信、权限变更需要 two-phase commit、undo log、human confirmation 和 environment-level oracle。LLM judge 不足以证明外部状态已恢复。
- **benchmark 往往绕开真实 state drift。** WebShop/ALFWorld/search QA 能研究长程 credit assignment，但它们的 side effect 比真实业务 API 更可控。tau-bench 更接近 API transaction，但仍主要用 terminal task checks，step-level rollback reward 覆盖不足。
- **format reward 会掩盖 drift。** 一条轨迹可以每步 JSON 都对、函数名都存在、参数类型都合法，但实体选错、旧状态沿用、policy precondition 缺失。把 schema score 加权过高会让模型在错误轨迹上显得“很规范”。
- **execution success 会掩盖 semantic drift。** 工具运行成功只说明调用没有 runtime error。APIGen 需要 semantic checker 的事实说明 execution check 本身不够；在长程任务里，execution success 甚至可能把错误写入真实状态。
- **LLM judge 不足以替代环境级 oracle。** Judge 可以评价 checklist、trajectory quality、semantic alignment，但对数据库终态、权限约束、资金/库存变化、hidden tests、真实用户确认等问题，仍需要环境快照、replay、unit tests 或 domain invariant。
- **backtracking reward 覆盖不足。** Reflexion 给了文本层面的 retry/反思，ELPO 给了 first irrecoverable step 的训练信号，但“发现错误、回滚状态、重新规划并验证新状态”的完整 tool-use reward 还没有成为主线。
- **belief drift 是新方向但还不稳。** ReBel 和 UProp 把 belief/uncertainty 放到长程 agent 的中心位置，这对 observation contamination 很关键；但 belief state 由模型抽取，可能自己漂移。API/tool-use reward 需要把 belief consistency 和 external state consistency 绑定，而不是只奖励内部叙述一致。
