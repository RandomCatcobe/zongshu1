# 第 7 部分：可借鉴的相邻领域

本节只做迁移假设，不把相邻领域的结论写成已经在 agentic tool-use 中充分证明。重点是：哪些 reward idea 值得搬过来，搬过来时会卡在哪里。

| 相邻领域 | 已有 reward idea | agentic tool-use 可迁移版本 | 为什么还没充分迁移 | 可能实验 |
|---|---|---|---|---|
| classical RL reward shaping | potential-based reward shaping 用状态势函数给中间进展奖励，同时保持最优策略不变 | 把 API transaction 的“已确认用户、已定位订单、已满足 policy prerequisite”写成 potential；只奖励朝合法终态靠近的状态变化 | LLM agent 的状态不是低维 MDP state，很多中间文本状态不可验证；错误 potential 会奖励错误子目标 | 在 tau-bench 风格 sandbox 中比较 terminal reward、checklist reward、potential-style subgoal reward |
| classical RL curriculum | 从短轨迹、少工具、确定 oracle 开始，逐步增加多轮、噪声和动态状态 | 从 single-turn BFCL/APIGen 任务过渡到 multi-turn BFCL、tau-bench、WebArena/SWE-bench 类任务 | 课程难度不只由步数决定，还由工具可逆性、状态漂移、用户不确定性决定 | 按“schema -> execution -> semantic -> side-effect safety”分阶段训练 |
| classical RL exploration bonus | 对新状态/新动作给 bonus，避免 sparse reward 下不探索 | 奖励探索必要工具路径或替代查询，但不奖励无意义 API spam | tool-use 中“新”不等于“有用”，探索会产生真实成本和副作用 | 只在 sandbox/read-only tools 上给 novelty bonus；write tools 需要 confirmation gate |
| HER / goal relabeling | failed trajectory reuse：失败后把已达成状态重标成另一个目标 | 把失败 API 轨迹重标为“成功完成了哪个子目标/验证了哪个约束”，用于训练子目标 verifier | 真实用户目标不能随意改写；错实体/错订单不能被重标成成功业务任务 | 对只读检索/搜索轨迹做 subgoal relabeling，对写操作禁止重标成功 |
| HER / hindsight relabeling | hindsight 中从失败中学习可达目标 | 从失败工具链中学习“应该先确认哪个 slot”“哪个 precondition 缺失” | hindsight critic 容易合理化错误步骤；HCAPO 已展示方向但不是 API-specific | 比较 HCAPO-style hindsight critic 与 rule-based precondition checker |
| robotics long-horizon manipulation | sparse-to-dense reward：用接触、抓取、移动、放置等谓词拆解任务 | 用 domain predicates 拆 API 流程：entity identified、permission checked、state committed、user notified | API 任务的谓词常是业务语义，不是传感器可直接读数 | 给每个 API workflow 写 state predicates，测试 dense predicate reward 是否减少 silent errors |
| robotics recovery policy | 机器人失败后需要退回、重抓、重新规划 | tool agent 失败后需要 rollback、fresh re-query、ask user、abort unsafe action | 多数 agent benchmark 不建模真实 undo / rollback；Part 6 标为结构性空白 | 构建可 checkpoint 的 transaction env，训练 `detect -> rollback -> retry` policy |
| robotics irreversible action handling | 对不可逆动作加入安全前置条件和人类确认 | 对付款、删除、发信、权限变更使用 two-phase commit 和 postcondition verification | 现有 tool-use RL 多在 sandbox 中，不承担真实副作用成本 | 高风险 write-action benchmark：比较 blind commit 与 confirmation reward |
| code generation RL | unit test reward、execution feedback、repair loop | 对 tool-use trace 做可执行 replay、hidden tests、失败后 repair trajectory | unit tests 不覆盖全部业务语义；SWE-bench 已暴露 test-pass 不等于完整需求满足 | 在 API agent 中加入 hidden policy tests 和 mutation tests |
| code generation RL | hidden tests 防止 visible-test overfitting | function-calling benchmark 使用 hidden API cases、hidden entity constraints、hidden policy variants | 真实 API schema 和 policy 更新频繁，hidden set 维护成本高 | benchmark rotation：固定公开 schema，隐藏参数语义和业务规则 |
| math reasoning PRM | step verifier / process supervision 给推理中间步打分 | 给工具调用中间步打分：这个查询是否必要、这个参数是否由证据支持、这个 observation 是否被正确读取 | 数学步骤相对形式化，API 语义和用户意图更开放 | ToolPRMBench-style correct-vs-plausible-wrong pairs 扩展到业务 API |
| math reasoning PRM | process supervision vs outcome supervision 的比较 | 比较 terminal task reward、turn-level reward、checklist reward、PRM reward 对 tool agent 的效果 | Step labels 昂贵，LLM judge 噪声会污染训练 | 同一任务集上比较 human step labels、LLM labels、contract labels |
| program synthesis / formal methods | invariant checking：永远不能破坏某些性质 | API agent 的 policy invariant：未确认不得退款、越权不得写入、证据不足不得下结论 | 业务 invariant 难写全，且可能和用户满意度冲突 | 为 tau-bench-like domains 写 invariant suite，测 false positive / false negative |
| program synthesis / formal methods | contract checking：precondition / postcondition | ToolGate-style 工具合同：调用前检查 slot、权限、状态，调用后验证状态变化 | 许多外部 API 返回不完整状态；postcondition 可能需要额外查询 | 对每个 write API 加 contract wrapper，比较 false-success rate |
| program synthesis / formal methods | differential testing：多实现、多版本、多路径交叉验证 | 同一事实用 search、DB、profile、cached memory 多源验证；同一任务在 snapshot/live 环境重放 | 多源不一致时需要信任排序；一致也可能共同错误或过期 | 构建 cross-tool consistency benchmark，奖励发现并处理冲突 |

这些迁移方向的共同边界是：相邻领域 reward 多假设状态可观测、目标明确、动作语义稳定；而 agentic tool-use 的状态经常藏在自然语言、外部 API、缓存、用户意图和业务 policy 中。因此最稳妥的迁移路线不是直接搬公式，而是把它们转成可审计的 verifier、invariant、hidden test、rollback protocol 和 calibrated process reward。
