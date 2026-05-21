# Prompt 4：逐篇抽取 reward 信息

```text
现在不要搜新论文，先基于现有 sources/sources.jsonl 做结构化抽取。

请为每个 high/medium confidence 的 source 创建 notes/works/{id}.md。

每个 work note 必须包含以下小节：

# {title}

## Metadata
- Authors:
- Year:
- Source type:
- URL:
- Code:
- Confidence:

## Problem Setting
- Task:
- Tool/API setting:
- Single-turn or multi-turn:
- Long-horizon? yes/no:

## Training / Optimization Paradigm
- PPO / GRPO / RLOO / REINFORCE / DPO / RLAIF / SFT / inference-time / benchmark-only / other:
- Is RL actually used, or only evaluation / prompting?

## Reward Signal
- Outcome reward:
- Process reward:
- Execution reward:
- Format/schema reward:
- LLM-as-judge:
- Preference/contrastive signal:
- KL/regularization:
- Step penalty/cost:

## Reward Formula or Pseudocode
用简短公式或伪代码。如果论文没有明确公式，写“not specified”。

## Failure Modes Reported
- Cascade drift / error propagation:
- Silent error / semantic mismatch:
- Reward hacking:
- Format overfitting:
- Judge hacking:
- Exploration collapse:
- Other:

## Evidence Quotes
列出 2-5 条证据，每条包括：
- claim
- supporting URL
- short quote or paraphrase
- confidence

## Relevance to Our Paper
- useful for Part 1:
- useful for Part 2:
- useful for cascade drift:
- useful for silent errors:
- useful for gaps / anti-patterns:

同时更新 matrices/reward_taxonomy.csv，列包括：
work_id,reward_type,formula_or_pseudocode,positive_reward,negative_reward,source_of_signal,granularity,applies_to,failure_modes,confidence

规则：
- 没有证据的字段写 unknown，不要猜。
- 不要把 benchmark-only 工作误判成 RL training 工作。
- 完成后只报告完成了多少 work notes，哪些 source 信息不足。
```

