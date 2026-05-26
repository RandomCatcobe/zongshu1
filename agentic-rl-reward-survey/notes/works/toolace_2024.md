# ToolACE: Winning the Points of LLM Function Calling

## Metadata
- Authors: Liu et al.
- Year: 2024
- Source type: arxiv
- URL: https://arxiv.org/abs/2409.00920
- Code: none found in source entry
- Confidence: high

## Problem Setting
- Task: function calling data generation
- Tool/API setting: function calling and tool-learning data pipeline
- Single-turn or multi-turn: function-calling data generation and model training
- Long-horizon? mixed

## Training / Optimization Paradigm
- PPO / GRPO / RLOO / REINFORCE / DPO / RLAIF / SFT / inference-time / benchmark-only / other: agentic data generation / SFT
- Is RL actually used, or only evaluation / prompting? No verified RL training in the core method.

## Reward Signal
- Outcome reward: Function-calling benchmark performance after training on generated data.
- Process reward: Data-generation pipeline contains quality controls, but no PRM extraction here.
- Execution reward: Function/tool correctness is evaluated through benchmarks.
- Format/schema reward: Function-call format correctness is central.
- LLM-as-judge: not fully extracted
- Preference/contrastive signal: none
- KL/regularization: not specified
- Step penalty/cost: not specified

## Reward Formula or Pseudocode
not specified

## Failure Modes Reported
- Cascade drift / error propagation: unknown
- Silent error / semantic mismatch: Function-call syntax can improve without robust semantic tool use.
- Reward hacking: Generated data quality can overfit to benchmark formats.
- Format overfitting: Generated data quality can overfit to benchmark formats.
- Judge hacking: unknown
- Exploration collapse: unknown
- Other:
- Generated data quality can overfit to benchmark formats.
- Function-call syntax can improve without robust semantic tool use.
- No online reward signal for recovery from tool errors is reported.

## Evidence Quotes
- claim: ToolACE presents an automatic pipeline for function-calling data.
  supporting URL: https://arxiv.org/abs/2409.00920
  short quote or paraphrase: ToolACE presents an automatic pipeline for function-calling data.
  confidence: high
- claim: The current source table marks it as agentic data generation/SFT rather than RL.
  supporting URL: https://arxiv.org/abs/2409.00920
  short quote or paraphrase: The current source table marks it as agentic data generation/SFT rather than RL.
  confidence: high
- claim: It is relevant to benchmark overfitting and format-oriented reward signals.
  supporting URL: https://arxiv.org/abs/2409.00920
  short quote or paraphrase: It is relevant to benchmark overfitting and format-oriented reward signals.
  confidence: high

## Relevance to Our Paper
- useful for Part 1: yes - helps classify training/evaluation paradigm and task setting
- useful for Part 2: yes - data_quality / function_call_benchmark
- useful for cascade drift: limited
- useful for silent errors: yes - useful for semantic/execution mismatch
- useful for gaps / anti-patterns: yes - captures reward/metric limitations or coverage caveats
