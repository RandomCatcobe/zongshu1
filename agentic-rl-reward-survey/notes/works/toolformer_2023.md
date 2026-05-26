# Toolformer: Language Models Can Teach Themselves to Use Tools

## Metadata
- Authors: Schick et al.
- Year: 2023
- Source type: arxiv
- URL: https://arxiv.org/abs/2302.04761
- Code: none found in source entry
- Confidence: high

## Problem Setting
- Task: tool use
- Tool/API setting: simple APIs such as calculator, QA, search, translation, and calendar tools
- Single-turn or multi-turn: mostly single-call examples in self-supervised API-augmented text
- Long-horizon? no

## Training / Optimization Paradigm
- PPO / GRPO / RLOO / REINFORCE / DPO / RLAIF / SFT / inference-time / benchmark-only / other: self-supervised finetuning / data filtering; not PPO-style RL
- Is RL actually used, or only evaluation / prompting? No gradient RL; API calls are selected by likelihood-improvement filtering during data construction.

## Reward Signal
- Outcome reward: Downstream task accuracy and language-model likelihood improvement are used as selection/evaluation signals.
- Process reward: No explicit process reward; candidate API calls are filtered at the call/result level.
- Execution reward: API execution is used to insert tool results, but not as an RL environment reward.
- Format/schema reward: API-call syntax must be parseable enough for the data pipeline; no standalone schema reward is reported.
- LLM-as-judge: none reported
- Preference/contrastive signal: none reported
- KL/regularization: not specified
- Step penalty/cost: not specified

## Reward Formula or Pseudocode
Keep/generated API call candidates when inserting the API result improves LM loss over the original text by a threshold; exact reward is a filtering criterion, not an RL reward.

## Failure Modes Reported
- Cascade drift / error propagation: unknown
- Silent error / semantic mismatch: No multi-turn credit assignment or semantic task-level reward is modeled.
- Reward hacking: unknown
- Format overfitting: unknown
- Judge hacking: unknown
- Exploration collapse: unknown
- Other:
- API-use decision can be myopic because each call is filtered locally.
- No multi-turn credit assignment or semantic task-level reward is modeled.
- Bad or irrelevant API calls are removed by likelihood heuristics rather than learned from environment feedback.

## Evidence Quotes
- claim: The paper frames Toolformer as models teaching themselves to use tools from a few demonstrations.
  supporting URL: https://arxiv.org/abs/2302.04761
  short quote or paraphrase: The paper frames Toolformer as models teaching themselves to use tools from a few demonstrations.
  confidence: high
- claim: The method samples possible API calls, executes them, and filters calls according to whether the result helps predict surrounding text.
  supporting URL: https://arxiv.org/abs/2302.04761
  short quote or paraphrase: The method samples possible API calls, executes them, and filters calls according to whether the result helps predict surrounding text.
  confidence: high
- claim: The work is directly tool-use training, but it is not reinforcement learning over an interactive agent policy.
  supporting URL: https://arxiv.org/abs/2302.04761
  short quote or paraphrase: The work is directly tool-use training, but it is not reinforcement learning over an interactive agent policy.
  confidence: high

## Relevance to Our Paper
- useful for Part 1: yes - helps classify training/evaluation paradigm and task setting
- useful for Part 2: yes - likelihood_filter / data_construction
- useful for cascade drift: limited
- useful for silent errors: yes - useful for semantic/execution mismatch
- useful for gaps / anti-patterns: yes - captures reward/metric limitations or coverage caveats
