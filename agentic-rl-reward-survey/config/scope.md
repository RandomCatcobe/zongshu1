# Scope

## 研究主题

研究 agentic RL 在 API/tool-use/function calling 场景下的 reward design。重点不是一般 RLHF，也不是纯 chatbot alignment，而是 LLM/agent 在调用工具、API、函数、检索、代码执行、网页或软件环境时，如何通过 RL 或类 RL 方法训练、优化、评估或校准行为。

## 时间范围

- 主要范围：2021 年至今。
- 执行检索时以检索当天日期为准。
- 对 2024-2026 年工作单独补检，避免遗漏最新 agentic RL / tool-use RL / process reward 方向。

## 核心对象

- LLM agents 使用 API、tools、function calling、web/browser、OS/software、search、retrieval、code execution、customer-service transaction environment。
- 使用 PPO / GRPO / RLOO / REINFORCE / DPO / RLAIF / preference optimization / process reward model / execution feedback / reward shaping / inference-time search 或 self-reflection 等方式优化 agent 行为的工作。
- 与 reward design 密切相关的 benchmark、evaluation framework、failure analysis、工程报告。

## 重点问题

1. tool-use 场景下 reward / penalty 有哪些类型？
2. format reward、execution reward、semantic reward 如何区分？
3. 长程 tool-use 中 cascade drift 如何发生、检测和惩罚？
4. silent errors 为什么会通过表面 reward？
5. 哪些 reward 反模式会导致 reward hacking、overfitting 或训练崩溃？
6. 哪些相邻领域的 reward idea 可以迁移到 agentic tool-use？

## 非目标

- 不写通用 RLHF 综述。
- 不写仅面向闲聊、摘要、开放问答的 reward model 综述，除非与 tool-use reward design 直接相关。
- 不把 benchmark-only 工作误写成 RL training 工作。
- 不把 syntax correctness、execution success、semantic success 混为一谈。

