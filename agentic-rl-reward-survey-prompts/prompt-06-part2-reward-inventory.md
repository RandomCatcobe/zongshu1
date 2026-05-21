# Prompt 6：写第 2 部分 reward / penalty 盘点

```text
现在只写 drafts/part2_reward_inventory.md。

请读取：
- matrices/reward_taxonomy.csv
- notes/works/*.md
- sources/source_conflicts.md

目标：写“第 2 部分：奖励函数与惩罚函数盘点”。

每一类必须包含：
- 奖励/惩罚形式：LaTeX 或伪代码
- 代表工作
- 适用场景
- 已知失败模式
- 证据链接

必须覆盖以下 reward / penalty：

1. final correctness
   - exact match
   - unit test pass
   - execution success

2. format / schema compliance
   - JSON valid
   - argument type correct
   - required fields present

3. step-level process reward
   - step correctness
   - progress toward subgoal
   - verifier score

4. tool efficiency
   - step penalty
   - redundant call penalty
   - token/API cost

5. exploration / diversity
   - entropy bonus
   - novelty
   - diverse rollout

6. safety / harmful-call refusal
   - unsafe tool penalty
   - refusal reward
   - policy constraint

7. length / early stopping
   - overlong penalty
   - premature stop penalty

8. KL constraint
   - KL to base/reference policy
   - adaptive KL

9. advantage reshaping
   - GRPO group normalization
   - RLOO / leave-one-out
   - reward whitening/clipping

10. preference / contrastive
   - DPO
   - KTO
   - SimPO
   - pairwise preference for trajectories/tool calls

建议表格列：
类别 | 公式/伪代码 | 正奖励 | 负奖励 | 粒度 | 代表工作 | 适用场景 | 失败模式

公式可以简短，例如：
R = 1[answer == gold]
R = 1[unit_tests_pass] - λ * num_steps
R_t = verifier(s_t, a_t, o_t)
R = R_outcome + α R_schema + β R_process - γ cost

要求：
- 明确区分 format reward 和 semantic reward。
- 明确指出纯 format reward 的危险。
- 明确指出长程任务中 pure outcome reward 的稀疏性。
- 不确定的代表工作不要硬塞。
- 不写第 3、4、6 部分。
```

