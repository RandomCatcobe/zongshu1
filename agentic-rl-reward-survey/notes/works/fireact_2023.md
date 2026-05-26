# FireAct: Toward Language Agent Fine-tuning

## Metadata
- Authors: Chen et al.
- Year: 2023
- Source type: arxiv
- URL: https://arxiv.org/abs/2310.05915
- Code: none found in source entry
- Confidence: high

## Problem Setting
- Task: search-based QA agent
- Tool/API setting: question answering with Google Search API and ReAct-style trajectories
- Single-turn or multi-turn: search-based QA trajectories with ReAct-style tool use
- Long-horizon? mixed

## Training / Optimization Paradigm
- PPO / GRPO / RLOO / REINFORCE / DPO / RLAIF / SFT / inference-time / benchmark-only / other: SFT / trajectory fine-tuning
- Is RL actually used, or only evaluation / prompting? No core RL reward; fine-tunes on agent trajectories.

## Reward Signal
- Outcome reward: QA task performance and trajectory quality.
- Process reward: No learned process reward.
- Execution reward: Search API observations support agent trajectories.
- Format/schema reward: ReAct-style format is prompted/fine-tuned; no separate reward extracted.
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
- Exploration collapse: QA correctness remains sparse at the final answer level.
- Other:
- Fine-tuning may imitate trajectories without learning when to recover.
- Search observations can be misread or over-trusted.
- QA correctness remains sparse at the final answer level.

## Evidence Quotes
- claim: FireAct studies language-agent fine-tuning with search-based QA trajectories.
  supporting URL: https://arxiv.org/abs/2310.05915
  short quote or paraphrase: FireAct studies language-agent fine-tuning with search-based QA trajectories.
  confidence: high
- claim: The current taxonomy treats it as SFT/trajectory fine-tuning.
  supporting URL: https://arxiv.org/abs/2310.05915
  short quote or paraphrase: The current taxonomy treats it as SFT/trajectory fine-tuning.
  confidence: high
- claim: It is relevant for comparing imitation-based agents against RL-trained search agents.
  supporting URL: https://arxiv.org/abs/2310.05915
  short quote or paraphrase: It is relevant for comparing imitation-based agents against RL-trained search agents.
  confidence: high

## Relevance to Our Paper
- useful for Part 1: yes - helps classify training/evaluation paradigm and task setting
- useful for Part 2: yes - QA_accuracy / trajectory_quality
- useful for cascade drift: yes - long-horizon or multi-step setting
- useful for silent errors: yes - useful for semantic/execution mismatch
- useful for gaps / anti-patterns: yes - captures reward/metric limitations or coverage caveats
