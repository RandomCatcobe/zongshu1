# APIGen: Automated Pipeline for Generating Verifiable and Diverse Function-Calling Datasets

## Metadata
- Authors: Liu et al.
- Year: 2024
- Source type: arxiv
- URL: https://arxiv.org/abs/2406.18518
- Code: none found in source entry
- Confidence: high

## Problem Setting
- Task: function calling data generation
- Tool/API setting: executable APIs and generated function-calling data
- Single-turn or multi-turn: verifiable function-calling dataset generation
- Long-horizon? mostly single-turn plus parallel/multiple calls

## Training / Optimization Paradigm
- PPO / GRPO / RLOO / REINFORCE / DPO / RLAIF / SFT / inference-time / benchmark-only / other: data generation / SFT
- Is RL actually used, or only evaluation / prompting? No RL training; verification stages serve as data filters and evaluation analogues.

## Reward Signal
- Outcome reward: Data point passes format, execution, and semantic verification.
- Process reward: Multi-stage verification resembles process filtering, not a trained PRM.
- Execution reward: Actual function execution checks whether calls run successfully.
- Format/schema reward: JSON format checker verifies parseability and required fields.
- LLM-as-judge: Semantic checker is another LLM judging alignment between query, calls, and execution results.
- Preference/contrastive signal: none
- KL/regularization: not specified
- Step penalty/cost: not specified

## Reward Formula or Pseudocode
Accept generated sample iff format_check && execution_check && semantic_check.

## Failure Modes Reported
- Cascade drift / error propagation: unknown
- Silent error / semantic mismatch: Format and execution checks filter many errors but semantic checker quality still matters.
- Reward hacking: unknown
- Format overfitting: unknown
- Judge hacking: unknown
- Exploration collapse: unknown
- Other:
- Format and execution checks filter many errors but semantic checker quality still matters.
- Execution success alone may not imply the user intent was satisfied.
- Generated data may inherit API/library coverage bias.

## Evidence Quotes
- claim: APIGen uses format checking, actual function execution, and semantic verification.
  supporting URL: https://arxiv.org/abs/2406.18518
  short quote or paraphrase: APIGen uses format checking, actual function execution, and semantic verification.
  confidence: high
- claim: The semantic checker evaluates alignment between execution results and query objectives.
  supporting URL: https://arxiv.org/abs/2406.18518
  short quote or paraphrase: The semantic checker evaluates alignment between execution results and query objectives.
  confidence: high
- claim: This is highly relevant for separating format, execution, and semantic reward signals.
  supporting URL: https://arxiv.org/abs/2406.18518
  short quote or paraphrase: This is highly relevant for separating format, execution, and semantic reward signals.
  confidence: high

## Relevance to Our Paper
- useful for Part 1: yes - helps classify training/evaluation paradigm and task setting
- useful for Part 2: yes - format_execution_semantic_verification
- useful for cascade drift: limited
- useful for silent errors: yes - useful for semantic/execution mismatch
- useful for gaps / anti-patterns: yes - captures reward/metric limitations or coverage caveats
