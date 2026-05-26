# ToolRL: Reward is All Tool Learning Needs

## Metadata
- Authors: Cheng Qian, Emre Can Acikgoz, Qi He, Hongru Wang, Xiusi Chen, Dilek Hakkani-Tur, Gokhan Tur, Heng Ji
- Year: 2025
- Source type: arxiv
- URL: https://arxiv.org/abs/2504.13958
- Code: none found in source entry
- Confidence: medium

## Problem Setting
- Task: tool learning RL
- Tool/API setting: LLM tool learning
- Single-turn or multi-turn: tool selection and application tasks
- Long-horizon? mixed

## Training / Optimization Paradigm
- PPO / GRPO / RLOO / REINFORCE / DPO / RLAIF / SFT / inference-time / benchmark-only / other: GRPO / PPO reward-design study
- Is RL actually used, or only evaluation / prompting? Yes; trains tool-use policies with GRPO and evaluates PPO.

## Reward Signal
- Outcome reward: Correctness reward over predicted tool calls versus ground truth.
- Process reward: No PRM; reward is decomposed by tool-call components.
- Execution reward: Tool-use tasks evaluate selected tools and parameters; execution signal is benchmark-dependent.
- Format/schema reward: Binary format reward checks required fields/tokens in correct order.
- LLM-as-judge: none central
- Preference/contrastive signal: none
- KL/regularization: They omit KL penalty against a reference model in their GRPO formulation.
- Step penalty/cost: temporal scaling of format/correctness rewards studied; no simple step penalty.

## Reward Formula or Pseudocode
R_final = R_format + R_correct; R_format in {0,1}; R_correct = 6*(R_max/S_max)-3 over name, parameter-name, and parameter-value matching.

## Failure Modes Reported
- Cascade drift / error propagation: unknown
- Silent error / semantic mismatch: unknown
- Reward hacking: Overweighting format can lead to format learning without task correctness.
- Format overfitting: Overweighting format can lead to format learning without task correctness.
- Judge hacking: unknown
- Exploration collapse: Coarse correctness rewards are sparse for multi-component tool calls.
- Other:
- Coarse correctness rewards are sparse for multi-component tool calls.
- Overweighting format can lead to format learning without task correctness.
- SFT initialization can yield high training rewards but weaker generalization.

## Evidence Quotes
- claim: ToolRL explicitly studies reward design for tool selection and application tasks.
  supporting URL: https://arxiv.org/abs/2504.13958
  short quote or paraphrase: ToolRL explicitly studies reward design for tool selection and application tasks.
  confidence: medium
- claim: It decomposes reward into format and correctness components.
  supporting URL: https://arxiv.org/abs/2504.13958
  short quote or paraphrase: It decomposes reward into format and correctness components.
  confidence: medium
- claim: The correctness reward includes tool-name, parameter-name, and parameter-content matching.
  supporting URL: https://arxiv.org/abs/2504.13958
  short quote or paraphrase: The correctness reward includes tool-name, parameter-name, and parameter-content matching.
  confidence: medium

## Relevance to Our Paper
- useful for Part 1: yes - helps classify training/evaluation paradigm and task setting
- useful for Part 2: yes - format_plus_correctness_reward
- useful for cascade drift: limited
- useful for silent errors: limited
- useful for gaps / anti-patterns: yes - captures reward/metric limitations or coverage caveats
