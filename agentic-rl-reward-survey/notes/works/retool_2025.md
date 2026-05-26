# ReTool: Reinforcement Learning for Strategic Tool Use in LLMs

## Metadata
- Authors: Jiazhan Feng, Shijue Huang, Xingwei Qu, Ge Zhang, Yujia Qin, Baoquan Zhong, Chengquan Jiang, Jinxin Chi, Wanjun Zhong
- Year: 2025
- Source type: arxiv
- URL: https://arxiv.org/abs/2504.11536
- Code: none found in source entry
- Confidence: high

## Problem Setting
- Task: strategic tool use
- Tool/API setting: long-form reasoning with code/tool execution interleaved with natural language reasoning
- Single-turn or multi-turn: long-form reasoning with interleaved code/tool execution
- Long-horizon? yes

## Training / Optimization Paradigm
- PPO / GRPO / RLOO / REINFORCE / DPO / RLAIF / SFT / inference-time / benchmark-only / other: RL with tool-integrated rollouts
- Is RL actually used, or only evaluation / prompting? Yes; RL training uses outcome rewards while rollouts include real-time code execution.

## Reward Signal
- Outcome reward: Task outcome reward, mainly final math answer correctness.
- Process reward: No explicit process reward; emergent self-correction is analyzed.
- Execution reward: Code interpreter execution is interleaved during rollouts.
- Format/schema reward: Tool invocation format required operationally; no standalone format reward extracted.
- LLM-as-judge: not central
- Preference/contrastive signal: none
- KL/regularization: not specified in current extraction
- Step penalty/cost: not specified

## Reward Formula or Pseudocode
r = outcome_correctness(final answer) during multi-turn code-execution rollout; exact scalar scale not specified in note extraction.

## Failure Modes Reported
- Cascade drift / error propagation: Outcome-only reward is sparse for long tool-use trajectories.
- Silent error / semantic mismatch: unknown
- Reward hacking: unknown
- Format overfitting: unknown
- Judge hacking: unknown
- Exploration collapse: Outcome-only reward is sparse for long tool-use trajectories.
- Other:
- Outcome-only reward is sparse for long tool-use trajectories.
- Bad code execution can compound unless the model learns self-correction.
- Models can misuse tools or fall back to brittle heuristics without RL exploration.

## Evidence Quotes
- claim: ReTool interleaves natural-language reasoning with real-time code execution.
  supporting URL: https://arxiv.org/abs/2504.11536
  short quote or paraphrase: ReTool interleaves natural-language reasoning with real-time code execution.
  confidence: high
- claim: RL training leverages task outcomes as rewards to refine tool-use strategy.
  supporting URL: https://arxiv.org/abs/2504.11536
  short quote or paraphrase: RL training leverages task outcomes as rewards to refine tool-use strategy.
  confidence: high
- claim: The paper analyzes emergent code self-correction under outcome-driven tool integration.
  supporting URL: https://arxiv.org/abs/2504.11536
  short quote or paraphrase: The paper analyzes emergent code self-correction under outcome-driven tool integration.
  confidence: high

## Relevance to Our Paper
- useful for Part 1: yes - helps classify training/evaluation paradigm and task setting
- useful for Part 2: yes - outcome_reward / execution_augmented_RL
- useful for cascade drift: yes - long-horizon or multi-step setting
- useful for silent errors: yes - useful for semantic/execution mismatch
- useful for gaps / anti-patterns: yes - captures reward/metric limitations or coverage caveats
