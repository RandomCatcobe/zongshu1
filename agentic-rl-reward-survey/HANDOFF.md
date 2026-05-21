# Handoff

## Current State

研究仓库已经完成正式跑之前的准备工作，并已将综述计划收束到 `REVIEW_PLAN.md`。Prompt 2/3 已完成第一轮联网检索和 2024-2026 补检；尚未开始逐篇 source 抽取或正文写作。

## Progress Snapshot

当前进度：

- 当前 Git 分支：`main`
- 当前远端：`origin` -> `https://github.com/RandomCatcobe/zongshu1.git`
- 已完成 Prompt：Prompt 0/1 准备阶段、Prompt 2 第一轮 source collection、Prompt 3 的 2024-2026 补检
- 当前停止点：`STOP AFTER PROMPT 3`
- 已收集 source：33 条，见 `sources/sources.jsonl`
- 已同步 work taxonomy：33 行，见 `matrices/work_taxonomy.csv`
- source confidence：27 条 `high`，6 条 `medium`
- first-pass taxonomy 中 `has_rl=true`：15 条
- 尚未开始：`notes/works/*.md` 逐篇抽取、`reward_taxonomy.csv`、`benchmark_taxonomy.csv`、`gap_matrix.csv`、正文草稿、审计

已跑过的机械校验：

- `sources/sources.jsonl` 可逐行解析为 JSON。
- `sources/sources.jsonl` 无重复 id。
- `matrices/work_taxonomy.csv` 无重复 id。
- `sources/sources.jsonl` 与 `matrices/work_taxonomy.csv` 的 id 集合完全一致。
- source 必填字段 `id/title/year/venue_or_source_type/url/confidence` 齐全。
- 当前 source URL 做过一次 HEAD 检查，未发现 4xx/5xx。

已完成：

- 仓库目录结构已创建。
- `REVIEW_PLAN.md` 已定义综述目标、证据规则、source pipeline、draft structure、agent 分工和质量门槛。
- `config/scope.md` 已定义研究范围。
- `config/inclusion_exclusion.md` 已定义纳入/排除标准。
- `config/search_queries.md` 已生成分组检索计划。
- `config/run_strategy.md` 已写明并行/串行执行建议。
- `config/preflight_checklist.md` 已写明正式跑前检查项。
- `schemas/work_schema.json`、`schemas/reward_schema.json`、`schemas/evidence_schema.json` 已创建并验证可解析。
- `sources/sources.jsonl` 已写入第一轮 source 条目。
- `matrices/work_taxonomy.csv` 已写入第一轮 taxonomy。
- `sources/missing_sources.md` 已记录已尝试检索的工作和仍需抽取的信息。
- `sources/source_conflicts.md` 已记录名称歧义和版本冲突。
- `matrices/reward_taxonomy.csv`、`matrices/benchmark_taxonomy.csv`、`matrices/gap_matrix.csv` 仍只有表头。
- `drafts/*.md` 与 `audits/*.md` 仍是占位。

## Stop Point

停止点设置在：

```text
STOP AFTER PROMPT 3: 第一轮代表工作/benchmark 搜集和 2024-2026 补检已完成。
```

下一个 agent 可以从 Prompt 4 开始，但必须先读取：

- `README.md`
- `REVIEW_PLAN.md`
- `config/scope.md`
- `config/inclusion_exclusion.md`
- `config/search_queries.md`
- `config/preflight_checklist.md`
- `config/run_strategy.md`
- `sources/sources.jsonl`
- `matrices/work_taxonomy.csv`
- `sources/missing_sources.md`
- `sources/source_conflicts.md`

## Next Action

下一步应该执行：

```text
Prompt 4：逐篇抽取 reward 信息
```

目标：

- 基于已有 sources，不直接扩写正文。
- 为 high/medium confidence source 建立 `notes/works/{id}.md`。
- 抽取 reward signal、reward granularity、execution/semantic/process reward、benchmark metric、failure mode 和 evidence id。
- 低置信或命名冲突条目先保留在 source_conflicts / missing_sources 中。
- 不写综述正文。

## Open Questions / Doubts

这些疑惑已经进入 `sources/source_conflicts.md`，但 Prompt 4 抽取时需要优先核实：

| Issue | Why it matters | Current handling |
|---|---|---|
| APIBench / API-Bank / ToolBench 命名关系 | Prompt 里写 APIBench；第一轮 canonical hit 是 API-Bank；ToolLLM/ToolBench 体系里也可能有 APIBench-style 评测命名。混写会污染 benchmark taxonomy。 | 暂时把 `api_bank_2023` 和 `toolllm_toolbench_2023` 分开；Prompt 4 逐篇对照。 |
| Reflexion 的 RL 身份 | 标题写 verbal reinforcement learning，但第一轮判断更像 inference-time verbal feedback + memory，不是梯度 RL。 | `has_rl=false`，`training_paradigm=self-reflection / inference-time`；抽取时找原文方法描述确认。 |
| BFCL 多版本 | 官方 leaderboard、paper、OpenReview/项目页可能对应不同版本和 metric。 | `bfcl_2024` 作为 canonical paper；如写 benchmark 演化，后续拆版本。 |
| ARTIST 名称歧义 | 搜索会命中旧的、非 LLM tool-use 的 Artist Agent RL 工作。 | 只把 2025 LLM/tool/RL ARTIST 纳入本综述。 |
| ReTool 名称歧义 | 名称与商业 Retool 平台和泛用 retool 搜索结果冲突。 | 只用 arXiv `2504.11536`。 |
| AgentPRM 命名 | first-pass 找到 `Process Reward Models for LLM Agents: Practical Framework and Directions`，后续还可能有相近标题/缩写的 AgentPRM 工作。 | 保留 `agentprm_framework_2025` 和 `toolprmbench_2026`；Prompt 4 先抽它们，不把后续同名工作混进来。 |
| RLAIF for tool use 证据偏窄 | 已找到 API-usage code generation 的 RLAIF，但不是强 canonical 的 broad tool-agent RLAIF。 | `rlaif_api_code_2024` 标 `medium`，不要过度代表化。 |
| 若干 2025/2026 条目的作者字段仍是 `unknown` | 第一轮以 URL 和标题落库，尚未做完整 bibliographic extraction。 | Prompt 4 需补作者、版本、venue、code_url 和 reward 细节。 |
| `ToolRL` reward 细节较浅 | 标题高度相关，但 first-pass 只确认 canonical URL，没有抽 reward 公式/任务设置。 | `toolrl_2025` 标 `medium`，Prompt 4 优先深挖。 |

## Execution Rule

所有后续 claim 必须先有 source，再进入 notes、matrices 或 drafts。

没有 source 的内容只能标为：

- `unknown`
- `TODO`
- `此处覆盖不足`
- `hypothesis`

不要把 benchmark-only 工作写成 RL training 工作；不要把 syntax correctness、execution success、semantic success 混在一起。

## Parallelism Guidance

Prompt 4 抽取阶段可以按 source 类别拆 sidecar，但主 agent 必须统一 evidence id 和 notes 模板：

| Sidecar | Scope |
|---|---|
| representative works | ToolFormer, ReAct, Reflexion, ToRA, AgentTuning, FireAct, xLAM, ToolACE, APIGen |
| benchmarks | ToolBench, API-Bank, AgentBench, WebArena, SWE-bench, tau-bench, BFCL, ToolPRMBench |
| latest RL methods | ReTool, ARTIST, RAGEN, Search-R1, ToolRL, CM2, MUA-RL, turn-level credit assignment |
| preference/process reward | AgentPRM, DMPO, EPO, RLAIF API-code, SWiRL |

主 agent 必须负责合并、去重、标准化 id、写入 `notes/works/*.md`、`matrices/reward_taxonomy.csv` 和后续 drafts。

不建议多个 agent 同时写：

- `sources/sources.jsonl`
- `sources/source_conflicts.md`
- `matrices/*.csv`
- `drafts/final_survey.md`
- `audits/*.md`

## Resume Checklist

恢复工作时先确认：

- `sources/sources.jsonl` 是否仍可逐行解析为 JSON。
- `matrices/work_taxonomy.csv` 是否与 sources 条目数一致。
- `sources/missing_sources.md` 中哪些条目仍需要深挖。
- 当前任务是否仍是 Prompt 4。
- `REVIEW_PLAN.md` 是否仍与 scope 和 inclusion/exclusion 一致。
