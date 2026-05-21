# Stop Point

```text
STOP AFTER PROMPT 3
```

当前仓库已完成正式运行前准备，并完成 Prompt 2/3 的第一轮资料搜集和 2024-2026 补检：

- schema ready
- search plan ready
- review plan ready
- source collection started
- `sources/sources.jsonl` populated with first-pass sources
- `matrices/work_taxonomy.csv` populated with first-pass taxonomy
- `sources/missing_sources.md` updated with attempted queries and remaining extraction gaps
- `sources/source_conflicts.md` updated with naming/version ambiguity
- draft/audit placeholders ready

下一步不得直接写最终正文。应先执行 Prompt 4，基于 `sources/sources.jsonl` 和 `matrices/work_taxonomy.csv` 为 high/medium confidence source 建立 `notes/works/{id}.md`，逐篇抽取 reward、metric、failure mode 和 evidence。

恢复时先读 `REVIEW_PLAN.md`、`HANDOFF.md`、`sources/source_conflicts.md`，尤其是 `HANDOFF.md` 中的 Open Questions / Doubts，然后执行 Prompt 4。
