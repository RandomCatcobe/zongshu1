# Process Reward Models for LLM Agents: Practical Framework and Directions

## Metadata
- Authors: Xi et al.
- Year: 2025
- Source type: arxiv
- URL: https://arxiv.org/abs/2502.10325
- Code: none found in source entry
- Confidence: high

## Problem Setting
- Task: agent process reward modeling
- Tool/API setting: LLM agents improving through interactions
- Single-turn or multi-turn: turn-level MDPs for LLM agents in external environments
- Long-horizon? yes

## Training / Optimization Paradigm
- PPO / GRPO / RLOO / REINFORCE / DPO / RLAIF / SFT / inference-time / benchmark-only / other: AgentPRM / InversePRM / RL with PRMs
- Is RL actually used, or only evaluation / prompting? Yes; trains PRMs and policies iteratively using RLHF-style updates.

## Reward Signal
- Outcome reward: Outcome rewards from environment rollouts provide Monte Carlo targets for AgentPRM.
- Process reward: PRM estimates Q(s,a), providing turn-wise process rewards/intermediate value estimates.
- Execution reward: Environment rollouts generate states, actions, and terminal/intermediate rewards.
- Format/schema reward: not central
- LLM-as-judge: PRM acts as learned reward/value model; not necessarily LLM-as-judge.
- Preference/contrastive signal: InversePRM can learn from positive/negative transitions/demonstrations.
- KL/regularization: Policy update regularizes against previous policy with KL.
- Step penalty/cost: no simple step cost; discounted cumulative return in Q target

## Reward Formula or Pseudocode
Q^pi(s_t,a_t)=E[sum_{k=t}^T gamma^{k-t} r(s_k,a_k)|s_t,a_t]; train PRM on Monte Carlo Q targets; update policy to maximize Q_phi(s,a)-beta KL(pi||pi_prev).

## Failure Modes Reported
- Cascade drift / error propagation: unknown
- Silent error / semantic mismatch: unknown
- Reward hacking: PRM score can increase while outcome success drops.
- Format overfitting: unknown
- Judge hacking: unknown
- Exploration collapse: Sparse outcomes make exploration and sample efficiency hard.
- Other:
- Reward hacking: PRM score can increase while outcome success drops.
- PRM accuracy degrades under distribution shift if policy moves too far.
- Sparse outcomes make exploration and sample efficiency hard.

## Evidence Quotes
- claim: AgentPRM defines PRMs as state-action value functions for turn-level agent MDPs.
  supporting URL: https://arxiv.org/abs/2502.10325
  short quote or paraphrase: AgentPRM defines PRMs as state-action value functions for turn-level agent MDPs.
  confidence: high
- claim: The algorithm computes PRM targets from rollouts and then trains the policy with RL.
  supporting URL: https://arxiv.org/abs/2502.10325
  short quote or paraphrase: The algorithm computes PRM targets from rollouts and then trains the policy with RL.
  confidence: high
- claim: The paper explicitly analyzes reward hacking and process reward shaping.
  supporting URL: https://arxiv.org/abs/2502.10325
  short quote or paraphrase: The paper explicitly analyzes reward hacking and process reward shaping.
  confidence: high

## Relevance to Our Paper
- useful for Part 1: yes - helps classify training/evaluation paradigm and task setting
- useful for Part 2: yes - process_reward_model / Q_value
- useful for cascade drift: yes - long-horizon or multi-step setting
- useful for silent errors: limited
- useful for gaps / anti-patterns: yes - captures reward/metric limitations or coverage caveats
