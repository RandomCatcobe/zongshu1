# Prompt 12：总审计

```text
现在不要写新内容，做审计。

请检查以下文件：
- drafts/part1_field_map.md
- drafts/part2_reward_inventory.md
- drafts/part3_cascade_drift.md
- drafts/part4_silent_errors.md
- drafts/part5_engineering_checklist.md
- drafts/part6_gaps_antipatterns.md
- drafts/part7_adjacent_fields.md
- sources/sources.jsonl
- notes/works/*.md

生成三个审计文件：

1. audits/citation_audit.md
检查：
- 每个具体工作是否有 URL
- 是否有不存在或疑似错误的引用
- 是否有同一工作重复命名
- 是否有年份/作者不确定

2. audits/claim_audit.md
检查：
- 哪些强 claim 缺少证据
- 哪些地方把 benchmark-only 工作误写成 RL training
- 哪些地方把 format success 混同 semantic success
- 哪些地方把 execution success 混同 task success
- 哪些地方把普通 hallucination 混同 cascade drift 或 silent error

3. audits/coverage_audit.md
检查：
- 原始研究需求中哪些项已经覆盖
- 哪些项覆盖不足
- 哪些工作仍缺失
- 哪些部分需要二次深挖

规则：
- 不要粉饰。
- 直接指出问题。
- 对每个问题给出修复建议。
```

