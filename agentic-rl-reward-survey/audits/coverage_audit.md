# Coverage Audit

Status: completed on 2026-05-21.

## Coverage Matrix

| Requirement | Status | Evidence | Gap | Fix |
|---|---|---|---|---|
| Representative early tool-use and agent baselines | covered | Toolformer, ReAct, Reflexion, ToRA, AgentTuning, FireAct | Reflexion is not gradient RL and must stay labeled inference-time | Keep paradigm labels in final tables |
| API/tool-use benchmark coverage | covered | API-Bank, ToolBench, AgentBench, WebArena, SWE-bench, tau-bench, BFCL, ToolPRMBench | APIBench/API-Bank naming ambiguity remains | Use canonical source names and source-conflict caveat |
| Function-calling reward and metric design | covered | BFCL, APIGen, APIGen-MT, xLAM, ToolACE, ToolRL | Long-horizon business semantics still under-covered | Emphasize syntax/execution/semantic split |
| RL / RL-like tool-use training | mostly covered | ReTool, ARTIST, RAGEN, Search-R1, ToolRL, MUA-RL, CM2, turn-level credit assignment, Task Reasoning GRPO, SWiRL | Some 2025/2026 rows have shallow metadata | Avoid over-specific method details without full notes |
| Process reward models for agents | covered | AgentPRM, ToolPRMBench, CM2, SWiRL, turn-level credit assignment | PRM calibration and distribution shift remain open | Include PRM risk section in final survey |
| Long-horizon credit assignment / cascade drift | covered | DMPO, AgentPRM, ELPO, HCAPO, GiGPO, HiPER, ReBel, UProp | Several Prompt 7 sources are not API-specific | Use as supporting long-horizon evidence |
| Silent errors / semantic mismatch | covered | APIGen, BFCL, tau-bench, SWE-bench, WebArena, ToolPRMBench, AgentHallu, ToolGate, Butterfly Toolchains | Prompt 8 diagnostic sources lack full work notes | Keep detailed claims scoped; topic note is enough for section-level support |
| Reward hacking and anti-patterns | covered enough for review draft | ToolRL, R1-Searcher, AgentPRM, RAGEN, ToolPRMBench, Reasoning Trap, BFCL/tau-bench limitations | Public repo/blog anecdotes were not added as core evidence | Treat engineering anecdotes as non-core unless later sourced |
| KL, normalization, batch, rollout engineering | partially covered | AgentPRM, DMPO, MUA-RL, GRPO-style works, Part 5 engineering synthesis | Many checklist items are engineering synthesis rather than paper-specific claims | Label Part 5 as engineering guidance, not literature fact |
| Cost/efficiency reward | under-covered | search-count/action-budget penalties in R1-Searcher / turn-level credit assignment; tau-bench action budget | No broad token/API cost reward literature in source set | Mark as gap and avoid strong claims |
| Safety/refusal / harmful action reward | under-covered | tau-bench policy compliance, BFCL relevance, APIGen-MT policy/unit tests | Real high-risk API side-effect safety not well covered | Put under structural gaps and future experiments |
| Rollback / recovery reward | under-covered | Reflexion text retry, ELPO first irrecoverable step, ToolGate verified state, Part 7 robotics analogy | No mature API rollback RL source | Present as research gap with proposed experiment |
| Calibrated abstention / no-tool decision | under-covered but visible | Reasoning Trap, Internal Tool Hallucination, tau-bench/user clarification logic indirectly | Few direct reward-training examples | Present as gap: reward abstain/ask when uncertainty high |
| Multi-tool consistency and cross-episode memory corruption | under-covered | Part 3 state/belief drift, ReBel, UProp, ToolGate, Part 7 formal-method analogy | Few direct API-agent benchmarks | Propose dynamic memory/cross-tool consistency benchmark |
| Adjacent-field transfer ideas | covered as analogy | classical RL shaping/curriculum/exploration, HER, robotics, code RL, math PRM, formal methods | Not direct evidence | Keep as short analogy section |

## Work Still Missing or Shallow

- Full work notes for Prompt 7 and Prompt 8 supplementary sources.
- Strong tool-use evidence for KTO, SimPO, and RLOO.
- Direct evidence for cost-aware token/API reward in production-like tool agents.
- Direct evidence for harmful-call refusal reward under real side-effect APIs.
- Direct reward-learning papers for rollback/recovery after external state mutation.
- More primary evidence on dynamic environment reward under API version drift.

## Recommended Follow-up Deep Dives

1. Create full `notes/works/{id}.md` for the 11 topic-note-only supplementary sources before submission-quality finalization.
2. Run a focused search for side-effect safety, two-phase commit, and rollback reward in API agents.
3. Run a focused search for KTO/SimPO/RLOO in tool-use agents, but keep them out of the core narrative until primary evidence appears.
4. Compare BFCL version history and ToolBench/APIBench/API-Bank naming if the final paper discusses benchmark evolution.
5. Add an adversarial benchmark proposal for schema-valid but semantically wrong function calls.
