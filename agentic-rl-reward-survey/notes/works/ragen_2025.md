# RAGEN: Understanding Self-Evolution in LLM Agents via Multi-Turn Reinforcement Learning

## Metadata
- Authors: Zihan Wang et al.
- Year: 2025
- Source type: arxiv
- URL: https://arxiv.org/abs/2504.20073
- Code: none found in source entry
- Confidence: high

## Problem Setting
- Task: multi-turn LLM agent RL
- Tool/API setting: modular training and evaluation system for multi-turn LLM agents
- Single-turn or multi-turn: multi-turn LLM agents in stochastic and deterministic environments
- Long-horizon? yes

## Training / Optimization Paradigm
- PPO / GRPO / RLOO / REINFORCE / DPO / RLAIF / SFT / inference-time / benchmark-only / other: StarPO / trajectory-level RL
- Is RL actually used, or only evaluation / prompting? Yes; RAGEN implements multi-turn trajectory-level agent RL.

## Reward Signal
- Outcome reward: Trajectory-level environment reward over state-thinking-actions-reward rollouts.
- Process reward: Studies lack of fine-grained reasoning-aware rewards; StarPO-S stabilizes training but is not a PRM.
- Execution reward: Environment interactions provide stochastic feedback.
- Format/schema reward: environment/prompt format matters but not central
- LLM-as-judge: not central
- Preference/contrastive signal: none
- KL/regularization: not specified
- Step penalty/cost: stability mechanisms rather than explicit step cost

## Reward Formula or Pseudocode
StarPO views rollouts as state-thinking-actions-reward trajectories and optimizes trajectory-level rewards; exact implementation formula not summarized here.

## Failure Modes Reported
- Cascade drift / error propagation: unknown
- Silent error / semantic mismatch: Without fine-grained reasoning-aware rewards, agents may learn shallow strategies or hallucinated thoughts.
- Reward hacking: unknown
- Format overfitting: unknown
- Judge hacking: unknown
- Exploration collapse: Rollout diversity and interaction granularity affect stability.
- Other:
- Echo Trap: reward variability cliffs and gradient spikes.
- Without fine-grained reasoning-aware rewards, agents may learn shallow strategies or hallucinated thoughts.
- Rollout diversity and interaction granularity affect stability.

## Evidence Quotes
- claim: RAGEN proposes StarPO for state-thinking-actions-reward policy optimization.
  supporting URL: https://arxiv.org/abs/2504.20073
  short quote or paraphrase: RAGEN proposes StarPO for state-thinking-actions-reward policy optimization.
  confidence: high
- claim: The study identifies Echo Trap as a recurring instability pattern.
  supporting URL: https://arxiv.org/abs/2504.20073
  short quote or paraphrase: The study identifies Echo Trap as a recurring instability pattern.
  confidence: high
- claim: It argues fine-grained reasoning-aware reward signals are needed for agent reasoning to emerge.
  supporting URL: https://arxiv.org/abs/2504.20073
  short quote or paraphrase: It argues fine-grained reasoning-aware reward signals are needed for agent reasoning to emerge.
  confidence: high

## Relevance to Our Paper
- useful for Part 1: yes - helps classify training/evaluation paradigm and task setting
- useful for Part 2: yes - trajectory_level_agent_RL
- useful for cascade drift: yes - long-horizon or multi-step setting
- useful for silent errors: limited
- useful for gaps / anti-patterns: yes - captures reward/metric limitations or coverage caveats
