# Citation Audit

Status: completed on 2026-05-21.

Scope checked:

- `sources/sources.jsonl`
- `matrices/work_taxonomy.csv`
- `matrices/reward_taxonomy.csv`
- `notes/works/*.md`
- `notes/topics/*.md`
- `drafts/part1_field_map.md` through `drafts/part7_adjacent_fields.md`

## Summary

- `sources/sources.jsonl` contains 44 parseable source rows.
- `matrices/work_taxonomy.csv` contains 44 data rows.
- `matrices/reward_taxonomy.csv` contains 44 data rows.
- 33 sources have full `notes/works/{id}.md` files.
- 11 supplementary sources are covered through topic notes: 6 in `notes/topics/cascade_drift_sources.md` and 5 in `notes/topics/silent_errors_sources.md`.
- Every source row has a URL.
- No duplicate source id was found in the active source list.

## Findings

| Issue | Location | Evidence | Severity | Fix |
|---|---|---|---|---|
| Supplementary sources lack full work-note files | Prompt 7 and Prompt 8 additions | `uprop_2025`, `hiper_2026`, `elpo_2026`, `hcapo_2026`, `gigpo_2025`, `rebel_belief_2026`, `agenthallu_2026`, `toolgate_2026`, `internal_tool_hallu_2026`, `butterfly_toolchains_2025`, `reasoning_trap_2025` | medium | Final survey may cite them, but detailed claims should stay at topic-note level unless full notes are created later. |
| `bfcl_2024` id has year mismatch with canonical paper year | `sources/sources.jsonl`, notes, matrices | Stable id remains `bfcl_2024`, but canonical source is OpenReview / ICML 2025 | low | Keep id stable; in prose cite as BFCL / Patil et al., 2025 and note version caveat. |
| `reasoning_trap_2025` id has year mismatch with current venue metadata | `sources/sources.jsonl`, matrices | Stable id uses arXiv-first naming; source row now records year 2026 / ACL 2026 | low | Keep id stable; in prose cite as Yin et al., 2026 and do not rely on the id for year. |
| Some rows have unknown authors | older first-pass source rows | `rlaif_api_code_2024`, `cm2_2026`, `mua_rl_2025`, `turn_level_credit_assignment_2025`, `task_reasoning_grpo_2025`, `toolprmbench_2026`, `r1_searcher_2025`, `swirl_2025` have shallow metadata | medium | In final references, cite title/year/URL where authors are unknown; avoid author-specific claims for these rows. |
| APIBench / API-Bank naming ambiguity | `source_conflicts.md`, `api_bank_2023`, `toolllm_toolbench_2023` | Prompt list used APIBench; canonical source in repo is API-Bank, while ToolLLM/ToolBench has separate API benchmark ecosystem | medium | Final survey must keep API-Bank and ToolBench separate and avoid using APIBench loosely. |
| ReTool name collision | `source_conflicts.md` | `retool_2025` conflicts with commercial Retool search results | low | Cite the arXiv title and URL whenever using ReTool. |
| ARTIST ambiguity | `source_conflicts.md` | Older unrelated Artist Agent RL work exists | low | Cite only `artist_2025` as the LLM/tool/RL paper. |

## Citation Rules for Final Survey

- Use source ids internally, but prose references should include paper title, year, and URL when possible.
- Benchmark-only rows must be labeled benchmark/evaluation, not training method.
- Topic-note-only sources should support scoped claims, not detailed method claims.
- Version-sensitive sources: BFCL, API-Bank/APIBench, ToolBench, and very recent 2026 arXiv papers.
