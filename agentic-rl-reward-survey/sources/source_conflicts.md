# Source Conflicts

Prompt 2/3 已开始正式检索。这里记录：

- 同一工作不同来源的标题、年份、作者、venue 冲突。
- 是否实际使用 RL 的说法冲突。
- reward signal、metric、benchmark setting 的冲突。
- arXiv、paper、GitHub、blog 版本差异。

## Conflict Log

| Work | Conflicting claims | Sources | Current resolution | Confidence | Follow-up |
|---|---|---|---|---|---|
| APIBench / API-Bank / ToolBench | Prompt list says APIBench; arXiv canonical hit is API-Bank; ToolLLM also references ToolBench/APIBench-style data and evaluation naming. | `api_bank_2023`, `toolllm_toolbench_2023` | Treat API-Bank as a separate benchmark source and ToolBench as ToolLLM's API benchmark/data ecosystem until full extraction confirms naming. | medium | Inspect ToolLLM paper and API-Bank paper side by side. |
| Reflexion RL status | Title says "Verbal Reinforcement Learning", but first-pass reading indicates inference-time verbal feedback/memory rather than gradient RL over model weights. | `reflexion_2023` | Mark `training_paradigm` as self-reflection / inference-time and `has_rl=false`. | high | Extract exact method language before drafting. |
| BFCL versions / false arXiv hit | Official Gorilla leaderboard, Berkeley Sky page, OpenReview paper, and current leaderboard versions may differ in task mix and metrics. During PDF collection, `https://arxiv.org/abs/2409.00608` resolved to TinyAgent, not BFCL. | `bfcl_2024` | Use OpenReview / ICML 2025 paper as canonical source; keep `bfcl_2024` id stable but set source URL to `https://openreview.net/forum?id=2GmDdhBdDk`. Record version-specific metric claims separately later. | high | Split BFCL v1/v2/v3/v4 if the review needs longitudinal benchmark discussion. |
| ARTIST ambiguity | Search also returns an older unrelated "Artist Agent" reinforcement-learning painting paper. | `artist_2025` | Use only the 2025 LLM/tool/RL ARTIST paper for this survey. | high | Verify acronym and author/version metadata during extraction. |
| ReTool ambiguity | Name collides with commercial Retool platform and generic "retool" search results. | `retool_2025` | Use arXiv 2504.11536 as canonical source. | high | Inspect project/code links if available. |
| AgentPRM naming | Search returns both `Process Reward Models for LLM Agents: Practical Framework and Directions` and later similarly named AgentPRM papers. | `agentprm_framework_2025`, `toolprmbench_2026` | Keep `agentprm_framework_2025` as the first-pass process-reward framework source; add later AgentPRM variants only after extraction. | medium | Search specifically for `AgentPRM step-wise promise progress` in a later pass. |
| RLAIF for tool use | First-pass search found API-usage code generation with RLAIF, but not a broad, canonical agentic tool-use RLAIF paper. | `rlaif_api_code_2024` | Include as medium-confidence adjacent/core candidate; do not overclaim broad tool-agent RLAIF coverage. | medium | Search ACL/OpenReview for tool-specific RLAIF and LLM-as-judge training rewards. |
| Prompt 7 supplementary source freshness | `rebel_belief_2026` is highly relevant to belief drift and credit assignment, but it was submitted on 2026-05-19 and has not yet had time for replication. | `rebel_belief_2026` | Include as a very recent Prompt 7 supplementary source with an explicit caveat. | medium | Recheck arXiv/OpenReview status if this source is used in the final survey. |
| Prompt 7 excluded search hits | Search surfaced a withdrawn OpenReview credit-assignment result and a title mismatch around a pathwise-evaluation paper. | not added | Do not add either as a core source until the canonical paper/version verifies cleanly. | high | Revisit only if a later prompt needs a broader related-work sweep. |
| Prompt 8 anonymous hallucination source | Search surfaced `Clicking the Void: LLM Agent is Hallucinating and Where to Find Them`, but the accessible result appeared as an anonymous submission rather than a stable canonical source. | not added | Do not add as a core source in this pass; keep the five Prompt 8 additions to arXiv/OpenReview-stable sources. | medium | Revisit if a published version or stable arXiv/OpenReview record becomes available. |
| Reasoning Trap status | `reasoning_trap_2025` is risk/diagnostic evidence about reasoning enhancement and tool hallucination, not a mitigation or reward-training method. | `reasoning_trap_2025` | Use only for failure-mode and anti-pattern claims unless a later source validates a mitigation. | medium | During Prompt 10, keep it under reward anti-patterns / limitations rather than reward inventory. |
