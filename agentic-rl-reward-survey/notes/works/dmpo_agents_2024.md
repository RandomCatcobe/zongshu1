# Direct Multi-Turn Preference Optimization for Language Agents

## Metadata
- Authors: Chen et al.
- Year: 2024
- Source type: arxiv
- URL: https://arxiv.org/abs/2406.14868
- Code: none found in source entry
- Confidence: high

## Problem Setting
- Task: language agent preference optimization
- Tool/API setting: multi-turn language agents
- Single-turn or multi-turn: multi-turn language-agent trajectories
- Long-horizon? yes

## Training / Optimization Paradigm
- PPO / GRPO / RLOO / REINFORCE / DPO / RLAIF / SFT / inference-time / benchmark-only / other: DPO / Direct Multi-Turn Preference Optimization
- Is RL actually used, or only evaluation / prompting? Yes in the broad preference-optimization sense; directly optimizes policy from trajectory preferences without separately learning a reward model.

## Reward Signal
- Outcome reward: Final binary task rewards create preferred and dispreferred trajectories.
- Process reward: No step-level PRM; preference objective accounts for trajectory length/turn structure.
- Execution reward: Agent environments produce rewards for trajectories.
- Format/schema reward: not central
- LLM-as-judge: not central
- Preference/contrastive signal: Trajectory-level preference pairs are the primary signal.
- KL/regularization: DPO-style implicit reference-policy regularization
- Step penalty/cost: discount factor and length normalization affect turn weighting

## Reward Formula or Pseudocode
DMPO loss increases likelihood of preferred trajectory tau_w and decreases likelihood of dispreferred tau_l with discounted/length-normalized trajectory log-ratio terms.

## Failure Modes Reported
- Cascade drift / error propagation: Behavior cloning suffers compounding errors in dynamic environments.
- Silent error / semantic mismatch: unknown
- Reward hacking: unknown
- Format overfitting: unknown
- Judge hacking: unknown
- Exploration collapse: unknown
- Other:
- Behavior cloning suffers compounding errors in dynamic environments.
- Noisy preference data and trajectory length disparity affect optimization.
- Trajectory-level preferences still provide limited local credit assignment.

## Evidence Quotes
- claim: DMPO extends DPO to multi-turn language-agent tasks.
  supporting URL: https://arxiv.org/abs/2406.14868
  short quote or paraphrase: DMPO extends DPO to multi-turn language-agent tasks.
  confidence: high
- claim: The paper builds preference data from win/loss trajectories and final rewards.
  supporting URL: https://arxiv.org/abs/2406.14868
  short quote or paraphrase: The paper builds preference data from win/loss trajectories and final rewards.
  confidence: high
- claim: It directly targets compounding errors from behavior cloning.
  supporting URL: https://arxiv.org/abs/2406.14868
  short quote or paraphrase: It directly targets compounding errors from behavior cloning.
  confidence: high

## Relevance to Our Paper
- useful for Part 1: yes - helps classify training/evaluation paradigm and task setting
- useful for Part 2: yes - trajectory_preference_optimization
- useful for cascade drift: yes - long-horizon or multi-step setting
- useful for silent errors: limited
- useful for gaps / anti-patterns: yes - captures reward/metric limitations or coverage caveats
