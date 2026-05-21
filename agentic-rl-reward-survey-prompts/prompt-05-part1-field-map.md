# Prompt 5：写第 1 部分领域地图

```text
现在只写 drafts/part1_field_map.md，不写其他部分。

请读取：
- sources/sources.jsonl
- matrices/work_taxonomy.csv
- notes/works/*.md

目标：写“第 1 部分：领域地图”。

结构必须如下：

# 第 1 部分：领域地图

## 1.1 按训练范式分类
表格列：
训练范式 | 代表工作 | 核心做法 | reward/optimization 信号 | 适用任务 | 局限

覆盖：
- PPO / GRPO / RLOO / REINFORCE 类端到端 RL
- DPO / KTO / SimPO / preference optimization
- process reward model
- reward shaping
- curriculum
- inference-time optimization / self-reflection / search

## 1.2 按奖励信号来源分类
表格列：
奖励信号来源 | 形式 | 代表工作 | 优点 | 失败模式

覆盖：
- outcome-based
- process-based
- execution-based
- LLM-as-judge
- self-consistency
- contrastive / preference

## 1.3 按任务类型分类
表格列：
任务类型 | 代表 benchmark/work | 单轮/多轮 | 是否真实执行 | 主要难点

覆盖：
- single-turn function call
- multi-turn tool use
- web / OS / software engineering agents
- search agents
- code/math execution agents
- API transaction / customer-service agents

## 1.4 代表工作一句话速览
每条格式：
- **WorkName** — 一句话说明它在本领域的位置。引用链接。

要求：
- 每个具体工作都要有链接。
- 不确定的地方写“此处覆盖不足”。
- 不要夸张，不要说“首个/首次”除非证据很强。
- 中文叙述，英文术语保留。
```

