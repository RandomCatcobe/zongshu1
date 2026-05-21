# Agentic RL Reward Survey Research Repo

本仓库用于按文件化 workflow 写作一篇关于 agentic RL 在 API/tool-use 场景下 reward design 的系统综述。当前状态是 source collection ready：仓库结构、scope、schema、检索计划、执行计划、第一轮 source、work taxonomy 和审计占位已经准备好，但尚未开始逐篇抽取或综述正文写作。

综述总计划见 `REVIEW_PLAN.md`。它是恢复工作时的主入口，用于约束 source pipeline、agent 分工、证据规则和质量门槛。

## Workflow

1. 初始化仓库结构、scope、schema、矩阵和审计占位。
2. 制定综述总计划与检索计划，列出 query group、纳入标准、预期来源、false positives、证据规则和质量门槛。
3. 第一轮正式检索代表工作和 benchmark，写入 `sources/sources.jsonl` 与 `matrices/work_taxonomy.csv`。已完成一轮。
4. 第二轮补全 2024-2026 年最新 agentic RL / tool-use RL / process reward 工作。已完成一轮。
5. 基于已有 sources 逐篇抽取 reward 信息，生成 `notes/works/{id}.md`。下一步。
6. 分 section 合成：field map、reward inventory、cascade drift、silent errors、engineering checklist、gaps / anti-patterns、adjacent fields。
7. 做 citation / claim / coverage 审计。
8. 基于 drafts 和 audits 合成 `drafts/final_survey.md`。

## 当前停止点

可以开始执行 Prompt 4，也就是基于已有 source 逐篇抽取 reward 信息。正式跑之前不要跳过以下检查：

- `config/scope.md` 是否符合最终研究边界。
- `config/inclusion_exclusion.md` 是否能区分 RL training、benchmark-only、prompting-only。
- `schemas/*.json` 是否覆盖后续抽取字段。
- `config/search_queries.md` 是否覆盖代表工作、failure modes、benchmarks 和相邻领域。
- `sources/missing_sources.md` 是否列出了仍需深挖的工作。
- `sources/source_conflicts.md` 是否记录了命名和版本冲突。
- `sources/sources.jsonl` 与 `matrices/work_taxonomy.csv` 是否一致。

交接状态见 `HANDOFF.md`。硬停止点见 `STOP_POINT.md`。

## 建议执行方式

采用“主工作流顺序推进 + 局部并行检索”的混合方式。不要把所有 prompt 并行丢给多个 agent，因为后续抽取和写作依赖前置 sources 与 schema；但在正式检索阶段，可以把代表工作、benchmark、failure mode、最新 RL 方法拆成并行 sidecar 任务，最后统一去重和审计。
