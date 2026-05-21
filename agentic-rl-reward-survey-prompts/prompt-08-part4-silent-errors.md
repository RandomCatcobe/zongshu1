# Prompt 8：深挖第 4 部分 silent errors

```text
现在只写 drafts/part4_silent_errors.md。这也是重点部分。

请先补充检索，但只围绕：
- silent failure LLM agents
- plausible but wrong tool use
- semantic mismatch function calling
- hallucinated tool result
- successful but incorrect API call
- execution grounded verification LLM tools
- tool call semantic correctness
- benchmark function calling semantic evaluation
- BFCL function calling evaluation
- tau-bench tool call correctness
- APIBench tool use hallucination
- ToolBench evaluation error

如果找到新 source，追加 sources/sources.jsonl，并更新 notes/topics/silent_errors_sources.md。

正文结构：

# 第 4 部分：静默错误

## 4.1 现象命名
解释以下说法：
- silent failure
- plausible-but-wrong
- hallucinated tool result
- semantic mismatch
- successful-but-incorrect
- false positive execution success
- schema-valid but semantically invalid
- reward-passing wrong behavior

## 4.2 错误类型 taxonomy
表格：
错误类型 | 表面上为什么像对的 | 实际错在哪里 | format reward 是否能发现 | execution reward 是否能发现 | 需要什么 oracle

至少覆盖：
- JSON valid but wrong argument
- correct API endpoint but wrong entity
- correct retrieval format but irrelevant evidence
- tool returns success but business goal fails
- unit tests pass but hidden requirement fails
- LLM invents tool output
- stale/cached result treated as current
- wrong aggregation over correct tool results
- unsafe action formatted correctly

## 4.3 检测方法
覆盖：
- execution-grounded verification
- cross-tool consistency
- LLM judge with explicit rubric
- reverse verification
- ground-truth oracle
- invariant / contract checking
- differential testing
- hidden tests
- human audit

## 4.4 奖励/惩罚设计
每类给公式或伪代码：
- two-layer reward: format pass != semantic pass
- execution-based semantic reward
- judge-augmented reward
- adversarial negative examples
- contradiction penalty
- false-success penalty
- uncertainty / abstention reward

## 4.5 代表工作
表格：
工作 | silent error 相关场景 | 检测方法 | reward/metric | 局限

## 4.6 空白区
重点指出：
- 哪些 silent errors 当前 benchmark 很少覆盖？
- 哪些错误被 execution success 掩盖？
- 哪些需要 domain expert / business oracle？
- 哪些 LLM-as-judge 容易误判？

规则：
- 必须明确区分 syntax correctness、execution success、semantic success。
- 不要把所有错误都说成 hallucination。
- 这一部分要为新 reward function 设计提供空间。
```

