# Synthetic Data Generation and Multi-Step RL for Reasoning and Tool Use

## Metadata
- Authors: Anna Goldie, Azalia Mirhoseini, Hao Zhou, Irene Cai, Christopher D. Manning
- Year: 2025
- Source type: arxiv
- URL: https://arxiv.org/abs/2504.04736
- Code: none found in source entry
- Confidence: medium

## Problem Setting
- Task: multi-step reasoning and tool use
- Tool/API setting: multi-step reasoning and tool-use trajectories
- Single-turn or multi-turn: multi-step reasoning and tool-use trajectories with search/calculator tools
- Long-horizon? yes

## Training / Optimization Paradigm
- PPO / GRPO / RLOO / REINFORCE / DPO / RLAIF / SFT / inference-time / benchmark-only / other: offline step-wise RL with generative reward model
- Is RL actually used, or only evaluation / prompting? Yes; policy-gradient-style RL optimizes step-wise rewards on synthetic sub-trajectories.

## Reward Signal
- Outcome reward: Final task accuracy on QA/math/tool-use benchmarks.
- Process reward: Generative reward model scores each action/sub-trajectory given context.
- Execution reward: Synthetic trajectories include tool/environment responses generated offline.
- Format/schema reward: tool-call tags are parsed; no standalone schema reward emphasized.
- LLM-as-judge: Gemini 1.5 Pro acts as generative reward model.
- Preference/contrastive signal: model-based judgments rather than human preferences
- KL/regularization: uses Gemma RLHF policy-gradient algorithm; KL details not extracted
- Step penalty/cost: step-wise reward at each action

## Reward Formula or Pseudocode
J(theta)=E_{s in T, a~pi_theta(s)}[R(a|s)], where R(a|s) comes from a generative reward model over each step context.

## Failure Modes Reported
- Cascade drift / error propagation: unknown
- Silent error / semantic mismatch: unknown
- Reward hacking: unknown
- Format overfitting: unknown
- Judge hacking: unknown
- Exploration collapse: unknown
- Other:
- Generative reward model can be overly permissive or harsh.
- Incorrect intermediate steps compound across multi-step tasks.
- Offline synthetic data quality and filtering strongly affect performance.

## Evidence Quotes
- claim: SWiRL decomposes multi-step trajectories into sub-trajectories for step-wise optimization.
  supporting URL: https://arxiv.org/abs/2504.04736
  short quote or paraphrase: SWiRL decomposes multi-step trajectories into sub-trajectories for step-wise optimization.
  confidence: medium
- claim: Its objective maximizes expected step-wise rewards from a generative reward model.
  supporting URL: https://arxiv.org/abs/2504.04736
  short quote or paraphrase: Its objective maximizes expected step-wise rewards from a generative reward model.
  confidence: medium
- claim: The paper reports that process-filtered data works best and that multi-step errors compound.
  supporting URL: https://arxiv.org/abs/2504.04736
  short quote or paraphrase: The paper reports that process-filtered data works best and that multi-step errors compound.
  confidence: medium

## Relevance to Our Paper
- useful for Part 1: yes - helps classify training/evaluation paradigm and task setting
- useful for Part 2: yes - step_wise_generative_reward
- useful for cascade drift: yes - long-horizon or multi-step setting
- useful for silent errors: limited
- useful for gaps / anti-patterns: yes - captures reward/metric limitations or coverage caveats
