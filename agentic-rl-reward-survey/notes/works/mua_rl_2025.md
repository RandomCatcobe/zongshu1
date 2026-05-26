# MUA-RL: Multi-turn User-interacting Agent Reinforcement Learning for agentic tool use

## Metadata
- Authors: Weikang Zhao, Xili Wang, Chengdi Ma, Lingbin Kong, Zhaohua Yang, Mingxiang Tuo, Xiaowei Shi, Yitao Zhai, Xunliang Cai
- Year: 2025
- Source type: arxiv
- URL: https://arxiv.org/abs/2508.18669
- Code: none found in source entry
- Confidence: high

## Problem Setting
- Task: multi-turn user-interacting tool-use agent
- Tool/API setting: agentic tool use with LLM-simulated users inside the RL loop
- Single-turn or multi-turn: multi-turn user-interacting tool-use episodes with real-time tool execution
- Long-horizon? yes

## Training / Optimization Paradigm
- PPO / GRPO / RLOO / REINFORCE / DPO / RLAIF / SFT / inference-time / benchmark-only / other: GRPO with LLM-simulated users
- Is RL actually used, or only evaluation / prompting? Yes; GRPO is used with simulated users inside the RL loop.

## Reward Signal
- Outcome reward: Binary ultimate task completion reward.
- Process reward: No intermediate tool-format/success rewards; deliberately simplified.
- Execution reward: Real-time tool execution and database interaction are in the rollout.
- Format/schema reward: No separate format reward; authors argue complex format rewards may encourage the wrong behavior.
- LLM-as-judge: not central for reward; simulated user participates in rollout
- Preference/contrastive signal: none
- KL/regularization: GRPO objective includes standard beta term; not detailed in note
- Step penalty/cost: no step penalty; reward solely for complete task resolution

## Reward Formula or Pseudocode
r = 1 iff the agent successfully fulfills the task in accordance with the system prompt; r = 0 otherwise.

## Failure Modes Reported
- Cascade drift / error propagation: unknown
- Silent error / semantic mismatch: unknown
- Reward hacking: Binary reward is sparse but avoids direct exploitation of syntax rewards.
- Format overfitting: unknown
- Judge hacking: unknown
- Exploration collapse: unknown
- Other:
- Binary reward is sparse but avoids direct exploitation of syntax rewards.
- Dynamic users make rollouts stochastic and harder to optimize.
- Dialogue requirements can impede learning correct tool invocation patterns.

## Evidence Quotes
- claim: MUA-RL integrates LLM-simulated users into the reinforcement-learning loop.
  supporting URL: https://arxiv.org/abs/2508.18669
  short quote or paraphrase: MUA-RL integrates LLM-simulated users into the reinforcement-learning loop.
  confidence: high
- claim: The framework provides reward solely based on ultimate task completion.
  supporting URL: https://arxiv.org/abs/2508.18669
  short quote or paraphrase: The framework provides reward solely based on ultimate task completion.
  confidence: high
- claim: The paper sets r=1 for successful task fulfillment and r=0 otherwise.
  supporting URL: https://arxiv.org/abs/2508.18669
  short quote or paraphrase: The paper sets r=1 for successful task fulfillment and r=0 otherwise.
  confidence: high

## Relevance to Our Paper
- useful for Part 1: yes - helps classify training/evaluation paradigm and task setting
- useful for Part 2: yes - binary_task_completion
- useful for cascade drift: yes - long-horizon or multi-step setting
- useful for silent errors: yes - useful for semantic/execution mismatch
- useful for gaps / anti-patterns: yes - captures reward/metric limitations or coverage caveats
