# ToolLLM: Facilitating Large Language Models to Master 16000+ Real-world APIs

## Metadata
- Authors: Qin et al.
- Year: 2023
- Source type: arxiv
- URL: https://arxiv.org/abs/2307.16789
- Code: https://github.com/OpenBMB/ToolBench
- Confidence: high

## Problem Setting
- Task: API use
- Tool/API setting: real-world REST APIs from RapidAPI via ToolBench
- Single-turn or multi-turn: single- and multi-step real-world API-use instructions
- Long-horizon? yes

## Training / Optimization Paradigm
- PPO / GRPO / RLOO / REINFORCE / DPO / RLAIF / SFT / inference-time / benchmark-only / other: SFT / tool-learning data construction / benchmark evaluation
- Is RL actually used, or only evaluation / prompting? No core RL reward; ToolEval is an automatic evaluation signal.

## Reward Signal
- Outcome reward: ToolEval pass rate and win rate over solution paths.
- Process reward: No trained PRM; DFS-style tool-use traces and evaluator comparisons provide trajectory assessment.
- Execution reward: Real API/tool responses are part of the task setup.
- Format/schema reward: API-call validity matters but is not separated as a reward in the core taxonomy.
- LLM-as-judge: ChatGPT-backed ToolEval is used for pass/win evaluation.
- Preference/contrastive signal: Win rate compares solution paths.
- KL/regularization: not specified
- Step penalty/cost: limited budget rather than explicit cost reward

## Reward Formula or Pseudocode
ToolEval pass rate = proportion of instructions completed within budget; win rate = pairwise evaluator preference over solution paths.

## Failure Modes Reported
- Cascade drift / error propagation: Pass rate hides where in a trajectory the tool-use failure occurred.
- Silent error / semantic mismatch: unknown
- Reward hacking: unknown
- Format overfitting: unknown
- Judge hacking: LLM evaluator can disagree with humans or miss subtle API misuse.
- Exploration collapse: unknown
- Other:
- LLM evaluator can disagree with humans or miss subtle API misuse.
- Temporal API variability complicates reproducible execution evaluation.
- Pass rate hides where in a trajectory the tool-use failure occurred.

## Evidence Quotes
- claim: ToolLLM introduces ToolBench and ToolEval for real-world API-use evaluation.
  supporting URL: https://arxiv.org/abs/2307.16789
  short quote or paraphrase: ToolLLM introduces ToolBench and ToolEval for real-world API-use evaluation.
  confidence: high
- claim: ToolEval reports pass rate and win rate and is validated against human annotation.
  supporting URL: https://arxiv.org/abs/2307.16789
  short quote or paraphrase: ToolEval reports pass rate and win rate and is validated against human annotation.
  confidence: high
- claim: The paper fine-tunes ToolLLaMA with tool-use data rather than RL.
  supporting URL: https://arxiv.org/abs/2307.16789
  short quote or paraphrase: The paper fine-tunes ToolLLaMA with tool-use data rather than RL.
  confidence: high

## Relevance to Our Paper
- useful for Part 1: yes - helps classify training/evaluation paradigm and task setting
- useful for Part 2: yes - LLM-as-judge / pass_rate
- useful for cascade drift: yes - long-horizon or multi-step setting
- useful for silent errors: yes - useful for semantic/execution mismatch
- useful for gaps / anti-patterns: yes - captures reward/metric limitations or coverage caveats
