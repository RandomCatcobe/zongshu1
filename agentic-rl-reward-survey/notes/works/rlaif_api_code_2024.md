# Applying RLAIF for Code Generation with API-usage in Lightweight LLMs

## Metadata
- Authors: Sujan Dutta, Sayantan Mahinder, Raviteja Anantha, Bortik Bandyopadhyay
- Year: 2024
- Source type: arxiv
- URL: https://arxiv.org/abs/2406.20060
- Code: none found in source entry
- Confidence: medium

## Problem Setting
- Task: API-usage code generation
- Tool/API setting: lightweight LLMs generating code that uses APIs
- Single-turn or multi-turn: API-usage code generation for lightweight LLMs
- Long-horizon? no

## Training / Optimization Paradigm
- PPO / GRPO / RLOO / REINFORCE / DPO / RLAIF / SFT / inference-time / benchmark-only / other: RLAIF
- Is RL actually used, or only evaluation / prompting? Yes; a reward model trained from AI feedback is used for RL fine-tuning.

## Reward Signal
- Outcome reward: Executability rate and code-generation metrics such as AST, ROUGE, and CodeBLEU.
- Process reward: No process reward for agent trajectories.
- Execution reward: Executability pipeline evaluates generated API-using code.
- Format/schema reward: Code/API-call syntax affects AST and executability metrics.
- LLM-as-judge: Larger LLM provides AI feedback used to train reward model.
- Preference/contrastive signal: AI feedback scores are used to prepare preference/reward-model data.
- KL/regularization: not specified in current extraction
- Step penalty/cost: not specified

## Reward Formula or Pseudocode
LLM feedback score S(t,o) is used to train a reward model; RL fine-tunes the lightweight model against that reward model. Exact scalar formula is partly extracted but not fully normalized here.

## Failure Modes Reported
- Cascade drift / error propagation: unknown
- Silent error / semantic mismatch: Executability can improve while semantic API appropriateness remains incomplete.
- Reward hacking: unknown
- Format overfitting: unknown
- Judge hacking: AI feedback quality depends on the larger LLM prompt.
- Exploration collapse: unknown
- Other:
- Narrow code-generation setting may not represent broad tool-agent RLAIF.
- AI feedback quality depends on the larger LLM prompt.
- Executability can improve while semantic API appropriateness remains incomplete.

## Evidence Quotes
- claim: The paper proposes RLAIF for code generation that requires API calls.
  supporting URL: https://arxiv.org/abs/2406.20060
  short quote or paraphrase: The paper proposes RLAIF for code generation that requires API calls.
  confidence: medium
- claim: A larger LLM supplies AI feedback used to train a reward model.
  supporting URL: https://arxiv.org/abs/2406.20060
  short quote or paraphrase: A larger LLM supplies AI feedback used to train a reward model.
  confidence: medium
- claim: Evaluation includes executability, AST, ROUGE, and CodeBLEU.
  supporting URL: https://arxiv.org/abs/2406.20060
  short quote or paraphrase: Evaluation includes executability, AST, ROUGE, and CodeBLEU.
  confidence: medium

## Relevance to Our Paper
- useful for Part 1: yes - helps classify training/evaluation paradigm and task setting
- useful for Part 2: yes - AI_feedback_reward_model
- useful for cascade drift: limited
- useful for silent errors: yes - useful for semantic/execution mismatch
- useful for gaps / anti-patterns: yes - captures reward/metric limitations or coverage caveats
