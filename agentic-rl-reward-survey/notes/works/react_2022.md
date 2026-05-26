# ReAct: Synergizing Reasoning and Acting in Language Models

## Metadata
- Authors: Yao et al.
- Year: 2022
- Source type: arxiv
- URL: https://arxiv.org/abs/2210.03629
- Code: none found in source entry
- Confidence: high

## Problem Setting
- Task: reasoning and acting
- Tool/API setting: interleaved reasoning traces and actions in knowledge and decision-making environments
- Single-turn or multi-turn: multi-step reasoning/action trajectories in QA and decision environments
- Long-horizon? yes

## Training / Optimization Paradigm
- PPO / GRPO / RLOO / REINFORCE / DPO / RLAIF / SFT / inference-time / benchmark-only / other: prompting / inference-time acting
- Is RL actually used, or only evaluation / prompting? No RL training in the core method; environment feedback is used during trajectories and evaluation.

## Reward Signal
- Outcome reward: Task success or benchmark score after interleaved reasoning and actions.
- Process reward: Reasoning traces structure the process but are not optimized by a learned reward model.
- Execution reward: Observations from external environments/tools affect the next action at inference time.
- Format/schema reward: not specified beyond prompt format
- LLM-as-judge: none in core method
- Preference/contrastive signal: none
- KL/regularization: not specified
- Step penalty/cost: not specified

## Reward Formula or Pseudocode
not specified

## Failure Modes Reported
- Cascade drift / error propagation: Wrong observations or early bad actions can propagate through later reasoning.
- Silent error / semantic mismatch: Reasoning text can be plausible even when the action path is wrong.
- Reward hacking: unknown
- Format overfitting: Prompted action format can break in harder environments.
- Judge hacking: unknown
- Exploration collapse: unknown
- Other:
- Wrong observations or early bad actions can propagate through later reasoning.
- Reasoning text can be plausible even when the action path is wrong.
- Prompted action format can break in harder environments.

## Evidence Quotes
- claim: ReAct interleaves reasoning traces with actions and observations.
  supporting URL: https://arxiv.org/abs/2210.03629
  short quote or paraphrase: ReAct interleaves reasoning traces with actions and observations.
  confidence: high
- claim: The source is a representative inference-time agent baseline rather than an RL training paper.
  supporting URL: https://arxiv.org/abs/2210.03629
  short quote or paraphrase: The source is a representative inference-time agent baseline rather than an RL training paper.
  confidence: high
- claim: Its action-observation framing is central for later tool-use reward design.
  supporting URL: https://arxiv.org/abs/2210.03629
  short quote or paraphrase: Its action-observation framing is central for later tool-use reward design.
  confidence: high

## Relevance to Our Paper
- useful for Part 1: yes - helps classify training/evaluation paradigm and task setting
- useful for Part 2: yes - environment_feedback / evaluation_metric
- useful for cascade drift: yes - long-horizon or multi-step setting
- useful for silent errors: limited
- useful for gaps / anti-patterns: yes - captures reward/metric limitations or coverage caveats
