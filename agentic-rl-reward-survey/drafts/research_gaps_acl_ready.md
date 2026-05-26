# 研究空白：ACL-ready 重点版

状态：2026-05-26 手写压缩版。目标是让另一个智能体直接围绕这些 gap 写综述、引言或未来工作。引用 key 来自 `latex/agentic_rl_reward_refs.bib`；PDF 最新版本见 `pdf/latest_pdf_manifest.csv`。

## Gap 1：Schema-valid function call 的语义错误仍缺少系统 reward

**问题。** Function calling benchmark 已经能较好检查函数名、JSON/AST、参数类型，但真实 API failure 常常是 schema-valid yet semantically wrong：实体选错、时间范围错、权限策略错、检索证据无关、成功返回但业务终态错。BFCL、APIGen、APIGen-MT 和 ToolRL 都提示 format reward 必须和 correctness/semantic reward 分离 \citep{bfcl_2024,apigen_2024,apigen_mt_2025,toolrl_2025}。

**为什么重要。** 这是 tool-use agent 最容易“骗过训练”的错误：reward 看起来稳定，dashboard 上 schema pass 上升，但用户目标没有改善。

**可做实验。** 构造一组 schema-valid negative calls：正确 endpoint + wrong entity、正确 JSON + stale value、正确 retrieval format + irrelevant evidence。比较三种 reward：

```latex
R_1 = R_{\text{schema}}, \quad
R_2 = R_{\text{schema}} + R_{\text{execution}}, \quad
R_3 = \min(R_{\text{schema}}, R_{\text{semantic}}).
```

**可能贡献。** 提出 semantic cap：只要 semantic oracle 判错，format reward 不能把总分抬高。

## Gap 2：External mutable state 下的 cascade drift 没有成熟 benchmark

**问题。** Web/software/API agents 的错误常常不是一次性 hallucination，而是 early wrong action 写入或污染状态，后续步骤围绕错误状态继续优化。现有 WebArena、SWE-bench、tau-bench 和 AgentBench 让 long-horizon failure 可见，但对“外部可变状态 + 不可逆 action + reward credit”的系统 benchmark 仍不足 \citep{webarena_2023,swe_bench_2023,tau_bench_2024,agentbench_2023}。

**为什么重要。** 真实 API agent 会改数据库、发邮件、退款、改权限。final reward 可能只看到最终失败，却不知道第一处不可恢复错误在哪里。

**可做实验。** Transaction sandbox：每个 action 都有 precondition、postcondition、undo log 和 audit log。比较 terminal-only reward 与 error-localized reward：

```latex
R(\tau)=R_{\text{final}}-\lambda \mathbf{1}[t=t_{\text{first-irrecoverable-error}}].
```

**可能贡献。** 把 ELPO/HCAPO/GiGPO 这类 credit assignment 思路迁移到真实 API side-effect 环境 \citep{elpo_2026,hcapo_2026,gigpo_2025}。

## Gap 3：Rollback / recovery reward 仍主要停留在 inference-time reflection

**问题。** Reflexion 证明 verbal feedback 和 memory 可以帮助 retry，但这不是对模型参数的稳定 recovery reward。ToolGate、UProp、ReBel-style belief consistency 更接近“先验证再提交状态”的方向，但缺少统一的 API rollback RL benchmark \citep{reflexion_2023,toolgate_2026,uprop_2025,rebel_belief_2026}。

**为什么重要。** 长程 agent 不可能永不犯错；部署价值取决于 detect、rollback、replan。

**可做实验。** 在 API task 中随机注入 wrong observation、stale cache、bad parameter fill，奖励 agent 发现错误并回滚：

```latex
R_{\text{recover}} =
\mathbf{1}[\text{detect}] + \mathbf{1}[\text{rollback}] + \mathbf{1}[\text{new-path succeeds}]
- \gamma C_{\text{extra-calls}}.
```

**可能贡献。** 从“奖励一次性成功”转为“奖励可恢复成功”。

## Gap 4：Calibrated abstention / ask-clarification reward 证据不足

**问题。** Reasoning-enhanced models 可能在 no-tool 或 distractor-tool 场景更容易编造工具调用；internal tool hallucination 也提示模型可在没有真实 dispatcher grounding 时模拟工具结果 \citep{reasoning_trap_2025,internal_tool_hallu_2026}。但多数 RL reward 仍偏向完成任务，而不是在不确定时 abstain、ask 或 request confirmation。

**为什么重要。** 高风险 API 场景中，错误调用比不调用更糟。

**可做实验。** Ambiguous API benchmark：用户目标缺少关键 slot，或候选工具有诱导性参数。比较是否加入 abstention reward：

```latex
R = R_{\text{task}} + \alpha \mathbf{1}[\text{ask when ambiguity high}]
- \beta \mathbf{1}[\text{blind commit under ambiguity}].
```

**可能贡献。** 把 no-tool decision 从 evaluation metric 变成 training reward。

## Gap 5：Process reward / PRM 缺少面向 tool-use 的校准和反作弊协议

**问题。** Process reward 可以缓解 sparse outcome reward，但 PRM 一旦参与 search 或 training，就会变成可被优化的 proxy。AgentPRM、ToolPRMBench 和 CM2 都说明 process/checklist reward 有价值，但也暴露 PRM noise、judge bias、distribution shift 和 reward-guided search 放大错误的风险 \citep{agentprm_framework_2025,toolprmbench_2026,cm2_2026}。

**为什么重要。** Dense reward 不是免费午餐；错误 verifier 会把错误轨迹优化得更稳定。

**可做实验。** 固定 held-out trace calibration set，训练期间监控：

```latex
\Delta_{\text{PRM}} =
\mathbb{E}[R_{\text{PRM}}(\tau)] - \mathbb{E}[R_{\text{task}}(\tau)].
```

如果 `Delta_PRM` 持续升高但 task success 不升，标记 PRM hacking。

**可能贡献。** 提出 PRM calibration dashboard：judge disagreement、held-out PRM error、outcome-PRM divergence、KL、entropy、invalid call rate。

## Gap 6：Cost / latency reward 和 safety reward 仍是工程必需但文献薄弱

**问题。** 当前资料中 cost/step penalty 主要出现在 search-count、action budget 或工程 checklist 里，缺少生产 API 成本、延迟、rate limit、权限风险的系统 RL 研究。tau-bench、Search-R1、R1-Searcher 和 turn-level credit assignment 提供了部分 evidence，但还不足以支撑成熟方法论 \citep{tau_bench_2024,search_r1_2025,r1_searcher_2025,turn_level_credit_assignment_2025}。

**为什么重要。** 真实 agent 部署时，API 成本、安全确认、用户等待、限流和副作用风险都是 reward 的一部分。

**可做实验。** 在 multi-turn API benchmark 中加入预算、延迟和高风险 action：

```latex
R = R_{\text{semantic}} - \gamma C_{\text{api}} - \delta C_{\text{latency}}
- \eta \mathbf{1}[\text{unsafe write without confirmation}].
```

**可能贡献。** 从 leaderboard success 扩展为 deployment reward。

## 最值得作为论文创新点的 5 个方向

| Rank | Gap | 最小实验 | 为什么能写成论文 |
|---:|---|---|---|
| 1 | schema-valid but semantically wrong calls | BFCL/APIGen-style negative set + semantic cap reward | 直接击中 function calling 当前评测和训练的盲点 |
| 2 | external mutable state cascade drift | transaction sandbox + first-irrecoverable-error penalty | 能把 long-horizon credit assignment 和真实 API side effect 连接起来 |
| 3 | rollback / recovery reward | wrong observation injection + rollback success reward | 从“少犯错”转向“可恢复”，部署价值强 |
| 4 | calibrated abstention | ambiguous/no-tool/distractor-tool benchmark + ask reward | 安全和可靠性方向清晰，现有证据不足但需求强 |
| 5 | PRM anti-hacking dashboard | held-out PRM calibration + outcome divergence metric | 能补 process reward 论文的关键可信度缺口 |

## 写作警戒线

- 不要把 ReAct、Reflexion、ToolBench 这类 inference-time / benchmark work 写成 RL training。
- 不要把 `R_schema` 写成 semantic reward。
- 不要把 API execution success 写成 business/task success。
- 没有 primary source 的工程经验只能标为 hypothesis。
- 2026 年新论文要显式标注 arXiv version 和 checked date，避免版本漂移。

