# Zongshu

本仓库保存一套文件化综述工作流，主题是 agentic RL 在 API、tool-use、function calling 场景下的 reward design。

## 目录

| Path | Purpose |
|---|---|
| `agentic-rl-reward-survey/` | 综述研究仓库：scope、检索计划、schema、矩阵、草稿和审计占位 |
| `agentic-rl-reward-survey-prompts/` | 可恢复执行的分阶段 prompt 记录 |

## 当前状态

综述计划已经整理到 `agentic-rl-reward-survey/REVIEW_PLAN.md`。当前停止点仍是正式资料搜集之前：

- 可以从 Prompt 2 开始做第一轮联网资料搜集。
- 不应直接写正文或向 `sources/sources.jsonl` 写入未核实条目。
- 所有 claim 必须先有可信 source，再进入 notes、matrices 或 drafts。

## 下一步

读取以下文件后执行 `agentic-rl-reward-survey-prompts/prompt-02-first-source-collection.md`：

- `agentic-rl-reward-survey/REVIEW_PLAN.md`
- `agentic-rl-reward-survey/HANDOFF.md`
- `agentic-rl-reward-survey/config/scope.md`
- `agentic-rl-reward-survey/config/inclusion_exclusion.md`
- `agentic-rl-reward-survey/config/search_queries.md`
- `agentic-rl-reward-survey/sources/missing_sources.md`
