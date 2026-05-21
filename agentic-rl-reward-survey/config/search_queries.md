# Search Queries

当前文件是正式检索前的 query plan。执行正式检索时，逐组记录命中 source、URL、缺失项和冲突信息。

## 1. Agentic RL for tool use / API use / function calling

| Query string | Purpose | Expected source types | Inclusion criteria | Likely false positives |
|---|---|---|---|---|
| "agentic reinforcement learning" "tool use" LLM | 找 agentic RL + tool-use 总体工作 | arXiv, OpenReview, GitHub, project page | 明确 LLM agent 使用 tools/API 并有 RL 或类 RL 优化 | 泛泛 agentic AI blog |
| "reinforcement learning" "function calling" LLM | 找 function calling 的 RL 训练 | paper, benchmark, blog, GitHub | 包含 function call selection、argument generation 或 call correctness reward | 普通 function calling tutorial |
| "LLM agent" "API use" "reinforcement learning" | 找 API transaction / tool-use agent 的 RL | arXiv, conference paper, report | API/tool 调用是任务核心，而非附带 demo | API rate limit 或 devops 内容 |

## 2. LLM tool-use training with PPO / GRPO / RLOO / REINFORCE / DPO / RLAIF

| Query string | Purpose | Expected source types | Inclusion criteria | Likely false positives |
|---|---|---|---|---|
| "GRPO" "tool use" LLM agent | 找 GRPO tool-use 训练 | arXiv, GitHub, technical report | GRPO 用于 tool-use/function-call/search/agent trajectory | DeepSeek-R1 普通 math RL 讨论 |
| "PPO" "tool use" "large language model" | 找 PPO tool-use | paper, GitHub | PPO reward 与 tool/API 行为相关 | 通用 RLHF PPO |
| "RLOO" "tool use" LLM | 找 leave-one-out / rollout advantage | paper, implementation | RLOO 用于 multi-rollout tool-use 或 agent task | RLOO 教程 |
| "REINFORCE" "function calling" LLM | 找简单 policy gradient 工具调用训练 | paper, repo | 奖励来自工具调用成功、执行反馈或任务成功 | 非 LLM 的 API selection |
| "DPO" "tool use" LLM agent | 找偏好优化 tool-use | paper, dataset, GitHub | preference pair 是 tool call 或 agent trajectory | 普通 chat DPO |
| "RLAIF" "tool use" LLM agent | 找 AI feedback 用于 tool-use 训练 | paper, report | LLM judge / AI feedback 用于 tool-use 行为 | 通用 RLAIF |

## 3. Process reward model for agents / tool use / long-horizon reasoning

| Query string | Purpose | Expected source types | Inclusion criteria | Likely false positives |
|---|---|---|---|---|
| "process reward model" "LLM agent" | 找 agent PRM | arXiv, OpenReview | PRM 评估 agent step、tool call、trajectory 或 subgoal | 纯数学 PRM |
| "step-level verifier" "tool use" LLM | 找 step verifier | paper, GitHub | verifier 对 tool-use/action step 打分 | 普通 chain-of-thought verifier |
| "long-horizon" "process reward" "LLM agent" | 找长程 agent credit assignment | paper, benchmark | 长程轨迹中间状态或步骤 reward | robotics-only 无 LLM |

## 4. Execution-based reward / unit-test reward / API execution feedback

| Query string | Purpose | Expected source types | Inclusion criteria | Likely false positives |
|---|---|---|---|---|
| "execution feedback" "reinforcement learning" LLM "tool use" | 找执行反馈作为 reward | arXiv, GitHub | reward 来自执行结果、unit tests、API response | 纯代码执行无 agent |
| "unit test reward" "LLM" "reinforcement learning" | 找 code RL 可迁移 reward | paper, GitHub | unit test pass/fail 作为 reward，可迁移到 tool execution | 普通 program repair 无 RL |
| "API execution feedback" LLM agent reward | 找 API success/failure reward | report, paper, blog | 明确区分 API execution success 和 task success | API monitoring |

## 5. Error propagation / compounding error / trajectory drift in LLM agents

| Query string | Purpose | Expected source types | Inclusion criteria | Likely false positives |
|---|---|---|---|---|
| "error propagation" "LLM agents" | 找级联错误分析 | paper, benchmark analysis, blog | 错误沿 trajectory 传播或污染后续状态 | 普通 hallucination 综述 |
| "compounding error" "tool use" LLM | 找 tool-use 错误累积 | paper, failure analysis | early tool/API mistake 影响后续 steps | 非 LLM control theory |
| "trajectory drift" "LLM agent" | 找 trajectory/state drift | paper, report | 多步 agent 轨迹偏离目标或状态污染 | embedding drift |
| "long-horizon credit assignment" "LLM agents" | 找长程 credit assignment | paper, RL report | 讨论 long-horizon reward sparsity 或 step credit | 纯 robotics |

## 6. Silent failure / plausible-but-wrong / semantic mismatch in tool use

| Query string | Purpose | Expected source types | Inclusion criteria | Likely false positives |
|---|---|---|---|---|
| "silent failure" "LLM agents" | 找静默失败 | paper, blog, benchmark analysis | 表面成功但任务或语义失败 | system reliability unrelated to LLM |
| "plausible but wrong" "tool use" LLM | 找 plausible wrong tool output/use | paper, report | LLM/tool 返回或解释看似合理但错误 | hallucination 泛文 |
| "semantic mismatch" "function calling" LLM | 找 schema-valid but semantically wrong | paper, benchmark | function call 语义错误、参数实体错误 | programming language type mismatch |
| "successful but incorrect API call" LLM agent | 找 execution success 掩盖 task failure | blog, issue, paper | API 返回 success 但业务目标失败 | generic API QA |

## 7. Benchmarks

| Query string | Purpose | Expected source types | Inclusion criteria | Likely false positives |
|---|---|---|---|---|
| "ToolBench" "ToolLLM" paper | ToolBench / ToolLLM | paper, GitHub, project | tool-use benchmark 或 data construction | third-party tutorials |
| "APIBench" LLM tool use benchmark | APIBench | paper, GitHub | API use benchmark | unrelated API benchmark |
| "AgentBench" LLM agents benchmark | AgentBench | paper, GitHub | multi-environment LLM agent benchmark | generic agent benchmarking |
| "WebArena" autonomous agents benchmark | WebArena | paper, GitHub, project | web agent benchmark | web UX benchmarks |
| "SWE-bench" agent failure analysis | SWE-bench | paper, GitHub, analysis | software engineering agent benchmark | coding assistant marketing |
| "tau-bench" "language agents" | tau-bench | paper, GitHub, project | API transaction/customer-service benchmark | statistics tau benchmark |
| "Berkeley Function Calling Leaderboard" BFCL | BFCL | leaderboard, blog, GitHub | function calling evaluation | school admin pages |
| "ToolACE" LLM tool use | ToolACE | paper, GitHub | tool-use dataset/training/eval | unrelated ACE acronym |
| "APIGen-MT" multi-turn tool use | APIGen-MT | paper, GitHub | multi-turn API/tool-use benchmark or dataset | API generation tooling |

## 8. Representative works

| Query string | Purpose | Expected source types | Inclusion criteria | Likely false positives |
|---|---|---|---|---|
| "ToolFormer" language models use tools | ToolFormer | paper, arXiv | self-supervised tool-use | tool builder posts |
| "ReAct" reasoning acting language models | ReAct | paper, project | reasoning + acting trajectories | React frontend |
| "Reflexion" language agents verbal reinforcement learning | Reflexion | paper, GitHub | self-reflection/retry as optimization | psychology reflexion |
| "ToRA" tool-integrated reasoning agents | ToRA | paper, GitHub | math/code tool-use | unrelated TORA acronym |
| "AgentTuning" LLM agents | AgentTuning | paper, GitHub | agent instruction/data tuning | enterprise tuning blogs |
| "FireAct" LLM agents | FireAct | paper, GitHub | fine-tuning + ReAct style agent | fire safety |
| "xLAM" function calling tool use | xLAM | paper, leaderboard, GitHub | action / tool-use model | unrelated xLAM |
| "ReTool" tool use reinforcement learning LLM | ReTool | paper, GitHub | tool-use training or reformulation | generic retool platform |
| "ARTIST" agent reinforcement learning tool | ARTIST | paper, GitHub | agent/tool RL if confirmed | art generation |
| "RAGEN" LLM agent reinforcement learning | RAGEN | paper, GitHub | agent RL if confirmed | unrelated names |
| "Search-R1" reinforcement learning search | Search-R1 | paper, GitHub | search agent RL | search engine posts |
| "ToolRL" reinforcement learning tool LLM | ToolRL | paper, GitHub | tool-use RL | generic tool RL |

## 9. Adjacent fields

| Query string | Purpose | Expected source types | Inclusion criteria | Likely false positives |
|---|---|---|---|---|
| "potential-based reward shaping" survey | classical reward shaping | paper, survey | reward shaping principle 可迁移 | unrelated shaping examples |
| "hindsight experience replay" goal relabeling | HER | paper, survey | failed trajectory reuse / relabeling | robotics only, still useful as analogy |
| "long-horizon manipulation" reward sparse dense recovery policy | robotics long-horizon | paper, survey | recovery, irreversible action, state predicate | no transfer to tool-use unless framed as analogy |
| "code generation reinforcement learning" "unit tests" | code RL | paper, GitHub | execution feedback, hidden tests, repair loop | static code benchmark only |
| "PRM800K" process reward model | math PRM | paper, dataset | process supervision / step verifier | pure math, note analogy |
| "Math-Shepherd" process reward model | math PRM | paper, GitHub | verifier / PRM idea | pure math, note analogy |
| "contract checking" "differential testing" program synthesis LLM | formal methods | paper, survey | invariant / contract / differential tests for semantic reward | classic PL only |

