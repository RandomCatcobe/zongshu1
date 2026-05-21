# Preflight Checklist

正式跑 Prompt 2 之前检查：

- [x] 仓库结构已创建。
- [x] `REVIEW_PLAN.md` 已定义综述总计划和质量门槛。
- [x] `config/scope.md` 已定义研究范围。
- [x] `config/inclusion_exclusion.md` 已定义纳入/排除标准。
- [x] `schemas/work_schema.json` 已定义 source schema。
- [x] `schemas/reward_schema.json` 已定义 reward extraction schema。
- [x] `schemas/evidence_schema.json` 已定义 evidence schema。
- [x] `config/search_queries.md` 已生成分组 query plan。
- [x] `sources/missing_sources.md` 已列出必查工作。
- [x] `matrices/*.csv` 已初始化表头。
- [x] `drafts/*.md` 已初始化占位。
- [x] `audits/*.md` 已初始化占位。
- [ ] 正式联网检索尚未开始。
- [ ] `sources/sources.jsonl` 仍为空，等待 Prompt 2 写入。

## Handoff Rule

正式跑时每个 source 必须先进入 `sources/sources.jsonl`，再进入矩阵或正文。没有 source 的强 claim 只能写成“此处覆盖不足”或 `hypothesis`。

## Write Ownership

- 单 agent：按 prompt 顺序写。
- 多 agent：sidecar 只交候选 source 和 notes 草稿，由主 agent 合并。
- 不允许多个 agent 同时写 `drafts/final_survey.md`。
