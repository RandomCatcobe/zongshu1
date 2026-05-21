# Zongshu

本仓库保存一套文件化综述工作流，主题是 agentic RL 在 API、tool-use、function calling 场景下的 reward design。

## 目录

| Path | Purpose |
|---|---|
| `agentic-rl-reward-survey/` | 综述研究仓库：scope、检索计划、schema、矩阵、草稿和审计占位 |
| `agentic-rl-reward-survey-prompts/` | 可恢复执行的分阶段 prompt 记录 |

## 当前状态

综述计划已经整理到 `agentic-rl-reward-survey/REVIEW_PLAN.md`。当前停止点是 Prompt 3 之后：

- Prompt 2 第一轮代表工作/benchmark 搜集已完成一轮。
- Prompt 3 的 2024-2026 最新工作补检已完成一轮。
- 下一步应从 Prompt 4 开始做逐篇 reward 信息抽取。
- 所有 claim 必须先有可信 source，再进入 notes、matrices 或 drafts。

## 下一步

读取以下文件后执行 `agentic-rl-reward-survey-prompts/prompt-04-extract-reward-info.md`：

- `agentic-rl-reward-survey/REVIEW_PLAN.md`
- `agentic-rl-reward-survey/HANDOFF.md`
- `agentic-rl-reward-survey/config/scope.md`
- `agentic-rl-reward-survey/config/inclusion_exclusion.md`
- `agentic-rl-reward-survey/sources/sources.jsonl`
- `agentic-rl-reward-survey/matrices/work_taxonomy.csv`
- `agentic-rl-reward-survey/sources/missing_sources.md`
- `agentic-rl-reward-survey/sources/source_conflicts.md`
