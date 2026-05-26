# Search-R1: Training LLMs to Reason and Leverage Search Engines with Reinforcement Learning

## Metadata
- Authors: Bowen Jin, Hansi Zeng, Zhenrui Yue, Jinsung Yoon, Sercan O. Arik, Dong Wang, Hamed Zamani, Jiawei Han
- Year: 2025
- Source type: arxiv
- URL: https://arxiv.org/abs/2503.09516
- Code: https://github.com/PeterGriffinJin/Search-R1
- Confidence: high

## Problem Setting
- Task: search agent RL
- Tool/API setting: LLM interacts with search engines during reasoning
- Single-turn or multi-turn: multi-turn search-engine interaction for QA reasoning
- Long-horizon? yes

## Training / Optimization Paradigm
- PPO / GRPO / RLOO / REINFORCE / DPO / RLAIF / SFT / inference-time / benchmark-only / other: PPO / GRPO-compatible search-augmented RL
- Is RL actually used, or only evaluation / prompting? Yes; RL optimizes retrieval-augmented reasoning trajectories.

## Reward Signal
- Outcome reward: Final answer correctness via rule-based outcome rewards.
- Process reward: No process reward; the paper intentionally avoids process-based rewards.
- Execution reward: Search engine calls retrieve passages during rollout; retrieved token masking stabilizes optimization.
- Format/schema reward: The paper avoids explicit format rewards in the main design.
- LLM-as-judge: none for training reward
- Preference/contrastive signal: none
- KL/regularization: PPO/GRPO regularization as in RL algorithm; no reward-specific KL extracted
- Step penalty/cost: not specified

## Reward Formula or Pseudocode
Outcome reward assesses correctness of final response; exact-match is used in experiments unless otherwise noted.

## Failure Modes Reported
- Cascade drift / error propagation: unknown
- Silent error / semantic mismatch: unknown
- Reward hacking: unknown
- Format overfitting: unknown
- Judge hacking: unknown
- Exploration collapse: GRPO may converge faster but can become unstable or collapse after many steps.
- Other:
- GRPO may converge faster but can become unstable or collapse after many steps.
- Outcome-only rewards give little signal about query quality.
- Retrieved irrelevant information can mislead reasoning.

## Evidence Quotes
- claim: Search-R1 trains LLMs to generate search queries during reasoning with real-time retrieval.
  supporting URL: https://arxiv.org/abs/2503.09516
  short quote or paraphrase: Search-R1 trains LLMs to generate search queries during reasoning with real-time retrieval.
  confidence: high
- claim: The paper adopts a straightforward outcome-based reward and avoids process rewards.
  supporting URL: https://arxiv.org/abs/2503.09516
  short quote or paraphrase: The paper adopts a straightforward outcome-based reward and avoids process rewards.
  confidence: high
- claim: It reports retrieved token masking and PPO/GRPO variants for stable optimization.
  supporting URL: https://arxiv.org/abs/2503.09516
  short quote or paraphrase: It reports retrieved token masking and PPO/GRPO variants for stable optimization.
  confidence: high

## Relevance to Our Paper
- useful for Part 1: yes - helps classify training/evaluation paradigm and task setting
- useful for Part 2: yes - outcome_answer_correctness
- useful for cascade drift: yes - long-horizon or multi-step setting
- useful for silent errors: yes - useful for semantic/execution mismatch
- useful for gaps / anti-patterns: yes - captures reward/metric limitations or coverage caveats
