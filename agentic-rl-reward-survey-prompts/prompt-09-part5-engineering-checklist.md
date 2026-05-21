# Prompt 9：工程实践 checklist

```text
现在只写 drafts/part5_engineering_checklist.md。

目标：给 agentic RL for tool-use 的 reward/training 工程 checklist。不要长篇解释，用表格和短 bullet。

必须覆盖：

1. KL 系数和 schedule
   - fixed KL
   - adaptive KL
   - KL too high / too low symptoms

2. learning rate、batch size、rollout 数
   - long trajectory 下的 batch variance
   - number of rollouts per prompt
   - group size for GRPO

3. reward normalization / clipping / whitening
   - per-batch
   - per-group
   - per-task
   - pitfalls

4. GRPO / RLOO advantage normalization
   - group size 太小
   - all rewards identical
   - reward scale mismatch
   - task difficulty imbalance

5. 长 trajectory credit assignment
   - intermediate verifier
   - subgoal reward
   - discounting
   - replay trace
   - partial oracle

6. reward hacking 诊断
   - format reward dominates
   - tool overuse
   - no-op loops
   - judge pleasing
   - early stopping hack
   - hidden state corruption

7. 训练崩溃
   - entropy collapse
   - mode collapse
   - KL explosion
   - reward plateau
   - rollout invalid rate rising
   - degenerate tool-call template

8. offline vs online mismatch
   - static benchmark overfitting
   - real API non-determinism
   - environment version drift
   - latency/cost constraints
   - human satisfaction mismatch

格式：
# 第 5 部分：调参技巧与工程实践

## 5.1 Checklist
表格：
问题 | 症状 | 常见原因 | 排查方法 | 修复手段

## 5.2 Minimal training sanity checks
列出训练前必须跑的 sanity checks。

## 5.3 Reward dashboard
列出应该记录的 metrics：
- final success
- schema pass
- semantic pass
- tool error rate
- invalid call rate
- avg steps
- redundant calls
- rollback count
- judge disagreement
- KL
- entropy
- reward variance
- per-task reward distribution

不要新增无证据的论文 claim。这里主要是工程经验综合。
```

