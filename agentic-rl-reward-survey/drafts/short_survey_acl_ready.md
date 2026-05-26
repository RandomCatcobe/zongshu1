# Agentic RL Tool-use Reward Design：短综述交接版

状态：2026-05-26 基于当前 `sources/sources.jsonl`、`notes/works/*.md`、`drafts/part*.md` 和最新版 PDF manifest 手写压缩版。本文不是最终投稿正文，而是给下一个综述智能体的 ACL-ready 素材底稿。引用 key 来自 `latex/agentic_rl_reward_refs.bib`。

## 1. 核心判断

Agentic RL for tool use 的难点不是简单“有没有 reward”，而是 reward 经常只覆盖 surface proxy：JSON/schema 是否正确、API 是否返回 success、unit test 是否通过、LLM judge 是否喜欢这条轨迹。已有 benchmark 和训练工作已经能支持一个较清晰的结论：syntax correctness、execution success、semantic task success 必须拆开建模，否则模型会学会通过 reward 而不是完成任务 \citep{apigen_2024,bfcl_2024,tau_bench_2024,toolrl_2025}。

从 2023 到 2026，领域大致经历了三层变化。第一层是 tool-use / agent benchmark 和 prompting baseline，例如 Toolformer、ReAct、Reflexion、ToolBench、API-Bank、AgentBench、WebArena、SWE-bench 和 tau-bench \citep{toolformer_2023,react_2022,reflexion_2023,toolllm_toolbench_2023,api_bank_2023,agentbench_2023,webarena_2023,swe_bench_2023,tau_bench_2024}。这些工作不一定使用 gradient RL，但定义了 tool call、action-observation trace、environment feedback 和 task success 的基本评估界面。第二层是 function calling / API data 和 metric 体系，例如 BFCL、APIGen、APIGen-MT、ToolACE、xLAM \citep{bfcl_2024,apigen_2024,apigen_mt_2025,toolace_2024,xlam_2024}。第三层是明确面向 tool-use 或 agent trajectory 的 RL / RL-like optimization，例如 ReTool、ARTIST、RAGEN、Search-R1、ToolRL、CM2、MUA-RL、turn-level credit assignment、ToolPRMBench \citep{retool_2025,artist_2025,ragen_2025,search_r1_2025,toolrl_2025,cm2_2026,mua_rl_2025,turn_level_credit_assignment_2025,toolprmbench_2026}。

## 2. Reward 设计的最小分解

一个更稳的 tool-use reward 不应写成单一 terminal score，而应至少分成：

```latex
R(\tau)=R_{\text{task}}+\alpha R_{\text{schema}}+\beta R_{\text{process}}
-\gamma C_{\text{tool}}-\eta R_{\text{unsafe}}-\lambda D_{\mathrm{KL}}(\pi_\theta || \pi_{\mathrm{ref}}).
```

其中 `R_schema` 只能证明输出形状可执行，不能证明调用语义正确；`R_task` 需要环境终态、hidden tests、DB state、business policy 或人工/专家 oracle；`R_process` 可以缓解 sparse outcome reward，但若 verifier/PRM 错误，会把错误轨迹优化得更像正确轨迹 \citep{agentprm_framework_2025,toolprmbench_2026,cm2_2026}。

| Reward 信号 | 可自动化程度 | 适合做什么 | 主要风险 | 代表证据 |
|---|---:|---|---|---|
| format/schema reward | 高 | JSON valid、函数名、参数类型、必填字段 | 学会“像工具调用”，不学语义 | \citep{bfcl_2024,toolrl_2025} |
| execution reward | 中高 | unit test、API success、环境返回值 | HTTP 200 / test pass 不等于任务成功 | \citep{swe_bench_2023,tau_bench_2024,apigen_2024} |
| semantic/task reward | 中低 | 用户目标、业务终态、policy compliance | oracle 昂贵，动态环境难复现 | \citep{tau_bench_2024,webarena_2023,bfcl_2024} |
| process reward / PRM | 中 | step verifier、trajectory ranking、search guidance | PRM hacking、distribution shift、错误 credit | \citep{agentprm_framework_2025,toolprmbench_2026,cm2_2026} |
| cost / step penalty | 高 | 控制 API 成本、冗余调用、延迟 | 过强会惩罚必要验证和 recovery | \citep{search_r1_2025,r1_searcher_2025,turn_level_credit_assignment_2025} |
| abstention / safety reward | 低到中 | no-tool、ask clarification、高风险 action 拦截 | 现有证据不足，容易被 task completion reward 压掉 | \citep{reasoning_trap_2025,internal_tool_hallu_2026,bfcl_2024} |

## 3. 两个最值得写深的失败模式

**Cascade drift** 指 early wrong tool/API call、错误 observation、旧 memory 或错误 subgoal 污染后续状态，导致后面的推理看似连贯但沿错误状态展开。它和普通 hallucination 不同：重点不是单句事实错，而是错误在 trajectory 中被提交、复用、放大。WebArena、SWE-bench、tau-bench、RAGEN、UProp、ReBel-style belief consistency 和 error-localized credit assignment 都说明长程 agent 需要 step/state 层面的检测，而不是只看 final answer \citep{webarena_2023,swe_bench_2023,tau_bench_2024,ragen_2025,uprop_2025,rebel_belief_2026,elpo_2026,hcapo_2026}。

**Silent errors** 指表面可执行、格式正确、甚至工具返回 success，但语义上错了：wrong entity、wrong endpoint with plausible args、stale result、irrelevant retrieval evidence、unit tests pass but hidden requirement fails。BFCL/APIGen/tau-bench/SWE-bench/ToolGate/Butterfly Toolchains 都支持把 syntax、execution、semantic 三层拆开 \citep{bfcl_2024,apigen_2024,tau_bench_2024,swe_bench_2023,toolgate_2026,butterfly_toolchains_2025}。

## 4. 对下一个智能体的写作建议

最终综述不要按“论文年份流水账”写。更强的结构是：

1. 先画 reward decomposition：format / execution / semantic / process / cost / safety。
2. 再用 cascade drift 和 silent errors 证明为什么 naive reward 不够。
3. 最后把研究空白落到可做实验：schema-valid but semantically wrong calls、external mutable state、rollback/recovery、calibrated abstention、multi-tool consistency、dynamic API reward。

ACL 正文中可以把强 claim 控制为：

```latex
Existing tool-use evaluations increasingly separate executable form from task-level success, but current reward designs still under-specify semantic validity, recovery, and side-effect safety in long-horizon API environments.
```

对应中文意思：现有评测已经开始区分“能不能调用”和“是否完成任务”，但 reward 设计仍系统性低估语义正确性、错误恢复和副作用安全。

