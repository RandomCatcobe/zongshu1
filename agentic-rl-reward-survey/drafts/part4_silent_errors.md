# 第 4 部分：静默错误

这里的“静默错误”指 agent 的轨迹在表面检查上通过了：JSON 可解析、函数名存在、API 返回 `200`、单元测试通过、日志里没有异常，甚至解释也很流畅；但它真正完成的是错误任务、错误实体、错误状态转移或错误业务目标。它和第 3 部分的级联漂移相邻，但焦点不同：级联漂移强调错误如何沿长轨迹传播，静默错误强调错误如何被表层 reward / metric 掩盖。

本节始终区分三层信号：

- **syntax correctness**：输出能否解析，字段和类型是否符合 schema。
- **execution success**：工具或代码是否运行成功，外部系统是否返回正常状态。
- **semantic success**：调用是否满足用户意图、领域约束、事实证据和最终业务目标。

## 4.1 现象命名

- **silent failure**：系统没有报错，但任务真实目标失败。tau-bench 的 `r_action * r_output` 设计说明 API transaction 不能只看工具调用是否完成，还要看数据库动作和用户可见输出是否同时正确。
- **plausible-but-wrong**：动作、解释或检索证据看起来合理，但关键实体、时间、约束或结论错了。ToolPRMBench 用“正确动作 vs plausible incorrect action”来测 PRM，正是因为这种错误很容易骗过粗粒度打分。
- **hallucinated tool result**：模型没有真实调用工具，或把不存在/未返回的信息写成工具结果。`internal_tool_hallu_2026` 把 tool bypass、错误工具选择和参数级 hallucination 都归入工具调用幻觉。
- **semantic mismatch**：函数调用在格式上正确、运行上成功，但调用语义与用户意图不匹配，例如查到了张三而不是李四，或把“取消”误做成“修改”。
- **successful-but-incorrect**：API、代码或测试返回成功，但最终世界状态错了。SWE-bench 类测试通过也可能漏掉隐藏需求；WebArena 类环境中局部导航成功也可能不等于用户目标完成。
- **false positive execution success**：执行信号给了正反馈，但只是说明没有 runtime error。APIGen 需要 format check、actual execution 和 semantic checker 三层过滤，正说明 execution success 不是 semantic success。
- **schema-valid but semantically invalid**：JSON、AST、参数类型都正确，但参数值或约束不对。BFCL 的 AST / executable evaluator 对 function calling 很有价值，但不能替代业务语义 oracle。
- **reward-passing wrong behavior**：模型学会通过 reward proxy，例如格式奖励、过浅 judge、可见测试或固定 benchmark 模板，却没有学会真实任务目标。

不要把这些都叫 hallucination。错误实体、过期状态、业务规则违反、隐藏测试遗漏、错误聚合和 unsafe write 都可能不是“凭空编造”，而是 oracle 设计不足。

## 4.2 错误类型 taxonomy

| 错误类型 | 表面上为什么像对的 | 实际错在哪里 | format reward 是否能发现 | execution reward 是否能发现 | 需要什么 oracle |
|---|---|---|---|---|---|
| JSON valid but wrong argument | JSON 可解析，字段齐全，类型正确 | 参数值不满足用户意图或领域约束 | 通常不能 | 只有执行失败时能；多数不能 | 参数语义 checker；用户意图到参数的对齐 oracle |
| correct API endpoint but wrong entity | 端点正确，API 返回成功 | 用户、订单、文件、时间窗口或地点选错 | 不能 | 通常不能；API 会返回另一个合法实体 | 实体解析 oracle；跨工具/数据库一致性检查 |
| correct retrieval format but irrelevant evidence | 检索调用格式正确，返回文档存在 | 证据与问题无关、过旧或不支持结论 | 不能 | 检索成功本身不能发现 | evidence-to-claim entailment；时间戳和来源可信度检查 |
| tool returns success but business goal fails | 工具返回 `success=true` 或 HTTP `200` | 业务终态、用户确认或 policy prerequisite 未满足 | 不能 | 单次 API success 不能发现 | 业务状态 oracle；policy invariant；最终 DB snapshot |
| unit tests pass but hidden requirement fails | 可见测试全绿，patch 可运行 | 隐藏需求、边界条件或非功能约束未满足 | 不能 | 可见测试不能发现 | hidden tests；需求语义 checker；人工 code review |
| LLM invents tool output | 文本中出现“工具返回...”且内容流畅 | 没有真实工具调用，或返回内容被改写 | 不能 | 若没有强制执行日志则不能 | append-only tool log；tool-result provenance；response-grounding check |
| stale/cached result treated as current | 结果格式正确，缓存命中 | 时间敏感状态已变化 | 不能 | API/cache success 不能发现 | freshness oracle；版本号/时间戳 invariant；live revalidation |
| wrong aggregation over correct tool results | 每个子查询都成功 | 聚合、排序、过滤、单位换算或去重错了 | 不能 | 子调用执行成功不能发现 | end-to-end aggregation oracle；property-based tests |
| unsafe action formatted correctly | 危险 API 调用 schema 合法 | 缺少权限、确认、最小化或安全策略约束 | 不能 | 可能执行成功但造成真实损害 | policy oracle；two-phase commit；human confirmation |
| tool bypass / simulated execution | 回答看起来像工具结果，延迟和格式也像 | 模型直接模拟工具，没有调用外部系统 | 不能 | 若平台不强制 tool log 则不能 | dispatcher-level provenance；mandatory execution gate |
| parameter-name hallucination | 参数名像真实 API 参数 | 工具不接受该参数，或框架宽松映射到错误字段 | 有时能 | 有时能；宽松框架可能静默吞掉 | strict schema；no coercion parser；parameter consistency check |

`butterfly_toolchains_2025` 对参数填充失败做了专门 taxonomy，提醒我们很多静默错误发生在“自然语言约束 -> API 参数”的映射层，而不是最终回答层。`reasoning_trap_2025` 则提示：更强 reasoning 或 reasoning-style RL 并不自动带来更可靠工具选择，甚至可能放大 no-tool / distractor-tool 场景下的工具幻觉。

## 4.3 检测方法

| 方法 | 检测对象 | 可用信号 | 局限 |
|---|---|---|---|
| execution-grounded verification | 工具、代码、数据库状态是否真实运行并产生预期状态 | sandbox、DB snapshot、API log、unit tests | 执行成功仍可能不满足语义目标 |
| cross-tool consistency | 多个工具或多次查询是否指向同一实体、时间和结论 | search vs DB、profile vs order、retrieval vs citation | 多工具可能共同过期或共同错 |
| LLM judge with explicit rubric | 开放语义、解释质量、policy compliance | 分项 rubric、证据引用、judge disagreement | judge 可能被流畅文本骗过，且不可替代环境 oracle |
| reverse verification | 从最终答案反推出必须成立的工具事实 | answer-to-evidence check、claim decomposition | 依赖分解质量；复杂业务流程成本高 |
| ground-truth oracle | 是否满足标准答案、最终状态或专家标注 | gold answer、hidden tests、专家审查 | 昂贵，覆盖通常有限 |
| invariant / contract checking | 是否违反前置条件、后置条件或不可破坏约束 | Hoare-style contract、policy unit tests、schema invariant | 只能发现写进 contract 的约束 |
| differential testing | 不同模型、不同工具路径或不同环境版本是否给一致结果 | paired rollout、A/B tool execution、version replay | 一致不等于正确；成本高 |
| hidden tests | 可见 reward proxy 外的真实需求覆盖 | private test suite、edge cases、mutation tests | 仍可能漏掉业务语义和安全策略 |
| human audit | 专家判断业务目标、安全风险和用户满意度 | sampled traces、high-risk action review | 慢、贵、难以直接用于大规模 RL |
| hallucination attribution | 识别哪一步开始违背工具、环境或上下文现实 | AgentHallu-style responsible-step labels | 当前最强模型的 step localization 仍很难，不能单独当 oracle |
| internal representation detector | 在生成工具调用时提前拦截工具幻觉 | same-forward-pass embedding classifier | 依赖模型可访问内部表示；对黑盒 API 不一定可用 |

`toolgate_2026` 的契约化工具执行思路很适合工程系统：把工具调用拆成 precondition、execution、postcondition 和 state commit。它不只是“检测工具能否跑”，而是要求外部状态只能由验证过的工具结果更新。

## 4.4 奖励/惩罚设计

**Two-layer / three-layer reward**

```text
R_surface = w_format * R_format + w_exec * R_exec
R_semantic = oracle(intent, tool_calls, observations, final_state)

if R_semantic == 0:
  R_total = min(R_surface, cap_surface_only) - penalty_false_success
else:
  R_total = R_surface + w_sem * R_semantic
```

核心是限制表层 reward 的上限：格式和执行可以是必要条件，但不应单独给高分。

**Execution-based semantic reward**

```text
R_task =
  1[db_state_matches_goal]
* 1[user_visible_output_correct]
* 1[domain_policy_satisfied]
* 1[no_unapproved_write]
```

适合 tau-bench 式 API transaction。数据库终态、用户可见输出和 policy compliance 要分开记分，避免“API 成功但用户目标失败”。

**Judge-augmented reward**

```text
judge_input = {
  user_intent,
  allowed_tools,
  tool_call_log,
  raw_tool_outputs,
  final_answer,
  rubric
}
R_judge = judge(judge_input)
R_total = R_verifiable + alpha * calibrate(R_judge)
```

LLM judge 只适合作为开放语义补充。必须给 judge 原始工具日志和显式 rubric，并记录 judge disagreement，防止 judge pleasing 变成新的 reward hacking。

**Adversarial negative examples**

```text
sample = (state, correct_action, plausible_wrong_action)
R_pair = 1[rank(correct_action) > rank(plausible_wrong_action)]
```

ToolPRMBench 的 correct-vs-plausible-wrong 结构可迁移到 API agent：负例不应只是明显坏格式，而应包括错实体、错时间、错 policy、错 aggregation。

**Contradiction penalty**

```text
if final_answer contradicts raw_tool_outputs:
  R_total -= lambda_contradiction
if final_answer cites unsupported tool result:
  R_total -= lambda_unsupported
```

用于检索、数据库查询、代码执行和多工具聚合。重点惩罚“工具已经给出证据，但模型解释相反”的 silent failure。

**False-success penalty**

```text
if R_exec == 1 and R_semantic == 0:
  R_total -= lambda_false_success
```

这类惩罚专门针对“执行成功掩盖语义失败”。如果没有它，模型可能学会最大化可执行调用数量，而不是最大化任务完成。

**Uncertainty / abstention reward**

```text
if uncertainty_high and action_is_irreversible:
  reward(ask_clarifying_question or require_confirmation)
  penalize(commit_without_verification)
```

当实体、权限、金额、收件人或库存状态不确定时，安全动作不是继续猜，而是澄清、复核或暂缓提交。第 3 部分的 UProp / ReBel 思路可作为不确定性和 belief mismatch 的诊断补充。

## 4.5 代表工作

| 工作 | silent error 相关场景 | 检测方法 | reward/metric | 局限 |
|---|---|---|---|---|
| [APIGen](https://arxiv.org/abs/2406.18518) | function call 可执行但语义不一定对 | format check、actual execution、semantic checker | 生成样本必须三层都通过 | semantic checker 仍是 LLM，质量决定上限 |
| [APIGen-MT](https://arxiv.org/abs/2504.03601) | 多轮 API 蓝图和模拟用户轨迹 | execution check、policy unit tests、review committee | verifiable blueprint / trajectory filter | 模拟用户和 reviewer 可能漂移 |
| [BFCL](https://openreview.net/forum?id=2GmDdhBdDk) | function calling 格式、AST、executable、multi-turn / agentic 能力 | AST matching、executable evaluation、relevance detection | benchmark function-call correctness | AST/execution proxy 不能覆盖全部业务语义 |
| [tau-bench](https://arxiv.org/abs/2406.12045) | 客服式 API transaction，错误信息和 policy 错误 | final DB/action check + user-facing output check | `r_action * r_output`，`pass^k` | terminal reward 稀疏，难定位哪一步静默失败 |
| [ToolLLM / ToolBench](https://arxiv.org/abs/2307.16789) | 真实 API 工具链中任务完成与路径质量 | ToolEval pass rate / win rate | LLM evaluator over solution paths | 时间变化和 LLM judge 可能掩盖细微误用 |
| [ToolPRMBench](https://arxiv.org/abs/2601.12294) | correct action vs plausible wrong action | step-level PRM cases，多 LLM 过滤 | binary PRM accuracy | benchmark-only；弱 PRM 用于搜索会放大错误 |
| [SWE-bench](https://arxiv.org/abs/2310.06770) | patch 通过测试但隐藏需求失败 | repository tests | issue resolution / test pass | 测试不完整时 execution success 不是语义成功 |
| [WebArena](https://arxiv.org/abs/2307.13854) | 浏览器动作局部合理但用户目标失败 | functional task evaluator | end-to-end task success | 终局稀疏，局部动作语义难打分 |
| [AgentHallu](https://arxiv.org/abs/2601.06818) | 多步 agent hallucination 归因 | responsible-step annotation、taxonomy | hallucination attribution benchmark | 工具使用 hallucination 的 step localization 仍很难 |
| [Internal Tool Hallucination](https://arxiv.org/abs/2601.05214) | 错工具、坏参数、tool bypass | 内部表示实时检测 | detection accuracy / classifier signal | 需要内部表示，不适合所有黑盒部署 |
| [Butterfly Toolchains](https://arxiv.org/abs/2507.15296) | 参数填充失败沿工具链放大 | 参数失败 taxonomy、输入扰动 | diagnostic analysis | 不是 reward 方法，需要转化为 verifier / penalty |
| [ToolGate](https://arxiv.org/abs/2601.04688) | 工具结果污染 agent state | precondition / postcondition / verified commit | contract-grounded execution | contract 覆盖不足时仍会漏掉语义错误 |
| [Reasoning Trap](https://arxiv.org/abs/2510.22977) | reasoning 增强后工具幻觉上升 | SimpleToolHalluBench | diagnostic benchmark over no-tool / distractor-tool cases | 说明风险，不提供完整 reward 方案 |
| [CM2](https://arxiv.org/abs/2602.12268) | open-ended multi-turn tool use 中 checklist 漏检或误判 | evidence-grounded checklist + LLM judge | checklist reward | dense judge/checklist 可能放大 judge noise |

## 4.6 空白区

- **benchmark 很少覆盖“合法但错对象”的业务错误。** 错用户、错订单、错时间窗口、错库存地点、错权限范围往往都能通过 schema 和 API 执行。
- **execution success 最容易掩盖状态写入错误。** HTTP `200`、数据库写入成功、工具返回 success，只说明动作发生了，不说明动作该发生。
- **缓存和时间敏感状态覆盖不足。** 静默错误常来自 stale observation：旧报价、旧库存、旧 policy、旧用户偏好被当成当前事实。
- **unsafe action formatted correctly 仍缺少系统性 reward evidence。** 现有来源支持 policy/unit-test/relevance detection，但对真实危险 API 的权限、确认、撤销和审计 reward 覆盖不足。
- **LLM-as-judge 容易误判三类情况。** 第一，judge 看不到真实外部状态；第二，工具日志和最终回答表面一致但业务规则错；第三，模型用流畅解释迎合 rubric。
- **domain expert / business oracle 难替代。** 退款资格、医疗建议、金融交易、法律合规、权限边界和企业内部流程都需要领域规则或专家抽查。
- **工具幻觉不等于全部静默错误。** hallucinated tool name / result 是重要子类，但错参数、错实体、错聚合、错 policy 和过期状态同样关键。
- **从诊断到训练 reward 仍有缺口。** AgentHallu、Butterfly Toolchains、Internal Representation detector 和 ToolGate 给出检测或验证方向，但如何稳定并入 PPO/GRPO/DPO/PRM 训练仍需要更多实验。
