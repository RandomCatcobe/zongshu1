# Inclusion / Exclusion Criteria

## 纳入标准

一项 work 满足以下任一组合即可纳入：

- LLM/agent 明确执行 tool/API/function call，并使用 RL 或类 RL 方法训练、优化或选择行为。
- 提供 tool-use/function calling/API use benchmark，且评价指标或环境反馈对 reward design 有参考价值。
- 提出 process reward、execution feedback、trajectory verifier、step-level verifier、self-reflection 或 search 机制，可服务于长程 agent reward。
- 分析 LLM agents 在 web、software engineering、search、API transaction、customer service、code/math execution 等场景中的 failure modes。
- 相邻领域工作虽非 LLM agent，但提供可迁移 reward idea，例如 reward shaping、HER、robotics long-horizon manipulation、code RL with execution feedback、math PRM、formal verification。

## 排除标准

- 纯 prompt engineering，且没有训练、优化、evaluation metric 或 failure analysis 价值。
- 纯 RLHF / preference modeling for chat，不涉及 tool/API/function calling 或 long-horizon agent 行为。
- 只展示产品能力、没有方法细节、没有 benchmark、没有 reward/metric 信息的 marketing blog。
- 无法找到可信 URL 或来源的条目。
- 与 2021 年以来 LLM/tool-use 生态关系较弱的传统 RL 工作，除非用于相邻领域借鉴。

## 边界处理

- ReAct、Reflexion 等可能不是 RL training，但对 inference-time optimization、self-reflection、error recovery 和 reward design 有启发，可纳入但必须标注 training_paradigm 为 prompting / inference-time / self-reflection。
- ToolBench、APIBench、BFCL、tau-bench、SWE-bench、WebArena 等 benchmark 可纳入，但必须标注 benchmark-only 或 evaluation framework，不能写成训练方法。
- DPO / KTO / SimPO 等 preference optimization 如果没有 tool-use 任务证据，只能作为 adjacent or candidate method，不作为核心 evidence。
- LLM-as-judge 的证据必须区分 evaluation metric、training reward、data filtering signal。

