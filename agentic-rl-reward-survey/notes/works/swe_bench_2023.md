# SWE-bench: Can Language Models Resolve Real-World GitHub Issues?

## Metadata
- Authors: Jimenez et al.
- Year: 2023
- Source type: arxiv
- URL: https://arxiv.org/abs/2310.06770
- Code: https://github.com/SWE-bench/SWE-bench
- Confidence: high

## Problem Setting
- Task: software engineering agent benchmark
- Tool/API setting: real GitHub issues requiring code edits and execution environments
- Single-turn or multi-turn: software engineering issue resolution in real repositories
- Long-horizon? yes

## Training / Optimization Paradigm
- PPO / GRPO / RLOO / REINFORCE / DPO / RLAIF / SFT / inference-time / benchmark-only / other: benchmark-only / optional SFT baseline
- Is RL actually used, or only evaluation / prompting? Benchmark evaluation; not an RL method in the core source.

## Reward Signal
- Outcome reward: Repository tests pass and issue is resolved.
- Process reward: No process reward in the benchmark itself.
- Execution reward: Unit tests and repository execution provide the key signal.
- Format/schema reward: patch must apply and code must run; no separate format reward.
- LLM-as-judge: none required for core test-based evaluation
- Preference/contrastive signal: none
- KL/regularization: not specified
- Step penalty/cost: not specified

## Reward Formula or Pseudocode
Reward/evaluation analogue: pass if generated patch resolves the issue under tests; fail otherwise.

## Failure Modes Reported
- Cascade drift / error propagation: Long code-edit workflows have sparse terminal feedback.
- Silent error / semantic mismatch: Tests can be incomplete, so passing tests may not guarantee semantic correctness.
- Reward hacking: Agents can overfit to visible tests or exploit benchmark artifacts.
- Format overfitting: unknown
- Judge hacking: unknown
- Exploration collapse: Long code-edit workflows have sparse terminal feedback.
- Other:
- Tests can be incomplete, so passing tests may not guarantee semantic correctness.
- Long code-edit workflows have sparse terminal feedback.
- Agents can overfit to visible tests or exploit benchmark artifacts.

## Evidence Quotes
- claim: SWE-bench evaluates whether models resolve real GitHub issues.
  supporting URL: https://arxiv.org/abs/2310.06770
  short quote or paraphrase: SWE-bench evaluates whether models resolve real GitHub issues.
  confidence: high
- claim: The central evaluation signal is execution through repository tests.
  supporting URL: https://arxiv.org/abs/2310.06770
  short quote or paraphrase: The central evaluation signal is execution through repository tests.
  confidence: high
- claim: It is a benchmark-only execution-reward analogue for agentic tasks.
  supporting URL: https://arxiv.org/abs/2310.06770
  short quote or paraphrase: It is a benchmark-only execution-reward analogue for agentic tasks.
  confidence: high

## Relevance to Our Paper
- useful for Part 1: yes - helps classify training/evaluation paradigm and task setting
- useful for Part 2: yes - execution_test_pass
- useful for cascade drift: yes - long-horizon or multi-step setting
- useful for silent errors: yes - useful for semantic/execution mismatch
- useful for gaps / anti-patterns: yes - captures reward/metric limitations or coverage caveats
