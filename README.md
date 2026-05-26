# Zongshu

本仓库保存一套文件化综述工作流，主题是 agentic RL 在 API、tool-use、function calling 场景中的 reward design。

## 目录

| Path | Purpose |
|---|---|
| `agentic-rl-reward-survey/` | 综述研究仓库：scope、检索计划、schema、矩阵、notes、drafts、审计和 PDF 状态 |
| `agentic-rl-reward-survey-prompts/` | 可恢复执行的分阶段 prompt 记录 |

## 当前状态

当前状态是 `ACL HANDOFF PACKAGE READY`。

最终综述已写入：

```text
agentic-rl-reward-survey/drafts/final_survey.md
```

2026-05-26 新增给下一个智能体的短交接材料：

```text
agentic-rl-reward-survey/drafts/short_survey_acl_ready.md
agentic-rl-reward-survey/drafts/research_gaps_acl_ready.md
agentic-rl-reward-survey/latex/agentic_rl_reward_refs.bib
agentic-rl-reward-survey/latex/acl_short_survey_snippet.tex
agentic-rl-reward-survey/pdf/latest_pdf_manifest.csv
```

已完成：

- Prompt 2/3：代表工作、benchmark 和 2024-2026 最新工作检索。
- Prompt 4：逐篇 reward 信息抽取，生成 `notes/works/*.md` 和 `matrices/reward_taxonomy.csv`。
- Prompt 5：第 1 部分领域地图。
- Prompt 6：第 2 部分 reward / penalty 盘点。
- Prompt 7：第 3 部分级联漂移。
- Prompt 8：第 4 部分静默错误。
- Prompt 9：第 5 部分调参技巧与工程实践。
- Prompt 10：第 6 部分 gaps / anti-patterns。
- Prompt 11：第 7 部分相邻领域。
- Prompt 12：citation / claim / coverage / adversarial audits。
- Prompt 13：最终合成 `final_survey.md`。
- PDF 收集：`agentic-rl-reward-survey/pdf/` 已包含当前 44 个 source 的本地 PDF，失败 0 个。2026-05-26 已通过 arXiv export API / OpenReview API 核对最新版，结果见 `pdf/latest_pdf_manifest.csv`。

## 下一步

原始 workflow 已完成。后续建议从 `agentic-rl-reward-survey/HANDOFF.md` 和 `drafts/research_gaps_acl_ready.md` 开始，优先把研究空白扩展成论文问题定义，而不是继续泛化综述。
