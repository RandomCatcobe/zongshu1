# Prompt 2：第一轮搜集代表工作

```text
现在执行第一轮资料搜集。目标不是写综述，而是建立 sources/sources.jsonl 和 matrices/work_taxonomy.csv。

请使用联网搜索。时间范围 2021 年至今。优先检索论文、arXiv、ACL Anthology、NeurIPS/ICLR/OpenReview、GitHub、项目页、官方 blog。

第一轮只覆盖代表工作和 benchmark：

必须尝试覆盖：
- ToolFormer
- ReAct
- Reflexion
- ToolLLM / ToolBench
- APIBench
- AgentBench
- WebArena
- SWE-bench
- τ-bench / tau-bench
- ToRA
- AgentTuning
- FireAct
- xLAM
- ToolACE
- APIGen / APIGen-MT
- ReTool
- ARTIST
- RAGEN
- Search-R1
- ToolRL
- BFCL / Berkeley Function Calling Leaderboard
- process reward model for LLM agents
- GRPO tool-use training
- RLAIF / DPO for tool use

对每个找到的 source，写入 sources/sources.jsonl，一行一个 JSON object，字段至少包括：
{
  "id": "...",
  "title": "...",
  "authors": "...",
  "year": "...",
  "venue_or_source_type": "paper/arxiv/blog/github/benchmark/report",
  "url": "...",
  "code_url": "...",
  "task_type": "...",
  "training_paradigm": "...",
  "reward_signal": "...",
  "tool_use_setting": "...",
  "notes": "...",
  "confidence": "high/medium/low"
}

同时更新 matrices/work_taxonomy.csv，列包括：
id,title,year,source_type,training_paradigm,reward_source,task_type,tool_use_setting,benchmark,has_rl,has_process_reward,has_execution_reward,notes

规则：
- 不确定就标 low confidence，不要编造。
- 找不到的写入 sources/missing_sources.md。
- 每个 source 至少有一个 URL。
- 不要写正文。
- 完成后给出：找到多少项、缺失多少项、最不确定的 5 项。
```

