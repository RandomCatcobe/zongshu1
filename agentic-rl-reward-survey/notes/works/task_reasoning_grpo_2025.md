# Training Task Reasoning LLM Agents for Multi-turn Task Planning via Single-turn Reinforcement Learning

## Metadata
- Authors: Hanjiang Hu, Changliu Liu, Na Li, Yebin Wang
- Year: 2025
- Source type: arxiv
- URL: https://arxiv.org/abs/2509.20616
- Code: none found in source entry
- Confidence: medium

## Problem Setting
- Task: multi-turn task planning
- Tool/API setting: task reasoning for autonomous agents with tool-use-relevant planning
- Single-turn or multi-turn: multi-turn task planning converted to single-turn task reasoning
- Long-horizon? yes

## Training / Optimization Paradigm
- PPO / GRPO / RLOO / REINFORCE / DPO / RLAIF / SFT / inference-time / benchmark-only / other: GRPO with dense verifiable reward from expert trajectories
- Is RL actually used, or only evaluation / prompting? Yes; GRPO post-training is applied to single-turn reasoning subtasks derived from expert trajectories.

## Reward Signal
- Outcome reward: Multi-turn task success probability and shorter successful episode lengths.
- Process reward: Dense single-turn rewards from expert trajectories provide local training signal.
- Execution reward: Robotouille/task-planning environment evaluates actions.
- Format/schema reward: ReAct format and single-turn correctness reward used in training data.
- LLM-as-judge: none central
- Preference/contrastive signal: none
- KL/regularization: not specified in note
- Step penalty/cost: indirectly optimizes minimal-turn success; no explicit step penalty extracted

## Reward Formula or Pseudocode
Single-turn reward r_piGT(s,a) in {0,1} indicates whether action matches expert trajectory action for state; GRPO improves this single-turn reward and lower-bounds multi-turn success.

## Failure Modes Reported
- Cascade drift / error propagation: Sparse episode rewards and long-horizon credit assignment motivate the decomposition.
- Silent error / semantic mismatch: unknown
- Reward hacking: unknown
- Format overfitting: unknown
- Judge hacking: unknown
- Exploration collapse: Sparse episode rewards and long-horizon credit assignment motivate the decomposition.
- Other:
- Depends on expert trajectories and unique optimal action assumptions.
- Sparse episode rewards and long-horizon credit assignment motivate the decomposition.
- Noisy non-expert trajectories can degrade performance, though GRPO shows robustness.

## Evidence Quotes
- claim: The paper transforms multi-turn planning into single-turn task reasoning problems.
  supporting URL: https://arxiv.org/abs/2509.20616
  short quote or paraphrase: The paper transforms multi-turn planning into single-turn task reasoning problems.
  confidence: medium
- claim: It uses GRPO with dense verifiable rewards from expert trajectories.
  supporting URL: https://arxiv.org/abs/2509.20616
  short quote or paraphrase: It uses GRPO with dense verifiable rewards from expert trajectories.
  confidence: medium
- claim: The theoretical analysis connects single-turn reward improvement to multi-turn success probability.
  supporting URL: https://arxiv.org/abs/2509.20616
  short quote or paraphrase: The theoretical analysis connects single-turn reward improvement to multi-turn success probability.
  confidence: medium

## Relevance to Our Paper
- useful for Part 1: yes - helps classify training/evaluation paradigm and task setting
- useful for Part 2: yes - expert_trajectory_verifiable_reward
- useful for cascade drift: yes - long-horizon or multi-step setting
- useful for silent errors: limited
- useful for gaps / anti-patterns: yes - captures reward/metric limitations or coverage caveats
