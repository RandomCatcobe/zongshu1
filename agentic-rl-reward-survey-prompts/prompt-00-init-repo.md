# Prompt 0：初始化研究仓库

```text
你是一个研究工程代理。不要一次性写完整综述。你的任务是初始化一个可迭代的研究仓库，用于写一篇关于 agentic RL 在 API/tool-use 场景下 reward design 的系统综述。

语言要求：
- 中文叙述
- 英文术语、方法名、论文名、benchmark 名保留英文
- 引用保留英文标题和链接
- 不要客套，不要总结性废话

请在当前目录创建如下结构：

agentic-rl-reward-survey/
  README.md
  config/
    scope.md
    search_queries.md
    inclusion_exclusion.md
  schemas/
    work_schema.json
    reward_schema.json
    evidence_schema.json
  sources/
    sources.jsonl
    missing_sources.md
    source_conflicts.md
  notes/
    works/
    topics/
    failure_cases/
  matrices/
    work_taxonomy.csv
    reward_taxonomy.csv
    benchmark_taxonomy.csv
    gap_matrix.csv
  drafts/
    part1_field_map.md
    part2_reward_inventory.md
    part3_cascade_drift.md
    part4_silent_errors.md
    part5_engineering_checklist.md
    part6_gaps_antipatterns.md
    part7_adjacent_fields.md
    final_survey.md
  audits/
    citation_audit.md
    claim_audit.md
    coverage_audit.md
    adversarial_review.md

初始化每个文件，写入简短说明和待办项。不要开始正式综述。

特别要求：
- 在 README.md 中写清楚整个 workflow。
- 在 config/scope.md 中写清楚研究范围：2021年至今，覆盖 LLM/agent 使用 API、tools、function calling，并使用 RL 或类 RL 方法训练/优化的工作。
- 在 config/inclusion_exclusion.md 中写清楚纳入和排除标准。
- 在 schemas/ 中定义 JSON schema，字段必须足够支持后续抽取。
- 完成后只汇报：创建了哪些文件、下一步应该做什么。
```

