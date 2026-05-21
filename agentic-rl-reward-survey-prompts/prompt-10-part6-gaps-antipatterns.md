# Prompt 10：第 6 部分 gaps / anti-patterns，要求犀利

```text
现在只写 drafts/part6_gaps_antipatterns.md。这是论文选题最重要部分。请写得具体、可证伪、不要泛泛而谈。

请读取：
- drafts/part2_reward_inventory.md
- drafts/part3_cascade_drift.md
- drafts/part4_silent_errors.md
- notes/works/*.md
- sources/source_conflicts.md
- audits/coverage_audit.md 如果存在

必要时补充检索以下关键词：
- LLM agent reward hacking
- RLHF reward hacking tool use
- LLM-as-judge reward hacking
- tool-use benchmark failure modes
- SWE-bench agent failure analysis
- WebArena failure analysis
- tau-bench failure analysis
- function calling benchmark limitations
- process reward model limitations agents

正文结构：

# 第 6 部分：Gap / 反模式 / 大家踩过的坑

## 6.1 常见 reward 反模式
表格：
反模式 | 表面吸引力 | 实际问题 | 典型症状 | 证据/代表工作 | 替代设计

至少覆盖：
- pure outcome reward in long-horizon tasks
- format reward dominating semantic reward
- step penalty too strong
- LLM judge reward without calibration
- execution success mistaken as task success
- static benchmark overfitting
- hidden oracle leakage
- rewarding tool use frequency instead of tool utility
- ignoring irreversible actions
- treating all tool errors equally
- sparse binary reward with high-variance rollouts

## 6.2 已知 reward hacking / failure cases
每个案例要包括：
- work/source
- setting
- what the agent exploited
- why reward allowed it
- lesson for reward design

没有足够证据的案例放到“待验证”，不要硬写成事实。

## 6.3 公开 limitation / repo issue / blog 里的失败经验
区分：
- paper limitation
- benchmark limitation
- implementation issue
- anecdotal engineering report

## 6.4 结构性空白
列出几乎没人系统做、或者做得很差的方向。每条必须包括：
- gap
- why important
- why hard
- possible experiment
- possible reward design

重点关注：
- cascade drift with external mutable state
- silent semantic errors after schema-valid function calls
- reward for rollback / recovery
- reward for calibrated abstention
- process reward without dense human labels
- multi-tool consistency reward
- cross-episode memory corruption
- API side-effect safety
- task-level business success vs local tool success
- dynamic environment reward

## 6.5 反共识观点
列出哪些“常识”可能被新工作挑战，例如：
- more tool calls are not always better
- execution success is not semantic success
- dense reward can hurt exploration if verifier is wrong
- LLM-as-judge helps coverage but can introduce judge-specific hacking
- process reward is not automatically causal credit assignment

要求：
- 犀利但不能编造。
- 每个强 claim 尽量有来源；没有来源就标 hypothesis。
- 输出应该能直接帮助设计新论文。
```

