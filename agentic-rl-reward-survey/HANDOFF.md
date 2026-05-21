# Handoff

## Current State

研究仓库已经完成正式跑之前的准备工作，并已将综述计划收束到 `REVIEW_PLAN.md`。尚未开始正式联网检索、source 抽取或正文写作。

已完成：

- 仓库目录结构已创建。
- `REVIEW_PLAN.md` 已定义综述目标、证据规则、source pipeline、draft structure、agent 分工和质量门槛。
- `config/scope.md` 已定义研究范围。
- `config/inclusion_exclusion.md` 已定义纳入/排除标准。
- `config/search_queries.md` 已生成分组检索计划。
- `config/run_strategy.md` 已写明并行/串行执行建议。
- `config/preflight_checklist.md` 已写明正式跑前检查项。
- `schemas/work_schema.json`、`schemas/reward_schema.json`、`schemas/evidence_schema.json` 已创建并验证可解析。
- `sources/missing_sources.md` 已列出第一轮必须尝试寻找的工作。
- `matrices/*.csv` 已初始化表头。
- `drafts/*.md` 与 `audits/*.md` 已初始化占位。
- `sources/sources.jsonl` 仍为空，表示尚未进入正式检索阶段。

## Stop Point

停止点设置在：

```text
STOP BEFORE PROMPT 2: 第一轮正式资料搜集尚未开始。
```

下一个 agent 可以从 Prompt 2 开始，但必须先读取：

- `README.md`
- `REVIEW_PLAN.md`
- `config/scope.md`
- `config/inclusion_exclusion.md`
- `config/search_queries.md`
- `config/preflight_checklist.md`
- `config/run_strategy.md`
- `sources/missing_sources.md`

## Next Action

下一步应该执行：

```text
Prompt 2：第一轮搜集代表工作
```

目标：

- 使用联网搜索。
- 建立 `sources/sources.jsonl`。
- 更新 `matrices/work_taxonomy.csv`。
- 更新 `sources/missing_sources.md`。
- 不写综述正文。

## Execution Rule

所有后续 claim 必须先有 source，再进入 notes、matrices 或 drafts。

没有 source 的内容只能标为：

- `unknown`
- `TODO`
- `此处覆盖不足`
- `hypothesis`

不要把 benchmark-only 工作写成 RL training 工作；不要把 syntax correctness、execution success、semantic success 混在一起。

## Parallelism Guidance

推荐策略：主工作流串行推进，正式检索阶段局部并行。

适合并行的检索 sidecar：

| Sidecar | Scope |
|---|---|
| representative works | ToolFormer, ReAct, Reflexion, ToRA, AgentTuning, FireAct, xLAM, ToolACE, ReTool, ARTIST, RAGEN, Search-R1, ToolRL |
| benchmarks | ToolBench, APIBench, AgentBench, WebArena, SWE-bench, tau-bench, BFCL, APIGen-MT |
| latest RL methods | 2024-2026 GRPO/RLOO/RLAIF/DPO/tool-use RL/process reward |
| failure modes | cascade drift, silent errors, reward hacking, benchmark limitations |

主 agent 必须负责合并、去重、标准化 id、写入 `sources/sources.jsonl` 和 `matrices/*.csv`。

不建议多个 agent 同时写：

- `sources/sources.jsonl`
- `sources/source_conflicts.md`
- `matrices/*.csv`
- `drafts/final_survey.md`
- `audits/*.md`

## Resume Checklist

恢复工作时先确认：

- `sources/sources.jsonl` 是否仍为空。
- `matrices/work_taxonomy.csv` 是否只有表头。
- `sources/missing_sources.md` 中 TODO 是否尚未处理。
- 当前任务是否仍是 Prompt 2。
- `REVIEW_PLAN.md` 是否仍与 scope 和 inclusion/exclusion 一致。
