# Agentic Reasoning and Tool Integration for LLMs via Reinforcement Learning

## Metadata
- Authors: Joykirat Singh, Raghav Magazine, Yash Pandya, Akshay Nambi
- Year: 2025
- Source type: arxiv
- URL: https://arxiv.org/abs/2505.01441
- Code: none found in source entry
- Confidence: high

## Problem Setting
- Task: agentic reasoning and tool integration
- Tool/API setting: math reasoning and multi-turn function calling with dynamic tool selection
- Single-turn or multi-turn: math reasoning and multi-turn function calling with dynamic tool selection
- Long-horizon? yes

## Training / Optimization Paradigm
- PPO / GRPO / RLOO / REINFORCE / DPO / RLAIF / SFT / inference-time / benchmark-only / other: RL / agentic reasoning with tool integration
- Is RL actually used, or only evaluation / prompting? Yes; the paper describes outcome-based RL for tool/environment interaction.

## Reward Signal
- Outcome reward: Outcome-based reward over final solution/task success.
- Process reward: No step-level supervision required in the core claim.
- Execution reward: Tools and environments are invoked inside reasoning chains.
- Format/schema reward: function-calling format matters in benchmarks; specific reward not extracted.
- LLM-as-judge: not central in current extraction
- Preference/contrastive signal: none
- KL/regularization: not specified
- Step penalty/cost: not specified

## Reward Formula or Pseudocode
r = outcome_success(task) for trajectories with interleaved reasoning, tool queries, and tool outputs; detailed formula not specified in extracted text.

## Failure Modes Reported
- Cascade drift / error propagation: unknown
- Silent error / semantic mismatch: Tool failures or hallucinated reasoning can be hidden until final evaluation.
- Reward hacking: potential proxy-reward weakness: function-calling benchmarks may reward surface call correctness more than robust recovery.
- Format overfitting: unknown
- Judge hacking: unknown
- Exploration collapse: unknown
- Other:
- Outcome-only RL may leave tool-use credit assignment unresolved.
- Tool failures or hallucinated reasoning can be hidden until final evaluation.
- Function-calling benchmarks may reward surface call correctness more than robust recovery.

## Evidence Quotes
- claim: ARTIST couples agentic reasoning, reinforcement learning, and tool integration.
  supporting URL: https://arxiv.org/abs/2505.01441
  short quote or paraphrase: ARTIST couples agentic reasoning, reinforcement learning, and tool integration.
  confidence: high
- claim: It lets models decide when, how, and which tools to invoke within multi-turn reasoning chains.
  supporting URL: https://arxiv.org/abs/2505.01441
  short quote or paraphrase: It lets models decide when, how, and which tools to invoke within multi-turn reasoning chains.
  confidence: high
- claim: The source explicitly says it uses outcome-based RL without requiring step-level supervision.
  supporting URL: https://arxiv.org/abs/2505.01441
  short quote or paraphrase: The source explicitly says it uses outcome-based RL without requiring step-level supervision.
  confidence: high

## Relevance to Our Paper
- useful for Part 1: yes - helps classify training/evaluation paradigm and task setting
- useful for Part 2: yes - outcome_based_agentic_RL
- useful for cascade drift: yes - long-horizon or multi-step setting
- useful for silent errors: yes - useful for semantic/execution mismatch
- useful for gaps / anti-patterns: yes - captures reward/metric limitations or coverage caveats
