# R1-Searcher: Incentivizing the Search Capability in LLMs via Reinforcement Learning

## Metadata
- Authors: Huatong Song, Jinhao Jiang, Yingqian Min, Jie Chen, Zhipeng Chen, Wayne Xin Zhao, Lei Fang, Ji-Rong Wen
- Year: 2025
- Source type: arxiv
- URL: https://arxiv.org/abs/2503.05592
- Code: none found in source entry
- Confidence: medium

## Problem Setting
- Task: search capability RL
- Tool/API setting: LLM search behavior
- Single-turn or multi-turn: search-capability RL for knowledge-intensive QA
- Long-horizon? yes

## Training / Optimization Paradigm
- PPO / GRPO / RLOO / REINFORCE / DPO / RLAIF / SFT / inference-time / benchmark-only / other: two-stage outcome-based RL
- Is RL actually used, or only evaluation / prompting? Yes; RL trains the model to invoke search and answer with retrieved information.

## Reward Signal
- Outcome reward: Stage-2 answer reward based on F1 with ground truth answer.
- Process reward: No process rewards; Stage-1 separately rewards retrieval invocation.
- Execution reward: External retrieval system is invoked during reasoning.
- Format/schema reward: Stage-1/2 format rewards enforce think/query/answer tags; Stage-2 penalizes bad format.
- LLM-as-judge: LLM-as-judge appears in evaluation, not core training reward.
- Preference/contrastive signal: none
- KL/regularization: RL loss includes KL divergence; details not central in note
- Step penalty/cost: retrieval reward in Stage-1; no general step penalty

## Reward Formula or Pseudocode
Stage-1 final reward = retrieval reward + 0.5 format reward. Stage-2 reward = F1 answer reward + format reward, where bad format receives -2.

## Failure Modes Reported
- Cascade drift / error propagation: unknown
- Silent error / semantic mismatch: unknown
- Reward hacking: Format reward had to be refined to avoid reward hacking and abnormal outputs.
- Format overfitting: Format reward had to be refined to avoid reward hacking and abnormal outputs.
- Judge hacking: unknown
- Exploration collapse: unknown
- Other:
- Format reward had to be refined to avoid reward hacking and abnormal outputs.
- Retrieval reward alone can teach search invocation without answer correctness.
- EM reward can shorten responses and underperform F1 reward.

## Evidence Quotes
- claim: R1-Searcher uses a two-stage outcome-based RL approach.
  supporting URL: https://arxiv.org/abs/2503.05592
  short quote or paraphrase: R1-Searcher uses a two-stage outcome-based RL approach.
  confidence: medium
- claim: Stage 1 rewards retrieval invocation and format, while Stage 2 uses answer reward and format penalty.
  supporting URL: https://arxiv.org/abs/2503.05592
  short quote or paraphrase: Stage 1 rewards retrieval invocation and format, while Stage 2 uses answer reward and format penalty.
  confidence: medium
- claim: The paper discusses format reward refinements to avoid reward hacking.
  supporting URL: https://arxiv.org/abs/2503.05592
  short quote or paraphrase: The paper discusses format reward refinements to avoid reward hacking.
  confidence: medium

## Relevance to Our Paper
- useful for Part 1: yes - helps classify training/evaluation paradigm and task setting
- useful for Part 2: yes - two_stage_retrieval_answer_reward
- useful for cascade drift: yes - long-horizon or multi-step setting
- useful for silent errors: yes - useful for semantic/execution mismatch
- useful for gaps / anti-patterns: yes - captures reward/metric limitations or coverage caveats
