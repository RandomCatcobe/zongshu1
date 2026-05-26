# tau-bench: A Benchmark for Tool-Agent-User Interaction in Real-World Domains

## Metadata
- Authors: Yao et al.
- Year: 2024
- Source type: arxiv
- URL: https://arxiv.org/abs/2406.12045
- Code: https://github.com/sierra-research/tau-bench
- Confidence: high

## Problem Setting
- Task: API transaction benchmark
- Tool/API setting: customer-service style domains with API tools and policy guidelines
- Single-turn or multi-turn: multi-turn user-agent API transaction episodes
- Long-horizon? yes

## Training / Optimization Paradigm
- PPO / GRPO / RLOO / REINFORCE / DPO / RLAIF / SFT / inference-time / benchmark-only / other: benchmark-only
- Is RL actually used, or only evaluation / prompting? Evaluation benchmark; no policy RL training in the paper.

## Reward Signal
- Outcome reward: Binary task reward based on final database state/actions and required user-facing outputs; pass^k measures consistency across trials.
- Process reward: No learned process reward.
- Execution reward: Agents call programmatic APIs against realistic databases.
- Format/schema reward: tool-call validity required by environment; not isolated as a reward.
- LLM-as-judge: user is LM-simulated, but reward is rule-based over database/output checks.
- Preference/contrastive signal: none
- KL/regularization: not specified
- Step penalty/cost: max action budget; no explicit cost reward extracted

## Reward Formula or Pseudocode
r = r_action * r_output in {0,1}; pass^k estimates probability that all k independent trials succeed.

## Failure Modes Reported
- Cascade drift / error propagation: unknown
- Silent error / semantic mismatch: Wrong information to the user and wrong domain-policy decisions are observed failure classes.
- Reward hacking: unknown
- Format overfitting: unknown
- Judge hacking: unknown
- Exploration collapse: Binary terminal reward is sparse for multi-turn API transactions.
- Other:
- Wrong information to the user and wrong domain-policy decisions are observed failure classes.
- Pass^1 can overstate reliability because pass^k drops when behavior is inconsistent.
- Binary terminal reward is sparse for multi-turn API transactions.

## Evidence Quotes
- claim: tau-bench evaluates tool-agent-user interactions with simulated users and API tools.
  supporting URL: https://arxiv.org/abs/2406.12045
  short quote or paraphrase: tau-bench evaluates tool-agent-user interactions with simulated users and API tools.
  confidence: high
- claim: The reward is based on final database actions and user-facing outputs.
  supporting URL: https://arxiv.org/abs/2406.12045
  short quote or paraphrase: The reward is based on final database actions and user-facing outputs.
  confidence: high
- claim: The paper introduces pass^k to measure consistency over repeated trials.
  supporting URL: https://arxiv.org/abs/2406.12045
  short quote or paraphrase: The paper introduces pass^k to measure consistency over repeated trials.
  confidence: high

## Relevance to Our Paper
- useful for Part 1: yes - helps classify training/evaluation paradigm and task setting
- useful for Part 2: yes - rule_based_task_success / pass^k
- useful for cascade drift: yes - long-horizon or multi-step setting
- useful for silent errors: yes - useful for semantic/execution mismatch
- useful for gaps / anti-patterns: yes - captures reward/metric limitations or coverage caveats
