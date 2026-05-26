# AgentTuning: Enabling Generalized Agent Abilities for LLMs

## Metadata
- Authors: Zeng et al.
- Year: 2023
- Source type: arxiv
- URL: https://arxiv.org/abs/2310.12823
- Code: https://github.com/THUDM/AgentTuning
- Confidence: high

## Problem Setting
- Task: general agent ability training
- Tool/API setting: mixed agent tasks collected as AgentInstruct
- Single-turn or multi-turn: mixed agent tasks in AgentInstruct
- Long-horizon? mixed

## Training / Optimization Paradigm
- PPO / GRPO / RLOO / REINFORCE / DPO / RLAIF / SFT / inference-time / benchmark-only / other: SFT / agent instruction tuning
- Is RL actually used, or only evaluation / prompting? No RL reward in the core method.

## Reward Signal
- Outcome reward: Agent benchmark metrics after instruction tuning.
- Process reward: No process reward model.
- Execution reward: Some agent tasks involve interaction/execution, but reward details are benchmark-level.
- Format/schema reward: task-specific
- LLM-as-judge: not central
- Preference/contrastive signal: none
- KL/regularization: not specified
- Step penalty/cost: not specified

## Reward Formula or Pseudocode
not specified

## Failure Modes Reported
- Cascade drift / error propagation: unknown
- Silent error / semantic mismatch: unknown
- Reward hacking: unknown
- Format overfitting: unknown
- Judge hacking: unknown
- Exploration collapse: unknown
- Other:
- Instruction tuning can improve average behavior without explicit credit assignment.
- Benchmark aggregation can hide which agent skills fail.
- No reward design for online correction is provided.

## Evidence Quotes
- claim: AgentTuning is a representative agent instruction-tuning baseline.
  supporting URL: https://arxiv.org/abs/2310.12823
  short quote or paraphrase: AgentTuning is a representative agent instruction-tuning baseline.
  confidence: high
- claim: The source table classifies it as SFT, not RL.
  supporting URL: https://arxiv.org/abs/2310.12823
  short quote or paraphrase: The source table classifies it as SFT, not RL.
  confidence: high
- claim: It helps separate supervised agent training from reward-optimized training.
  supporting URL: https://arxiv.org/abs/2310.12823
  short quote or paraphrase: It helps separate supervised agent training from reward-optimized training.
  confidence: high

## Relevance to Our Paper
- useful for Part 1: yes - helps classify training/evaluation paradigm and task setting
- useful for Part 2: yes - benchmark_metric / SFT_evaluation
- useful for cascade drift: yes - long-horizon or multi-step setting
- useful for silent errors: limited
- useful for gaps / anti-patterns: yes - captures reward/metric limitations or coverage caveats
