# Reinforcing Multi-Turn Reasoning in LLM Agents via Turn-Level Credit Assignment

## Metadata
- Authors: Quan Wei, Siliang Zeng, Chenliang Li, William Brown, Oana Frunza, Wei Deng, Anderson Schneider, Yuriy Nevmyvaka, Yang Katie Zhao, Alfredo Garcia, Mingyi Hong
- Year: 2025
- Source type: arxiv
- URL: https://arxiv.org/abs/2505.11821
- Code: none found in source entry
- Confidence: high

## Problem Setting
- Task: multi-turn reasoning and search tool-use RL
- Tool/API setting: multi-turn reasoning agents and search-based tool-use tasks
- Single-turn or multi-turn: multi-turn reasoning/search agents with turn-level rewards
- Long-horizon? yes

## Training / Optimization Paradigm
- PPO / GRPO / RLOO / REINFORCE / DPO / RLAIF / SFT / inference-time / benchmark-only / other: MT-GRPO / MT-PPO with turn-level credit assignment
- Is RL actually used, or only evaluation / prompting? Yes; extends GRPO and PPO to incorporate intermediate turn rewards.

## Reward Signal
- Outcome reward: Final answer exact match and format correctness.
- Process reward: Turn-level intermediate rewards include retrieval correctness, tool execution, format, and LLM-as-judge variants.
- Execution reward: Search/tool execution provides intermediate feedback.
- Format/schema reward: Outcome and intermediate format rewards are explicit.
- LLM-as-judge: LLM-as-judge rewards are studied alongside verifiable rewards.
- Preference/contrastive signal: none central
- KL/regularization: PPO/GRPO regularization as algorithmic component
- Step penalty/cost: search count reward penalizes excessive search usage

## Reward Formula or Pseudocode
Example outcome reward: 1 if exact match and format correct, 0.2 if answer wrong but format correct, -1 if format incorrect. Intermediate reward = retrieval reward + format reward + search-count penalty.

## Failure Modes Reported
- Cascade drift / error propagation: Trajectory-level reward aggregation causes poor credit assignment.
- Silent error / semantic mismatch: unknown
- Reward hacking: unknown
- Format overfitting: unknown
- Judge hacking: Verifiable rewards are rigid; LLM-as-judge is more flexible but may be noisy.
- Exploration collapse: unknown
- Other:
- Trajectory-level reward aggregation causes poor credit assignment.
- GRPO with turn-level advantages can require exponential rollout samples in general K-turn settings.
- Verifiable rewards are rigid; LLM-as-judge is more flexible but may be noisy.

## Evidence Quotes
- claim: The paper designs turn-level rewards for multi-turn RL algorithms and agent applications.
  supporting URL: https://arxiv.org/abs/2505.11821
  short quote or paraphrase: The paper designs turn-level rewards for multi-turn RL algorithms and agent applications.
  confidence: high
- claim: It uses verifiable and LLM-as-judge rewards in search-agent case studies.
  supporting URL: https://arxiv.org/abs/2505.11821
  short quote or paraphrase: It uses verifiable and LLM-as-judge rewards in search-agent case studies.
  confidence: high
- claim: It reports more stable training and better credit assignment with turn-level rewards.
  supporting URL: https://arxiv.org/abs/2505.11821
  short quote or paraphrase: It reports more stable training and better credit assignment with turn-level rewards.
  confidence: high

## Relevance to Our Paper
- useful for Part 1: yes - helps classify training/evaluation paradigm and task setting
- useful for Part 2: yes - turn_level_verifiable_and_judge_reward
- useful for cascade drift: yes - long-horizon or multi-step setting
- useful for silent errors: yes - useful for semantic/execution mismatch
- useful for gaps / anti-patterns: yes - captures reward/metric limitations or coverage caveats
