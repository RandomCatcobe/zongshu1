# Handoff

Last updated: 2026-05-26

## Current State

The workflow is complete through final synthesis, and an ACL handoff package has been added for the next agent.

Completed in this continuation:

- Prompt 10: `drafts/part6_gaps_antipatterns.md`
- Prompt 11: `drafts/part7_adjacent_fields.md`
- Prompt 12: audits in `audits/`
- Prompt 13: `drafts/final_survey.md`

Current stop point:

```text
ACL HANDOFF PACKAGE READY
```

The next agent should start from:

- `drafts/research_gaps_acl_ready.md`
- `drafts/short_survey_acl_ready.md`
- `latex/agentic_rl_reward_refs.bib`
- `latex/acl_short_survey_snippet.tex`
- `pdf/latest_pdf_manifest.csv`

## What This Instance Did

I continued from `STOP AFTER PROMPT 9` and completed the remaining workflow:

- Wrote Part 6 gaps / anti-patterns.
- Wrote Part 7 adjacent-field transfer ideas.
- Completed `audits/citation_audit.md`.
- Completed `audits/claim_audit.md`.
- Completed `audits/coverage_audit.md`.
- Also completed the existing `audits/adversarial_review.md` placeholder.
- Wrote the final synthesis to `drafts/final_survey.md`.

No new sources were added after Prompt 8. The final source set remains:

- `sources/sources.jsonl`: 44 rows
- `matrices/work_taxonomy.csv`: 44 data rows
- `matrices/reward_taxonomy.csv`: 44 data rows
- `pdf/*.pdf`: 44 local PDFs

2026-05-26 additions:

- Verified latest arXiv versions for all arXiv sources via arXiv export API.
- Verified `bfcl_2024` via OpenReview API and official OpenReview PDF.
- Regenerated ACL-ready BibTeX at `latex/agentic_rl_reward_refs.bib`.
- Added short survey handoff draft: `drafts/short_survey_acl_ready.md`.
- Added focused research gaps draft: `drafts/research_gaps_acl_ready.md`.
- Added ACL LaTeX snippet: `latex/acl_short_survey_snippet.tex`.
- Added latest PDF manifest: `pdf/latest_pdf_manifest.csv`.

## Where to Look

- Full paper draft: `drafts/final_survey.md`
- Short handoff survey: `drafts/short_survey_acl_ready.md`
- Research gaps for paper ideation: `drafts/research_gaps_acl_ready.md`
- ACL references: `latex/agentic_rl_reward_refs.bib`
- Latest PDF manifest: `pdf/latest_pdf_manifest.csv`
- Step drafts: `drafts/part1_field_map.md` through `drafts/part7_adjacent_fields.md`
- Audits: `audits/citation_audit.md`, `audits/claim_audit.md`, `audits/coverage_audit.md`, `audits/adversarial_review.md`
- Source registry: `sources/sources.jsonl`
- Work notes: `notes/works/*.md`
- Supplementary topic notes: `notes/topics/cascade_drift_sources.md`, `notes/topics/silent_errors_sources.md`
- PDF status: `pdf/README.md`

## Remaining Weak Points

These are intentionally preserved as caveats rather than hidden:

- KTO / SimPO / RLOO coverage is still insufficient for core API/tool-use reward claims.
- Token/API cost reward evidence is thin.
- Real harmful-call refusal and side-effect safety reward evidence remains under-covered.
- Rollback/recovery reward is a research gap, not a solved line.
- The 11 supplementary Prompt 7/8 sources have topic notes, not full `notes/works/{id}.md` extraction files.
- `rebel_belief_2026` is very recent and should keep a replication caveat.

## Notes

- `*.pdf` remains ignored by `.gitignore`, so pushing PDFs requires `git add -f agentic-rl-reward-survey/pdf/*.pdf`.
- Do not rewrite the long survey before choosing the target gap; use the short survey and gap file as the next-agent entry point.
