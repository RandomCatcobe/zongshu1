# Agentic RL Reward Survey Research Repo

本仓库用于按文件化 workflow 写作一篇关于 agentic RL 在 API/tool-use/function-calling 场景中 reward design 的系统综述。

综述总计划见 `REVIEW_PLAN.md`。恢复工作时的主入口是 `HANDOFF.md`，硬停止点见 `STOP_POINT.md`。

## Workflow Status

当前状态：

```text
ACL HANDOFF PACKAGE READY
```

最终综述：

- `drafts/final_survey.md`
- `drafts/short_survey_acl_ready.md`
- `drafts/research_gaps_acl_ready.md`

分步草稿：

- `drafts/part1_field_map.md`
- `drafts/part2_reward_inventory.md`
- `drafts/part3_cascade_drift.md`
- `drafts/part4_silent_errors.md`
- `drafts/part5_engineering_checklist.md`
- `drafts/part6_gaps_antipatterns.md`
- `drafts/part7_adjacent_fields.md`

审计结果：

- `audits/citation_audit.md`
- `audits/claim_audit.md`
- `audits/coverage_audit.md`
- `audits/adversarial_review.md`

证据与矩阵：

- `sources/sources.jsonl`
- `sources/source_conflicts.md`
- `sources/missing_sources.md`
- `matrices/work_taxonomy.csv`
- `matrices/reward_taxonomy.csv`
- `notes/works/*.md`
- `notes/topics/cascade_drift_sources.md`
- `notes/topics/silent_errors_sources.md`

## PDF 状态

`pdf/` 已包含当前 44 个 source 对应的本地 PDF，失败 0 个。2026-05-26 已用 arXiv export API 和 OpenReview API 核对最新版；状态记录见 `pdf/README.md` 和 `pdf/latest_pdf_manifest.csv`。

## ACL 引用材料

- `latex/agentic_rl_reward_refs.bib`
- `latex/acl_short_survey_snippet.tex`
- `latex/citation_guide.md`

## 后续建议

原始 prompt workflow 已结束。若要把综述推进到投稿质量，建议优先：

- 为 Prompt 7/8 的 11 个 supplementary sources 补 full `notes/works/{id}.md`。
- 深挖 KTO / SimPO / RLOO 是否存在强 tool-use primary evidence。
- 深挖 API side-effect safety、rollback/recovery reward、cost-aware reward。
- 按目标会议/期刊格式改写 `drafts/final_survey.md`。
- 从 `drafts/research_gaps_acl_ready.md` 选一个 gap，扩成 problem statement + benchmark proposal + reward formulation。
