# Cascade Drift Supplementary Sources

Last updated: 2026-05-21.

This topic note records the supplementary Prompt 7 search around error propagation, compounding error, trajectory drift, long-horizon credit assignment, step-level verifier/process reward methods, and backtracking-style correction. These sources were added after Prompt 5, so they are covered here as a topic note rather than as full Prompt 4 work notes.

## Included Sources

| Source id | Why included | Prompt 7 use | Caveat |
|---|---|---|---|
| `uprop_2025` | UProp decomposes sequential decision uncertainty into internal and inherited/extrinsic uncertainty across multi-step agent decisions. | Detection methods: uncertainty propagation and uncertainty-aware penalties. | It is not a reward-training method, so use it only for detection and risk scoring. |
| `hiper_2026` | HiPER separates high-level planning from low-level execution and assigns credit at both levels using hierarchical advantage estimation. | Reward design: subgoal decomposition reward and hierarchical credit assignment. | It is long-horizon agent RL, but not specifically API/function-calling. |
| `elpo_2026` | ELPO explicitly localizes the first irrecoverable step in tool-integrated reasoning and strengthens updates on the critical step and suffix. | Mechanisms: early wrong action contaminating later trajectory; reward design: explicit intermediate error penalty. | Tool-integrated reasoning includes code/math/science tools rather than broad API transaction agents. |
| `hcapo_2026` | HCAPO uses hindsight reasoning and an LLM post-hoc critic to refine step-level Q-values for long-horizon LLM agents. | Reward design: dense step credit from delayed sparse outcomes. | The post-hoc critic can introduce judge/model bias; treat as a process-reward direction, not an oracle. |
| `gigpo_2025` | GiGPO builds macro trajectory advantages and micro step advantages from repeated anchor states across trajectories. | Reward design: advantage reshaping and cross-trajectory credit assignment. | It assumes comparable states can be identified across rollouts. |
| `rebel_belief_2026` | ReBel directly frames incomplete observations as belief drift and converts belief-observation discrepancies into dense supervision. | Definition and mechanisms: state/belief drift, observation contamination, partial observability. | Very recent source as of 2026-05-21; use cautiously until independently replicated. |

## Search Notes

- `Rewarding the Path: Alleviating Reward Sparsity for LLM Agents with Pathwise Evaluation` appeared in search results, but the opened arXiv hit did not verify cleanly against the displayed title, so it was not added.
- A withdrawn OpenReview result on decoupled credit assignment was not added as a core source.
- Backtracking/rollback-specific tool-use reward papers remain under-covered in this source set; existing evidence is mostly inference-time self-reflection or replay-style evaluation rather than systematic rollback rewards.

## PDF Status

PDFs were downloaded successfully for all six supplementary sources:

- `pdf/uprop_2025.pdf`
- `pdf/hiper_2026.pdf`
- `pdf/elpo_2026.pdf`
- `pdf/hcapo_2026.pdf`
- `pdf/gigpo_2025.pdf`
- `pdf/rebel_belief_2026.pdf`
