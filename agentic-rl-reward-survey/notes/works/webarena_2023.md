# WebArena: A Realistic Web Environment for Building Autonomous Agents

## Metadata
- Authors: Zhou et al.
- Year: 2023
- Source type: arxiv
- URL: https://arxiv.org/abs/2307.13854
- Code: https://github.com/web-arena-x/webarena
- Confidence: high

## Problem Setting
- Task: web agent benchmark
- Tool/API setting: realistic websites plus tools and external knowledge bases
- Single-turn or multi-turn: web browsing tasks on realistic websites
- Long-horizon? yes

## Training / Optimization Paradigm
- PPO / GRPO / RLOO / REINFORCE / DPO / RLAIF / SFT / inference-time / benchmark-only / other: benchmark-only
- Is RL actually used, or only evaluation / prompting? Only evaluates agents; no training reward is introduced.

## Reward Signal
- Outcome reward: End-to-end functional task success on realistic web tasks.
- Process reward: No process reward model.
- Execution reward: Realistic web environment state changes and observations are central.
- Format/schema reward: browser action validity is required but not isolated as reward.
- LLM-as-judge: some tasks rely on programmed/evaluator checks; details not fully extracted here
- Preference/contrastive signal: none
- KL/regularization: not specified
- Step penalty/cost: not specified

## Reward Formula or Pseudocode
not specified as a generic formula; tasks are judged by functional success criteria.

## Failure Modes Reported
- Cascade drift / error propagation: Long web trajectories are vulnerable to early wrong clicks and state drift.
- Silent error / semantic mismatch: Surface navigation may look plausible while failing the user goal.
- Reward hacking: unknown
- Format overfitting: unknown
- Judge hacking: unknown
- Exploration collapse: Functional success is sparse and gives little credit assignment.
- Other:
- Surface navigation may look plausible while failing the user goal.
- Long web trajectories are vulnerable to early wrong clicks and state drift.
- Functional success is sparse and gives little credit assignment.

## Evidence Quotes
- claim: WebArena is a realistic web environment for autonomous agents.
  supporting URL: https://arxiv.org/abs/2307.13854
  short quote or paraphrase: WebArena is a realistic web environment for autonomous agents.
  confidence: high
- claim: The benchmark emphasizes end-to-end task completion in web settings.
  supporting URL: https://arxiv.org/abs/2307.13854
  short quote or paraphrase: The benchmark emphasizes end-to-end task completion in web settings.
  confidence: high
- claim: It is relevant to semantic success beyond local tool-call syntax.
  supporting URL: https://arxiv.org/abs/2307.13854
  short quote or paraphrase: It is relevant to semantic success beyond local tool-call syntax.
  confidence: high

## Relevance to Our Paper
- useful for Part 1: yes - helps classify training/evaluation paradigm and task setting
- useful for Part 2: yes - functional_task_success
- useful for cascade drift: yes - long-horizon or multi-step setting
- useful for silent errors: yes - useful for semantic/execution mismatch
- useful for gaps / anti-patterns: yes - captures reward/metric limitations or coverage caveats
