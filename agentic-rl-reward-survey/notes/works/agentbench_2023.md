# AgentBench: Evaluating LLMs as Agents

## Metadata
- Authors: Liu et al.
- Year: 2023
- Source type: arxiv
- URL: https://arxiv.org/abs/2308.03688
- Code: https://github.com/THUDM/AgentBench
- Confidence: high

## Problem Setting
- Task: multi-environment agent benchmark
- Tool/API setting: OS, database, knowledge graph, digital game, web browsing, and other agent environments
- Single-turn or multi-turn: multi-environment agent benchmark episodes
- Long-horizon? yes

## Training / Optimization Paradigm
- PPO / GRPO / RLOO / REINFORCE / DPO / RLAIF / SFT / inference-time / benchmark-only / other: benchmark-only
- Is RL actually used, or only evaluation / prompting? Only evaluates agents; no RL training reward is proposed.

## Reward Signal
- Outcome reward: Environment-specific success metrics across OS, DB, KG, game, web, and other settings.
- Process reward: No process reward model.
- Execution reward: Agent actions interact with environments and receive observations.
- Format/schema reward: environment-specific action format constraints
- LLM-as-judge: not a central reward mechanism
- Preference/contrastive signal: none
- KL/regularization: not specified
- Step penalty/cost: budgets may exist by environment; no generic step penalty extracted

## Reward Formula or Pseudocode
not specified as a unified reward; each environment supplies its own success metric.

## Failure Modes Reported
- Cascade drift / error propagation: Long-horizon failures may stem from early bad actions and state drift.
- Silent error / semantic mismatch: unknown
- Reward hacking: unknown
- Format overfitting: unknown
- Judge hacking: unknown
- Exploration collapse: unknown
- Other:
- Aggregated benchmark scores can hide environment-specific failure modes.
- Long-horizon failures may stem from early bad actions and state drift.
- Evaluation-only source does not tell how to train reward-seeking policies.

## Evidence Quotes
- claim: AgentBench evaluates LLMs as agents across multiple interactive environments.
  supporting URL: https://arxiv.org/abs/2308.03688
  short quote or paraphrase: AgentBench evaluates LLMs as agents across multiple interactive environments.
  confidence: high
- claim: The source is useful for long-horizon evaluation taxonomy.
  supporting URL: https://arxiv.org/abs/2308.03688
  short quote or paraphrase: The source is useful for long-horizon evaluation taxonomy.
  confidence: high
- claim: It is explicitly benchmark-only in the current source table.
  supporting URL: https://arxiv.org/abs/2308.03688
  short quote or paraphrase: It is explicitly benchmark-only in the current source table.
  confidence: high

## Relevance to Our Paper
- useful for Part 1: yes - helps classify training/evaluation paradigm and task setting
- useful for Part 2: yes - environment_success_metric
- useful for cascade drift: yes - long-horizon or multi-step setting
- useful for silent errors: yes - useful for semantic/execution mismatch
- useful for gaps / anti-patterns: yes - captures reward/metric limitations or coverage caveats
