# PDF Collection

Last updated: 2026-05-26.

This folder stores local PDF copies for the papers listed in `../sources/sources.jsonl`.
Files are named as `{source_id}.pdf` to keep them aligned with work notes and matrix rows.

## Status

- Attempted sources: 44
- Downloaded PDFs: 44
- Failed or unavailable PDFs: 0
- Corrected false-hit downloads: 1 (`bfcl_2024`)
- Latest-version manifest: `latest_pdf_manifest.csv`
- Version check source: arXiv export API for arXiv papers; OpenReview API + official PDF for `bfcl_2024`.
- Checked on: 2026-05-26.

Most current sources use arXiv canonical URLs and should be treated as the latest version recorded in `latest_pdf_manifest.csv`.
`bfcl_2024` uses the OpenReview / ICML 2025 paper PDF because the previous arXiv URL in the source list resolved to a different paper.

Prompt 7 supplementary sources were added and downloaded on 2026-05-21:

- `uprop_2025`
- `hiper_2026`
- `elpo_2026`
- `hcapo_2026`
- `gigpo_2025`
- `rebel_belief_2026`

Prompt 8 supplementary sources were added and downloaded on 2026-05-21:

- `agenthallu_2026`
- `toolgate_2026`
- `internal_tool_hallu_2026`
- `butterfly_toolchains_2025`
- `reasoning_trap_2025`

## Notes

- `*.pdf` files are ignored by the repository `.gitignore` by default, but this handoff intentionally force-adds the current latest PDF set so the next agent has the exact local evidence bundle.
- If later sources cannot be downloaded as PDFs, add them under "Failed or unavailable PDFs" with the source id, URL, and reason.
- Before replacing a PDF, update `latest_pdf_manifest.csv` and cite the exact `pdf_url` / version used.

## Failed or unavailable PDFs

None for the current source set.
