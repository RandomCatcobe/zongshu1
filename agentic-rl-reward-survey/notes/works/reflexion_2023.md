# Reflexion: Language Agents with Verbal Reinforcement Learning

## Metadata
- Authors: Shinn et al.
- Year: 2023
- Source type: arxiv
- URL: https://arxiv.org/abs/2303.11366
- Code: https://github.com/noahshinn/reflexion
- Confidence: high

## Problem Setting
- Task: language agents
- Tool/API setting: sequential decision-making, coding, and reasoning tasks
- Single-turn or multi-turn: sequential decision, coding, and reasoning episodes with feedback memory
- Long-horizon? yes

## Training / Optimization Paradigm
- PPO / GRPO / RLOO / REINFORCE / DPO / RLAIF / SFT / inference-time / benchmark-only / other: self-reflection / inference-time verbal reinforcement
- Is RL actually used, or only evaluation / prompting? No gradient RL over model weights in the core method; scalar/verbal feedback is converted into reflection memory.

## Reward Signal
- Outcome reward: Episode-level success/failure feedback from tasks.
- Process reward: Verbal reflections summarize mistakes and guide later attempts.
- Execution reward: Coding and environment tasks can return execution-style feedback.
- Format/schema reward: not specified
- LLM-as-judge: can use evaluator feedback depending on task
- Preference/contrastive signal: none reported
- KL/regularization: not specified
- Step penalty/cost: not specified

## Reward Formula or Pseudocode
not specified

## Failure Modes Reported
- Cascade drift / error propagation: unknown
- Silent error / semantic mismatch: Self-reflection can mask semantic errors when the evaluator is shallow.
- Reward hacking: unknown
- Format overfitting: unknown
- Judge hacking: Reflection memory may preserve misleading explanations if feedback is weak.
- Exploration collapse: unknown
- Other:
- Reflection memory may preserve misleading explanations if feedback is weak.
- Self-reflection can mask semantic errors when the evaluator is shallow.
- Repeated attempts still depend on the same task-level feedback bottleneck.

## Evidence Quotes
- claim: The title uses verbal reinforcement learning, but the mechanism is feedback plus memory at inference time.
  supporting URL: https://arxiv.org/abs/2303.11366
  short quote or paraphrase: The title uses verbal reinforcement learning, but the mechanism is feedback plus memory at inference time.
  confidence: high
- claim: The method uses scalar or verbal feedback to produce reflections for later trials.
  supporting URL: https://arxiv.org/abs/2303.11366
  short quote or paraphrase: The method uses scalar or verbal feedback to produce reflections for later trials.
  confidence: high
- claim: It is useful as a process-feedback reference, not as evidence of policy-gradient RL.
  supporting URL: https://arxiv.org/abs/2303.11366
  short quote or paraphrase: It is useful as a process-feedback reference, not as evidence of policy-gradient RL.
  confidence: high

## Relevance to Our Paper
- useful for Part 1: yes - helps classify training/evaluation paradigm and task setting
- useful for Part 2: yes - verbal_feedback / reflection_memory
- useful for cascade drift: yes - long-horizon or multi-step setting
- useful for silent errors: yes - useful for semantic/execution mismatch
- useful for gaps / anti-patterns: yes - captures reward/metric limitations or coverage caveats
