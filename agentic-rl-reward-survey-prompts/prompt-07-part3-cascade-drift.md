# Prompt 7：深挖第 3 部分 cascade drift

```text
现在只写 drafts/part3_cascade_drift.md。这是重点部分，要比第 1 部分更深。

请先补充检索，但只围绕以下主题：
- error propagation LLM agents
- compounding error tool use
- trajectory drift LLM agent
- long-horizon credit assignment LLM agents
- step-level verifier agents
- process reward model long-horizon agent
- self-reflection correction LLM agents
- backtracking tool-use agents

如果找到新 source，追加 sources/sources.jsonl，并创建 notes/works/{id}.md 或 notes/topics/cascade_drift_sources.md。

正文结构必须如下：

# 第 3 部分：级联漂移

## 3.1 定义对齐
列出社区中的近义说法：
- error propagation
- compounding error
- trajectory drift
- state drift
- long-horizon credit assignment failure
- distributional shift across tool calls
- observation contamination
- irreversible action error

每个术语给 1-2 句解释，并说明它和本论文“级联漂移”的关系。

## 3.2 典型发生机制
用表格：
机制 | 例子 | 为什么 final reward 难发现 | 可观测信号 | 可惩罚点

至少覆盖：
- early wrong API call contaminates later context
- wrong intermediate state but plausible final answer
- stale memory / stale observation
- tool result misread
- subgoal decomposition wrong
- irreversible external action
- retry loop amplifies error
- search/query drift

## 3.3 检测方法
覆盖：
- trace comparison
- step-level verifier
- process reward model
- state consistency check
- cross-trajectory self-consistency
- rollback / replay
- oracle intermediate state
- invariant checking

## 3.4 奖励设计
每类给公式或伪代码：
- dense step reward
- discounted trajectory reward
- explicit intermediate error penalty
- backtracking/reflection reward
- subgoal decomposition reward
- state-consistency reward
- uncertainty-aware penalty

## 3.5 代表工作如何处理
表格：
工作 | 场景 | drift 处理方式 | reward/verification 机制 | 局限

## 3.6 未解决问题
要求犀利具体：
- 哪些 drift 现在几乎没人系统处理？
- 哪些方法只是在 benchmark 上绕开问题？
- 哪些 reward 会掩盖 drift？
- 哪些 drift 需要环境级 oracle，LLM judge 不够？

规则：
- 没有证据就写“此处覆盖不足”。
- 不要把普通 hallucination 全部叫 cascade drift。
- 要服务于 API/tool-use reward design。
```

