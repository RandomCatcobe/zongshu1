# Run Strategy

## Recommendation

建议采用 hybrid strategy：主线按 workflow 顺序跑，检索阶段局部并行。

## 为什么不能全并行

- Prompt 4 依赖 `sources/sources.jsonl` 和 `matrices/work_taxonomy.csv`。
- Prompt 5-13 依赖 notes、matrices、audits 的前置结果。
- 如果多个 agent 同时写 `sources/sources.jsonl`、`matrices/*.csv`、`drafts/*.md`，容易重复、冲突、引用不一致。
- final synthesis 必须等 claim audit 和 citation audit 完成后再写。

## 哪些适合并行

正式检索阶段可以拆成 4 个 sidecar：

| Sidecar | 负责范围 | 输出 |
|---|---|---|
| representative works | ToolFormer, ReAct, Reflexion, ToRA, AgentTuning, FireAct, xLAM, ToolACE, ReTool, ARTIST, RAGEN, Search-R1, ToolRL | candidate sources + uncertainty |
| benchmarks | ToolBench, APIBench, AgentBench, WebArena, SWE-bench, tau-bench, BFCL, APIGen-MT | benchmark taxonomy candidates |
| latest RL methods | 2024-2026 GRPO/RLOO/RLAIF/DPO/tool-use RL/process reward | latest source candidates |
| failure modes | cascade drift, silent errors, reward hacking, benchmark limitations | failure-mode sources |

主 agent 负责统一去重、标准化 id、写入 `sources/sources.jsonl`、更新 `matrices/*.csv` 和 `sources/source_conflicts.md`。

## 建议节奏

1. 严格顺序执行 Prompt 0-1。
2. Prompt 2-3 可并行搜集，但由一个主 agent 合并。
3. Prompt 4 只能在合并后的 sources 基础上执行。
4. Prompt 7、8、10 可分别深挖，但必须避免同时写同一个文件。
5. Prompt 12-13 必须顺序执行。

## 结论

如果只有一个 Codex，就工作流跑，最稳。
如果有多个 agent，就“并行检索 + 串行合并 + 串行审计/合成”。不要让多个 agent 同时写最终正文。

