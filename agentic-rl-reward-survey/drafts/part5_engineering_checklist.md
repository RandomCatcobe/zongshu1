# 第 5 部分：调参技巧与工程实践

本节不是新增论文 claim，而是把前面 reward taxonomy、cascade drift 和 silent errors 转成训练与评估时可执行的 checklist。核心原则：所有 dashboard 都要把 `schema pass`、`execution pass`、`semantic pass`、`task success` 分开记。

## 5.1 Checklist

| 问题 | 症状 | 常见原因 | 排查方法 | 修复手段 |
|---|---|---|---|---|
| KL 系数过低 | reward 快涨但真实成功率不涨；工具调用模板变怪 | policy 离 reference 太远，开始钻 reward 空子 | 画 KL、semantic pass、invalid call rate 的同图 | 提高 KL；使用 adaptive KL；降低 LR；加语义 gate |
| KL 系数过高 | reward 和成功率都不动；探索不足 | 更新被 reference policy 压住 | 看 token-level KL、action diversity、工具选择熵 | 降低 KL；warmup 后再加约束；放宽早期探索 |
| fixed KL 不适配训练阶段 | 前期学不动，后期又漂移 | 同一 KL 不能同时服务探索和收敛 | 分阶段比较 fixed vs adaptive KL | 用 target-KL schedule；按 invalid rate / semantic pass 调整 |
| learning rate 过高 | KL explosion、entropy collapse、invalid call rate 上升 | 更新步过大；reward 噪声被放大 | 小批量 overfit、梯度范数、KL per update | 降 LR；加 gradient clipping；增加 rollout batch |
| learning rate 过低 | reward plateau，工具策略几乎不变 | 更新太弱或 KL 太强 | 看 advantage 非零但 policy 不动 | 提 LR；减 KL；检查 reward scale |
| batch size / rollout 数不足 | 同一 prompt 结果方差大；GRPO advantage 抖动 | 长 trajectory 下回报方差高 | 统计 per-task reward variance 和 pass^k | 增加 rollouts per prompt；按任务难度分桶采样 |
| GRPO group size 太小 | 组内全赢或全输，优势估计无信息 | group 不能形成可比样本 | 记录每组 reward 唯一值数量 | 增大 group size；混合难度；加入 step verifier |
| GRPO group size 太大 | 成本暴涨，更新慢 | 采样预算被单个 prompt 吃掉 | 比较 marginal gain vs cost | 自适应 group size；只对高不确定样本扩展 |
| reward normalization 错位 | 简单任务支配训练；困难任务被压扁 | per-batch normalization 混合了不同任务 | 按 task/domain 画 reward 分布 | per-task 或 per-group normalization；保留 raw reward dashboard |
| clipping / whitening 过强 | 关键失败和小失败看起来一样 | 极端错误被裁掉 | 比较 raw reward、clipped reward、advantage | 只裁异常值；关键安全/语义错误单独惩罚 |
| all rewards identical | advantage 全 0；训练无更新 | verifier 太粗，任务太易或太难 | 每组 reward histogram | 加 harder negatives；改用 dense step reward；调采样难度 |
| reward scale mismatch | format reward 主导 semantic reward | 各分量量纲不一致 | 画 reward component contribution | 归一化分量；限制 surface reward 上限；语义失败 cap 总分 |
| task difficulty imbalance | 模型只学会简单工具或模板 | 采样分布偏简单任务 | per-task success、per-task KL、per-task loss | curriculum；stratified sampling；困难任务单独队列 |
| 长 trajectory credit assignment | 最终 0/1 reward 多，模型不知道错在哪 | outcome reward 稀疏 | replay 失败轨迹，定位 first bad action | intermediate verifier；subgoal reward；discounted return；partial oracle |
| process reward 太吵 | PRM 分数升，task success 降 | PRM / LLM judge 被 policy shift 攻破 | PRM score vs true success scatter plot | holdout judge set；judge ensemble；KL 收紧；人工抽检 |
| format reward dominates | JSON 更稳定但任务不更对 | schema pass 过度加权 | 比较 schema pass 和 semantic pass gap | 降低 format 权重；语义失败时 cap；加 plausible-wrong negatives |
| tool overuse | 平均步数和 API 成本升高，成功率不升 | 工具调用被过度奖励 | avg steps、redundant calls、same-query retry | call cost penalty；必要性 classifier；stop reward |
| no-op loops | 重复查询、重复失败、反复解释 | 缺少 novelty / retry 限制 | repeated call rate、same error code count | retry budget；novelty threshold；失败后 ask/abort reward |
| judge pleasing | 回答迎合 rubric，但日志不支持 | judge 看文本多于看工具证据 | judge disagreement、unsupported-claim rate | 强制 judge 读取 raw tool log；claim-to-evidence check |
| early stopping hack | 输出变短，工具少用，但任务未完成 | 长度/成本惩罚太强 | premature stop rate、missing required action | stop-when-done verifier；必要步骤 checklist |
| hidden state corruption | 后续动作基于错误 belief / memory | 工具结果、缓存或上下文污染 | state snapshot vs agent belief diff | state consistency reward；freshness check；verified commit |
| entropy collapse / mode collapse | 工具选择单一，失败模式重复 | RL 过早收敛 | tool distribution entropy、unique trajectories | entropy bonus；多样 rollout；重采样失败任务 |
| KL explosion | 语言质量和工具格式同时退化 | reward spike、LR 过高、KL 太低 | KL per update 和 bad trace review | rollback checkpoint；降 LR；提高 KL；清洗异常 reward |
| reward plateau | 所有指标停住 | oracle 太稀疏、任务太难或 reward 饱和 | 成功/失败 trace 分桶 | 加 intermediate verifier；换 curriculum；补 hard negatives |
| rollout invalid rate rising | 无效工具、坏参数、解析失败增加 | parser 宽松或 format reward 不够稳定 | invalid call taxonomy | strict parser；schema unit tests；格式 SFT refresh |
| degenerate tool-call template | 固定工具名/固定参数反复出现 | 模型学到模板捷径 | top tool-call n-gram、argument diversity | 模板去重；增加工具负例；调高语义 reward |
| static benchmark overfitting | benchmark 分数涨，线上不涨 | 离线数据太固定 | held-out tasks、paraphrase eval、versioned env | benchmark rotation；hidden tests；online canary |
| real API non-determinism | 同一轨迹重跑结果不同 | API 版本、时间、库存、速率限制变化 | replay with pinned snapshots vs live | snapshot env；mock/live 双评估；记录 version/time |
| environment version drift | 过去能过的策略突然失败 | 工具 schema、网页、policy 改了 | schema diff、contract diff、failure onset time | version pinning；contract tests；变更后再训练 |
| latency/cost constraint mismatch | 训练成功率高，线上太慢太贵 | 训练 reward 不含预算 | p95 latency、API cost、token cost | cost-aware penalty；budgeted rollout；缓存但强制 freshness |
| human satisfaction mismatch | 指标过关但用户不满意 | oracle 漏掉语气、澄清、确认、解释 | human audit、complaint tags、task reopen rate | 人工偏好补充；高风险动作确认；domain rubric |

## 5.2 Minimal training sanity checks

- **Parser and schema smoke test**：用合法、缺字段、错类型、额外字段、近似工具名样例测试 dispatcher，禁止宽松吞错。
- **Reward component unit tests**：手写 20 条轨迹，分别覆盖 format pass / execution fail、execution pass / semantic fail、semantic pass、unsafe write。
- **Golden trace replay**：在 mock 环境重放成功和失败轨迹，确认 reward 与预期一致。
- **Plausible-wrong negative set**：加入错实体、错时间、错 policy、错聚合、stale result、tool bypass，不只加坏 JSON。
- **Tiny-batch overfit**：先让模型在极小任务集上学到目标，再扩大任务；如果 tiny set 都学不会，先查 reward。
- **Group reward variance check**：GRPO/RLOO 前确认每个 prompt group 内存在可比较回报，不是全 0 或全 1。
- **KL / entropy smoke run**：短跑几百步，确认 KL、entropy、invalid call rate 没有早期爆炸。
- **Judge calibration set**：如果用 LLM-as-judge，固定一组人工标注轨迹，持续测 judge accuracy 和 disagreement。
- **Hidden-test dry run**：用不进入训练 reward 的任务或隐藏测试评估 surface overfitting。
- **Irreversible-action gate**：任何写入、删除、付款、发信、权限变更类动作，都要有 precondition、confirmation 和 postcondition 测试。
- **Offline-to-online canary**：上线前小流量对照，记录真实 API drift、延迟、成本和人工失败标签。

## 5.3 Reward dashboard

| Metric | 为什么记录 | 建议切分 |
|---|---|---|
| final success | 最终任务是否完成 | task、domain、trajectory length |
| schema pass | 格式层是否稳定 | tool、model version、prompt template |
| semantic pass | 是否满足用户意图和领域约束 | task、oracle type、risk level |
| tool error rate | 外部工具运行失败率 | tool、error code、time |
| invalid call rate | 无效函数名、坏参数、解析失败 | error subtype、tool family |
| avg steps | 是否过度调用或过早停止 | success vs failure、task type |
| redundant calls | 重复 query、重复 API、no-op loop | trace prefix、tool |
| rollback count | 发现错误并回退的频率 | action type、risk level |
| judge disagreement | LLM judge / PRM 是否稳定 | judge model、rubric version |
| KL | policy drift 是否受控 | update step、task bucket |
| entropy | 是否 mode collapse | tool selection、token/action |
| reward variance | 采样是否有可学信号 | prompt group、task difficulty |
| per-task reward distribution | 简单任务是否支配训练 | task、domain、source split |
| format/execution/semantic gap | surface reward 是否掩盖语义失败 | tool、benchmark、model checkpoint |
| false-success rate | 执行成功但语义失败比例 | tool、API endpoint、business flow |
| unsafe commit rate | 高风险写操作是否缺确认 | action type、policy domain |
| pass^k / repeated-trial success | agent 是否稳定可靠 | k、task、temperature |
| cost and latency | 训练 reward 是否能上线 | p50/p95 latency、API cost、tokens |

最小 dashboard 不应只报一个总 reward。至少同时看 `final success`、`schema pass`、`semantic pass`、`invalid call rate`、`avg steps`、`KL`、`entropy` 和 `reward variance`。一旦 schema pass 上升但 semantic pass 不动，就要先怀疑 reward proxy 被学穿了。
