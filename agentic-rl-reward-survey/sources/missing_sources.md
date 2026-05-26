# Missing Sources

Prompt 2/3 已完成第一轮尝试检索。已找到可信 URL 的工作写入 `sources/sources.jsonl` 和 `matrices/work_taxonomy.csv`。本表保留尝试记录、仍缺信息和下一轮抽取重点。

| Name | Attempted queries | Why it matters | Missing information | Next search suggestion |
|---|---|---|---|---|
| ToolFormer | "ToolFormer language models use tools" | early tool-use training reference | Resolved: `toolformer_2023`; extract API-call filtering details | inspect paper sections on self-supervised API call annotation |
| ReAct | "ReAct reasoning acting language models" | reasoning + acting baseline | Resolved: `react_2022`; confirm task/environment taxonomy | extract action/observation framing |
| Reflexion | "Reflexion language agents verbal reinforcement learning" | self-reflection / verbal reinforcement reference | Resolved: `reflexion_2023`; clarify that this is not gradient RL | extract feedback-memory loop and benchmark details |
| ToolLLM / ToolBench | "ToolLLM ToolBench paper" | tool-use benchmark and data | Resolved: `toolllm_toolbench_2023`; extract ToolEval metric details | inspect ICLR/OpenReview version and GitHub docs |
| APIBench / API-Bank | "APIBench LLM API benchmark"; "API-Bank tool-augmented LLMs" | API-use benchmark | Resolved as `api_bank_2023`; naming conflict remains with APIBench references | check whether ToolLLM's APIBench is separate from API-Bank |
| AgentBench | "AgentBench LLM agents benchmark" | multi-environment agent benchmark | Resolved: `agentbench_2023`; extract per-environment metrics | inspect benchmark taxonomy |
| WebArena | "WebArena autonomous agents benchmark" | web agent benchmark | Resolved: `webarena_2023`; extract task success and failure modes | inspect project page and paper appendix |
| SWE-bench | "SWE-bench agent benchmark" | software engineering agent benchmark | Resolved: `swe_bench_2023`; extract execution reward and test-pass caveats | inspect SWE-bench variants separately later |
| tau-bench / tau-bench | "tau-bench language agents" | API transaction/customer-service benchmark | Resolved: `tau_bench_2024`; extract policy compliance metrics | inspect GitHub and domains |
| ToRA | "ToRA tool integrated reasoning agents" | tool-integrated reasoning | Resolved: `tora_2023`; extract output-space shaping and tool interaction | inspect method section |
| AgentTuning | "AgentTuning LLM agents" | agent fine-tuning baseline | Resolved: `agenttuning_2023`; extract AgentInstruct composition | inspect ACL version if needed |
| FireAct | "FireAct LLM agents" | ReAct-style fine-tuning / agent work | Resolved: `fireact_2023`; extract search API setup | inspect project page |
| xLAM | "xLAM function calling tool use" | function calling/action model | Resolved: `xlam_2024`; reward/training details need extraction | inspect Salesforce release and paper |
| ToolACE | "ToolACE LLM tool use" | tool-use data/eval/training candidate | Resolved: `toolace_2024`; confirm no RL training claim | inspect data generation pipeline |
| APIGen | "APIGen LLM API tool use" | API/tool-use data generation | Resolved: `apigen_2024`; extract three-stage verification | inspect NeurIPS paper |
| APIGen-MT | "APIGen-MT multi-turn tool use" | multi-turn API/tool-use | Resolved: `apigen_mt_2025`; metadata needs deeper extraction | inspect arXiv full text |
| ReTool | "ReTool tool use reinforcement learning LLM" | tool-use RL candidate, name ambiguous | Resolved: `retool_2025`; distinguish from commercial Retool platform | inspect RL objective and benchmark setup |
| ARTIST | "ARTIST agent reinforcement learning tool" | agent RL/tool-use candidate, name ambiguous | Resolved: `artist_2025`; older unrelated artist-agent RL exists | inspect acronym and reward details |
| RAGEN | "RAGEN LLM agent reinforcement learning" | agent RL candidate | Resolved: `ragen_2025`; extract StarPO details | inspect rollout/reward sections |
| Search-R1 | "Search-R1 reinforcement learning search" | search RL candidate | Resolved: `search_r1_2025`; compare with R1-Searcher | inspect reward function |
| ToolRL | "ToolRL reinforcement learning tool LLM" | tool-use RL candidate | Resolved: `toolrl_2025`; details still shallow | inspect full paper and code if available |
| BFCL / Berkeley Function Calling Leaderboard | "Berkeley Function Calling Leaderboard BFCL" | function calling benchmark | Resolved: `bfcl_2024`; canonical source corrected to OpenReview / ICML 2025 after arXiv false hit; versioning still needs tracking | compare official leaderboard and BFCL version history if later writing benchmark evolution |
| process reward model for LLM agents | "process reward model LLM agent tool use"; "AgentPRM" | step-level reward evidence | Resolved: `agentprm_framework_2025`, plus `toolprmbench_2026` | inspect AgentPRM and ToolPRMBench labels |
| GRPO tool-use training | "GRPO tool use LLM agent"; "multi-turn tool use reinforcement learning GRPO" | latest RL method evidence | Resolved through `turn_level_credit_assignment_2025`, `task_reasoning_grpo_2025`, `cm2_2026`; exact GRPO variants need extraction | inspect GRPO objective details and benchmark overlap |
| RLAIF / DPO for tool use | "DPO tool use LLM agent"; "RLAIF tool use LLM agent" | preference/AI feedback methods | Partially resolved: `dmpo_agents_2024`, `epo_2024`, `rlaif_api_code_2024`; RLAIF agent tool-use evidence remains narrow | search for stronger tool-specific RLAIF work in later pass |

## Prompt 8 Silent-Error Search Notes

Prompt 8 searched around silent failure, plausible-but-wrong tool use, semantic mismatch in function calling, hallucinated tool results, execution-grounded verification, BFCL/tau-bench/ToolBench evaluation limitations, APIBench/API-Bank ambiguity, and tool-call semantic correctness.

Resolved as supplementary topic-note sources:

| Name | Attempted queries | Why it matters | Status | Next search suggestion |
|---|---|---|---|---|
| AgentHallu | "AgentHallu hallucination attribution LLM agents"; "LLM agent hallucination attribution tool use" | Detecting and localizing hallucinated or silent-failure steps in multi-step agent trajectories | Resolved: `agenthallu_2026`; topic note only | Create full `notes/works/agenthallu_2026.md` if final synthesis makes detailed claims |
| ToolGate | "contract grounded verified tool execution LLM"; "ToolGate verified tool execution" | Contract/invariant checking for preconditions, postconditions, and verified state commit | Resolved: `toolgate_2026`; topic note only | Extract exact contract model and benchmark setup before strong claims |
| Internal representations for tool hallucination | "Internal Representations as Indicators of Hallucinations in Agent Tool Selection" | Early detection of incorrect tool selection, malformed parameters, and tool bypass | Resolved: `internal_tool_hallu_2026`; topic note only | Check whether method requires white-box model internals |
| Butterfly Effects in Toolchains | "failed parameter filling LLM Tool-Agent systems"; "Butterfly Effects in Toolchains" | Parameter-filling failures are a central schema-valid but semantically wrong class | Resolved: `butterfly_toolchains_2025`; topic note only | Convert failure taxonomy into verifier/penalty examples in Prompt 10 |
| Reasoning Trap | "reasoning trap tool hallucination"; "enhancing LLM reasoning amplifies tool hallucination" | Risk evidence that reasoning enhancement, including reasoning RL, can worsen tool grounding | Resolved: `reasoning_trap_2025`; topic note only | Use as limitation / anti-pattern evidence, not as mitigation |
| Clicking the Void | "Clicking the Void LLM Agent hallucinating where to find them" | Potentially relevant hallucination localization work | Not added; accessible result appeared anonymous / not stable enough | Revisit if a stable published version appears |
