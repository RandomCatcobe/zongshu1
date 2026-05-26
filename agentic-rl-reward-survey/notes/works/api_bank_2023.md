# API-Bank: A Comprehensive Benchmark for Tool-Augmented LLMs

## Metadata
- Authors: Li et al.
- Year: 2023
- Source type: arxiv
- URL: https://arxiv.org/abs/2304.08244
- Code: none found in source entry
- Confidence: high

## Problem Setting
- Task: API use benchmark
- Tool/API setting: tool-augmented LLMs with API calls
- Single-turn or multi-turn: tool-augmented LLM benchmark with API calls across levels
- Long-horizon? mixed

## Training / Optimization Paradigm
- PPO / GRPO / RLOO / REINFORCE / DPO / RLAIF / SFT / inference-time / benchmark-only / other: benchmark-only
- Is RL actually used, or only evaluation / prompting? Only evaluation; no policy RL training method is introduced.

## Reward Signal
- Outcome reward: Task completion and answer/API-call correctness.
- Process reward: No process reward model.
- Execution reward: API execution/correct API use is part of evaluation.
- Format/schema reward: API call structure and arguments are evaluated.
- LLM-as-judge: not central / unknown
- Preference/contrastive signal: none
- KL/regularization: not specified
- Step penalty/cost: not specified

## Reward Formula or Pseudocode
not specified in current extraction; benchmark scores API call correctness and task completion.

## Failure Modes Reported
- Cascade drift / error propagation: Coverage of realistic long-horizon user interaction is limited compared with later benchmarks.
- Silent error / semantic mismatch: Benchmark metrics can conflate syntactic API correctness with semantic task success.
- Reward hacking: unknown
- Format overfitting: unknown
- Judge hacking: unknown
- Exploration collapse: unknown
- Other:
- Benchmark metrics can conflate syntactic API correctness with semantic task success.
- Coverage of realistic long-horizon user interaction is limited compared with later benchmarks.
- APIBench/API-Bank naming ambiguity can pollute taxonomy if not kept separate.

## Evidence Quotes
- claim: API-Bank is a benchmark for tool-augmented LLMs with API calls.
  supporting URL: https://arxiv.org/abs/2304.08244
  short quote or paraphrase: API-Bank is a benchmark for tool-augmented LLMs with API calls.
  confidence: high
- claim: The source is canonical for the API-Bank entry, distinct from ToolBench.
  supporting URL: https://arxiv.org/abs/2304.08244
  short quote or paraphrase: The source is canonical for the API-Bank entry, distinct from ToolBench.
  confidence: high
- claim: It is benchmark-only and should not be described as RL training.
  supporting URL: https://arxiv.org/abs/2304.08244
  short quote or paraphrase: It is benchmark-only and should not be described as RL training.
  confidence: high

## Relevance to Our Paper
- useful for Part 1: yes - helps classify training/evaluation paradigm and task setting
- useful for Part 2: yes - API_correctness / benchmark_metric
- useful for cascade drift: limited
- useful for silent errors: yes - useful for semantic/execution mismatch
- useful for gaps / anti-patterns: yes - captures reward/metric limitations or coverage caveats
