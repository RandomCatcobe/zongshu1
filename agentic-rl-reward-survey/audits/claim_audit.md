# Claim Audit

Status: completed on 2026-05-21.

Scope checked:

- `drafts/part1_field_map.md`
- `drafts/part2_reward_inventory.md`
- `drafts/part3_cascade_drift.md`
- `drafts/part4_silent_errors.md`
- `drafts/part5_engineering_checklist.md`
- `drafts/part6_gaps_antipatterns.md`
- `drafts/part7_adjacent_fields.md`
- `sources/sources.jsonl`
- `notes/works/*.md`
- `notes/topics/*.md`

## Findings

| Issue | Location | Evidence status | Severity | Fix |
|---|---|---|---|---|
| KTO / SimPO coverage remains weak | Parts 1, 2, Handoff caveats | Current source set does not contain strong API/tool-use agent papers using KTO or SimPO as core methods | medium | Final survey should mention them only as under-covered adjacent preference objectives, not as established tool-use reward methods. |
| RLOO coverage remains weak | Parts 2, 5 | Current source set has GRPO/PPO-style and group-advantage evidence, but no strong RLOO-specific API/tool-use source | medium | Use "GRPO/RLOO-style advantage pitfalls" as engineering caution; do not claim RLOO is widely validated here. |
| Prompt 7 supplementary sources are not all API-specific | Part 3 | HiPER, HCAPO, GiGPO, ReBel, UProp cover long-horizon agents/credit/belief, often ALFWorld/WebShop/search, not direct production API workflows | medium | Final survey should present them as supporting long-horizon credit evidence, not as mature API transaction reward designs. |
| Prompt 8 supplementary sources are mostly diagnostic, not training methods | Part 4 and Part 6 | AgentHallu, Internal Tool Hallucination, Butterfly Toolchains, Reasoning Trap are evaluation/diagnostic; ToolGate is runtime verification | medium | Final survey should put them under detection/verification/gaps, not under RL reward-training successes. |
| Cost/latency reward evidence is thin | Parts 2, 5, 6 | Evidence supports search-count/action-budget style penalties, not comprehensive token/API cost RL | low | Mark explicit cost-aware reward as engineering need / coverage limitation. |
| Safety/refusal reward evidence is partial | Parts 2, 4, 6 | tau-bench, BFCL, APIGen-MT support policy/relevance/unit-test checks, but not broad real harmful API refusal reward | medium | Final survey should avoid claiming solved tool safety; frame as open gap around side-effect safety. |
| Execution success vs semantic success is correctly separated | Parts 2, 4, 6 | APIGen, tau-bench, SWE-bench, BFCL examples support the distinction | none | Keep this as a central thesis in final survey. |
| Format success vs semantic success is correctly separated | Parts 2, 4, 6 | ToolRL, BFCL, APIGen, xLAM, ToolACE support distinction and caveats | none | Keep reward formulas explicitly layered. |
| Benchmark-only works are mostly labeled correctly | Parts 1 and notes | BFCL, API-Bank, AgentBench, WebArena, SWE-bench, tau-bench, ToolPRMBench are marked benchmark/evaluation | low | In final survey, repeat benchmark-only labels in representative-work tables. |
| Hallucination is not over-used as a synonym | Parts 3 and 4 | Part 3 distinguishes cascade drift; Part 4 distinguishes silent errors from hallucinated tool results | none | Preserve definitions in final survey introduction. |
| Adjacent-field transfer is analogy, not proven result | Part 7 | Part 7 explicitly says migration hypotheses, not proven tool-use evidence | none | Final survey should keep adjacent fields short and scoped. |

## Claims to Soften in Final Survey

- "Process reward solves credit assignment" should become "process reward can improve observability but is not automatically causal credit assignment."
- "Execution-based reward is reliable" should become "execution-based reward is stronger than format reward but still incomplete without semantic/business oracles."
- "LLM judge covers semantic success" should become "LLM judge broadens coverage but requires calibration and environment evidence."
- "Reasoning RL improves tool use" should become "reasoning RL may improve capability but can worsen tool hallucination without grounding/abstention objectives."
- "Tool-use benchmark score implies deployment readiness" should become "benchmark score is a partial proxy and needs dynamic environment, cost, safety, and user-outcome checks."
