# Prompt 1：生成检索计划，不写正文

```text
现在只做检索计划，不写综述正文。

请读取 config/scope.md 和原始研究目标，然后更新 config/search_queries.md。

需要生成分组检索 query，覆盖以下方向：

1. agentic RL for tool use / API use / function calling
2. LLM tool-use training with PPO / GRPO / RLOO / REINFORCE / DPO / RLAIF
3. process reward model for agents / tool use / long-horizon reasoning
4. execution-based reward / unit-test reward / API execution feedback
5. error propagation / compounding error / trajectory drift in LLM agents
6. silent failure / plausible-but-wrong / semantic mismatch in tool use
7. benchmarks: ToolBench, APIBench, AgentBench, WebArena, SWE-bench, τ-bench, BFCL, ToolACE, APIGen-MT
8. representative works: ToolFormer, ReAct, Reflexion, ToRA, AgentTuning, FireAct, xLAM, ToolACE, ReTool, ARTIST, RAGEN, Search-R1, ToolRL, APIGen-MT
9. adjacent fields: reward shaping, HER, robotics long-horizon manipulation, code RL with execution feedback, math PRM

对每组 query 输出：
- query string
- purpose
- expected source types
- inclusion criteria
- likely false positives

同时创建 sources/missing_sources.md，列出必须尝试寻找的工作清单。不要编造链接。没有检索到的先标 TODO。

完成后停止。
```

