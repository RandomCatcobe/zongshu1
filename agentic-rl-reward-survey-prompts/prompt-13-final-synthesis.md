# Prompt 13：最终合成

```text
现在基于 drafts/ 和 audits/ 合成最终稿：drafts/final_survey.md。

要求严格按以下结构：

# Agentic RL 在 API/Tool-use 场景下的奖励函数设计：系统性研究综述与工程参考

## 第 1 部分：领域地图
整合 drafts/part1_field_map.md

## 第 2 部分：奖励函数与惩罚函数盘点
整合 drafts/part2_reward_inventory.md

## 第 3 部分：级联漂移
整合 drafts/part3_cascade_drift.md

## 第 4 部分：静默错误
整合 drafts/part4_silent_errors.md

## 第 5 部分：调参技巧与工程实践
整合 drafts/part5_engineering_checklist.md

## 第 6 部分：Gap / 反模式 / 大家踩过的坑
整合 drafts/part6_gaps_antipatterns.md

## 第 7 部分：可借鉴的相邻领域
整合 drafts/part7_adjacent_fields.md

## References
从 sources/sources.jsonl 自动整理。

写作要求：
- 中文叙述，英文术语和论文名保留英文。
- 多用表格。
- 每个具体工作给作者、年份、链接。
- 奖励公式用 LaTeX 或伪代码。
- 不要客套。
- 不要重复。
- 信息冲突处明确指出。
- 覆盖不足处直接写“此处覆盖不足”。
- 根据 audits/claim_audit.md 修正 unsupported claims。
- 根据 audits/citation_audit.md 修正引用。
- 根据 audits/coverage_audit.md 在相关位置补一句 coverage limitation。

完成后不要只输出摘要。请写入 drafts/final_survey.md，并在聊天中只汇报：
- final_survey.md 字数
- 仍然最薄弱的 5 个点
- 最值得作为论文创新点的 5 个 gap
```

