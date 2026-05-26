# 第 2 部分：奖励函数与惩罚函数盘点

本部分把 agentic tool-use / API-use RL 中常见的 reward 拆成十类。核心原则是：**format reward 只能说明输出可解析，execution reward 只能说明调用能运行，semantic reward 才接近用户目标是否完成**。如果把三者混成一个分数，模型很容易学会“会写 JSON、会调用工具、但没有解决任务”的捷径。

| 类别 | 公式 / 伪代码 | 正奖励 | 负奖励 | 粒度 | 代表工作 | 适用场景 | 失败模式 |
|---|---|---|---|---|---|---|---|
| final correctness: exact match / unit test / execution success | `R = 1[answer == gold]`；`R = 1[unit_tests_pass]`；`R = 1[db_state_ok] * 1[user_output_ok]` | 最终答案正确、测试通过、数据库终态和用户可见答复都正确 | 错答案、测试失败、任务终态错误 | episode / patch / final answer | [Search-R1](https://arxiv.org/abs/2503.09516), [ReTool](https://arxiv.org/abs/2504.11536), [SWE-bench](https://arxiv.org/abs/2310.06770), [tau-bench](https://arxiv.org/abs/2406.12045), [MUA-RL](https://arxiv.org/abs/2508.18669) | 有明确答案、测试套件、数据库终态或任务成功判据的场景 | 长程任务里信号稀疏；早期错误难定位；测试通过不等于语义正确；pass^1 会高估不稳定 agent |
| format / schema compliance | `R_schema = 1[valid_json && required_fields && type_ok]` | JSON 可解析、字段齐全、参数类型正确、标签顺序正确 | 无法解析、缺字段、参数类型错、工具调用格式错 | call / message / generated sample | [ToolRL](https://arxiv.org/abs/2504.13958), [APIGen](https://arxiv.org/abs/2406.18518), [R1-Searcher](https://arxiv.org/abs/2503.05592), [BFCL](https://openreview.net/forum?id=2GmDdhBdDk) | function calling、结构化 tool call、数据生成过滤 | 纯 format reward 很危险：模型可能只学会外壳和标签，不学工具选择、参数语义或最终任务成功；R1-Searcher 还显示 format reward 需要修正以避免异常输出 |
| step-level process reward | `R_t = verifier(s_t, a_t, o_t)`；`Q(s_t,a_t)=E[sum gamma^k r_{t+k}]` | 当前动作推进子目标、检索正确、checklist item 满足、PRM 认为动作优于 plausible wrong action | 当前动作误导轨迹、子目标未满足、PRM 低分 | step / turn / state-action | [AgentPRM](https://arxiv.org/abs/2502.10325), [ToolPRMBench](https://arxiv.org/abs/2601.12294), [CM2](https://arxiv.org/abs/2602.12268), [Turn-Level Credit Assignment](https://arxiv.org/abs/2505.11821), [SWiRL](https://arxiv.org/abs/2504.04736) | 长程工具调用、搜索 agent、open-ended multi-turn tool use、需要提前纠错的轨迹 | PRM / judge 噪声会被 dense reward 放大；PRM 分数可能被 reward hacking；step label 昂贵且会随 policy distribution shift 失真 |
| tool efficiency: step / redundant-call / cost penalty | `R = R_task - lambda_steps * n_steps - lambda_api * n_calls - lambda_tok * n_tokens` | 用较少工具调用和 token 完成任务；必要检索或必要 API call | 重复查询、无效调用、超预算、过度搜索 | step / call / episode | [Turn-Level Credit Assignment](https://arxiv.org/abs/2505.11821), [R1-Searcher](https://arxiv.org/abs/2503.05592), [tau-bench](https://arxiv.org/abs/2406.12045), [HCAPO](https://arxiv.org/abs/2603.08754) | 搜索、API 计费、数据库工具、生产 agent 的 latency/cost 控制 | 惩罚太强会导致过早停止或不敢调用工具；惩罚太弱会让 retry loop 膨胀；当前 source set 对 token/API cost 的显式训练证据仍不足 |
| exploration / diversity | `R = R_task + beta * H(pi)`；`bonus = novelty(state, action)`；`A_i = normalize(R_i within rollout_group)` | 多样 rollout、覆盖不同工具路径、避免所有样本同质失败 | 低熵、重复轨迹、过早策略坍缩 | rollout group / trajectory | [RAGEN](https://arxiv.org/abs/2504.20073), [Search-R1](https://arxiv.org/abs/2503.09516), [GiGPO](https://arxiv.org/abs/2505.10978), [HCAPO](https://arxiv.org/abs/2603.08754) | sparse reward 下的 RL 探索、WebShop/ALFWorld/search agent | entropy / novelty bonus 在当前 tool-use source set 中覆盖不足；多样性可能制造更多低质量 tool call；group-normalized reward 若全组都差会给出误导性相对优势 |
| safety / harmful-call refusal | `R_safe = 1[refuse_unsafe] - 1[forbidden_call] - 1[policy_violation]` | 拒绝不允许的调用、遵守 domain policy、避免越权写入或危险操作 | 违规 API call、绕过确认、泄露敏感参数、错误拒绝必要任务 | call / turn / episode | [tau-bench](https://arxiv.org/abs/2406.12045), [APIGen-MT](https://arxiv.org/abs/2504.03601), [BFCL](https://openreview.net/forum?id=2GmDdhBdDk), [MUA-RL](https://arxiv.org/abs/2508.18669) | 客服交易、订单/退款/订票 API、需要 policy compliance 的多轮工具使用 | 现有证据更偏 policy/unit-test 和 relevance detection，真正 harmful tool-call refusal 的 RL reward 覆盖不足；过强拒绝奖励会牺牲可完成任务 |
| length / early stopping | `R = R_task - lambda_len * max(0, len - L)`；`R_stop = 1[stop_when_done] - 1[premature_stop]` | 简洁地完成任务、在状态充分时停止 | 冗长推理、重复调用、未完成就停止 | response / trajectory | [R1-Searcher](https://arxiv.org/abs/2503.05592), [Search-R1](https://arxiv.org/abs/2503.09516), [HCAPO](https://arxiv.org/abs/2603.08754), [tau-bench](https://arxiv.org/abs/2406.12045) | 搜索问答、code/tool reasoning、生产 API agent | 证据覆盖不足：现有工作多是观察到响应变短或行动预算，而不是系统化 early-stop reward；长度惩罚可能鼓励省略必要验证 |
| KL constraint | `J = E[R] - beta * D_KL(pi, pi_ref)`；`beta <- adapt(beta, target_kl)` | 策略改进但不远离参考策略 / 上一轮策略 | 过大 policy drift、过度优化 reward proxy | token / trajectory / policy update | [AgentPRM](https://arxiv.org/abs/2502.10325), [DMPO](https://arxiv.org/abs/2406.14868), [MUA-RL](https://arxiv.org/abs/2508.18669), [ToolPRMBench](https://arxiv.org/abs/2601.12294) | PPO/GRPO/RLHF-style tool-use RL，尤其是 PRM 或 LLM judge 参与时 | KL 是训练约束，不是任务 reward；太小会 reward hacking，太大会抑制探索；ToolRL 这类工作还显示有时会省略 KL，不能假设所有 GRPO 都使用同一约束 |
| advantage reshaping | `A_i = (R_i - mean(R_group)) / std(R_group)`；`A_step = R_step - baseline(anchor_state)`；`clip(A, -c, c)` | 把稀疏结果转换成相对优势，突出关键 step 或可比状态下的更好动作 | 低于组内基线的轨迹/动作、关键错误 step | group / turn / step | [Search-R1](https://arxiv.org/abs/2503.09516), [Turn-Level Credit Assignment](https://arxiv.org/abs/2505.11821), [GiGPO](https://arxiv.org/abs/2505.10978), [ELPO](https://arxiv.org/abs/2602.09598), [HCAPO](https://arxiv.org/abs/2603.08754) | GRPO/PPO-style agent RL、长程 credit assignment、重复状态可比的环境 | RLOO / leave-one-out 在当前核心 tool-use source set 覆盖不足；组内归一化会受采样质量影响；错误的 anchor-state 匹配会制造伪 step credit |
| preference / contrastive | `L_DPO = -log sigma(beta * (log pi_w - log pi_l - log pi_ref_w + log pi_ref_l))`；`R = 1[pick correct_action over plausible_wrong]` | preferred trajectory、correct action、环境反馈排序更高 | rejected trajectory、plausible but wrong action、低分反馈 | trajectory / action pair | [DMPO](https://arxiv.org/abs/2406.14868), [EPO](https://arxiv.org/abs/2408.16090), [ToolPRMBench](https://arxiv.org/abs/2601.12294), [RLAIF API-code](https://arxiv.org/abs/2406.20060) | multi-turn language agents、环境反馈优化、PRM step pair、API code generation | trajectory preference 仍然局部 credit 不足；偏好噪声会放大；KTO / SimPO 在本 source set 中缺少强 tool-use 核心证据，不能硬写成主流 agentic API-use reward |

## 2.1 三个容易混淆的边界

**Format reward 不等于 execution reward。** JSON valid、required fields present、argument type correct 只说明调用“像一个调用”。它无法证明工具名是否合适、参数值是否满足用户意图，甚至不能证明调用能执行。ToolRL 把 `R_format` 和 `R_correct` 拆开是有价值的，因为它暴露了格式奖励被过度学习的风险。

**Execution reward 不等于 semantic reward。** APIGen 的三段过滤很清楚：format check 之后还要 actual execution，execution 之后还要 semantic checker。函数能跑通，只说明 runtime 层面没有失败；如果查询目标错了、参数语义错了，execution success 仍可能给出看似正常但对用户无用的结果。

**Task-level outcome reward 不等于可训练的细粒度反馈。** tau-bench、SWE-bench、Search-R1、ReTool 这类终局信号客观、便宜、可扩展，但在长程轨迹里会把“哪个 API call 开始偏航、哪个观察被误读、哪个中间状态不可恢复”全部压缩成一个 0/1 或 final score。

## 2.2 组合式奖励的实用模板

一个较稳妥的 tool-use reward 通常不是单项，而是分层组合：

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

这不是建议所有项都上。相反，权重要由任务可验证性决定：single-turn function calling 可以较重地用 schema 和 call matching；API transaction 要把数据库终态、用户可见输出和 policy compliance 分开；搜索/网页/软件工程 agent 则需要 process/verifier 或 replay 检查来缓解稀疏 final reward。

## 2.3 证据不足但常被误写的项

- **KTO / SimPO**：当前 source set 没有足够强的 API/tool-use agent 核心论文直接使用它们，适合在 gaps 或 adjacent fields 中提，不适合写成已被充分验证的主线。
- **RLOO / leave-one-out**：可作为 advantage reshaping 的相关算法概念，但本轮核心 source 中没有足够直接的 tool-use reward 证据。
- **token/API cost reward**：工程上重要，但当前论文证据多是 action budget、search-count penalty 或“更简洁”结果，系统化 cost-aware RL reward 仍覆盖不足。
- **harmful-call refusal reward**：tau-bench/APIGen-MT/BFCL 支撑 policy compliance、relevance 和 unit-test-style checks，但对真实危险 API、越权写入、隐私泄露的 RL reward 证据仍不足。
