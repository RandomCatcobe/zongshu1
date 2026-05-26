# ACL Citation Guide

本目录面向 ACL / ARR 风格论文写作。

## Files

- `agentic_rl_reward_refs.bib`：从 arXiv export API 和 OpenReview API 生成的 BibTeX。
- `acl_short_survey_snippet.tex`：可直接粘进 ACL 论文 related work / motivation 的短片段。

## ACL 用法

ACL 模板默认使用 natbib。正文中建议：

```latex
\citet{toolrl_2025} study reward design for tool learning.
Several benchmarks separate syntax, execution, and task success \citep{bfcl_2024,apigen_2024,tau_bench_2024}.
```

文末：

```latex
\bibliography{agentic_rl_reward_refs}
```

如果 `.bib` 放在子目录：

```latex
\bibliography{latex/agentic_rl_reward_refs}
```

## Citation Hygiene

- arXiv 条目已经写入 `eprint`、`archivePrefix`、`url` 和 latest version note。
- BFCL 使用 OpenReview / ICML 2025 官方记录，而不是错误 arXiv 命中。
- 如果后续新增 PDF，先更新 `pdf/latest_pdf_manifest.csv`，再补 BibTeX。
- 强 claim 优先引用 primary source；博客和 leaderboard 页面只作辅助说明。

