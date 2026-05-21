# Review Plan

Last updated: 2026-05-21

## Objective

Write a source-grounded systematic review of reward design for agentic RL in API/tool-use/function-calling settings. The review should explain what reward signals are used, where they fail, and how future systems can separate syntax correctness, execution success, semantic success, and long-horizon task success.

## Core Questions

1. What reward and penalty types appear in tool-use or API-use agents?
2. How do format reward, execution reward, verifier reward, process reward, preference reward, and task-level semantic reward differ?
3. Which works train policies with RL or RL-like optimization, and which are benchmark-only or inference-time methods?
4. How do cascade drift, silent errors, and plausible-but-wrong tool outcomes bypass surface rewards?
5. Which reward-design anti-patterns lead to reward hacking, benchmark overfitting, or brittle agents?
6. Which ideas from adjacent fields can transfer to agentic tool-use reward design?

## Boundaries

Primary scope:

- LLM or language-agent systems that call tools, APIs, functions, search, browsers, code execution, web environments, software repositories, or transaction systems.
- RL or RL-like optimization, including PPO, GRPO, RLOO, REINFORCE, DPO, RLAIF, preference optimization, process reward models, verifiers, execution feedback, self-reflection, and inference-time search when they affect reward design.
- Benchmarks and failure analyses that clarify reward/metric design for tool-use agents.

Out of scope unless used as adjacent evidence:

- Generic chat RLHF without tool/API/function-calling behavior.
- Product demos or marketing posts without method, benchmark, metric, or reward details.
- Traditional RL works with no clear transfer path to LLM tool-use agents.

## Evidence Rules

- Every non-obvious claim must trace back to `sources/sources.jsonl`.
- A work may enter `matrices/*.csv` only after it has a stable id, URL, confidence rating, and source type.
- Benchmark-only works must stay labeled as benchmark-only or evaluation framework.
- Prompting or self-reflection works such as ReAct or Reflexion may be included, but must not be described as RL training unless the source supports that claim.
- Missing or uncertain evidence should be written as `unknown`, `TODO`, `hypothesis`, or `此处覆盖不足`.

## Source Pipeline

1. Run first-pass representative source collection from `prompt-02-first-source-collection.md`.
2. Run a focused 2024-2026 update pass from `prompt-03-latest-2024-2026.md`.
3. Deduplicate source ids and normalize source types.
4. Write canonical entries to `sources/sources.jsonl`.
5. Update `sources/missing_sources.md` with attempted queries, unresolved ambiguities, and next searches.
6. Update `matrices/work_taxonomy.csv` only from accepted sources.
7. Record conflicts or naming ambiguity in `sources/source_conflicts.md`.

## Extraction Pipeline

For each accepted work, create `notes/works/{id}.md` with:

- bibliographic identity and canonical URL
- task/environment setting
- training paradigm
- reward signal and reward granularity
- verifier, execution feedback, preference, or process-reward details
- benchmark and metric details
- failure modes or caveats
- exact evidence ids for important claims

Do not synthesize section drafts until the relevant notes and matrices exist.

## Draft Structure

| Draft | Purpose |
|---|---|
| `drafts/part1_field_map.md` | Field map and taxonomy of agentic RL reward design |
| `drafts/part2_reward_inventory.md` | Reward-signal inventory and examples |
| `drafts/part3_cascade_drift.md` | Long-horizon cascade drift and credit assignment |
| `drafts/part4_silent_errors.md` | Silent failures and semantic mismatch |
| `drafts/part5_engineering_checklist.md` | Practical reward-design checklist |
| `drafts/part6_gaps_antipatterns.md` | Gaps, anti-patterns, and open problems |
| `drafts/part7_adjacent_fields.md` | Transferable ideas from adjacent fields |
| `drafts/final_survey.md` | Final synthesis after audits |

## Agent Execution Model

Use one main agent as source-of-truth owner. Sidecar agents may search independently, but should return candidate sources and uncertainty notes instead of writing shared files.

Safe sidecar scopes:

| Sidecar | Scope |
|---|---|
| representative works | ToolFormer, ReAct, Reflexion, ToRA, AgentTuning, FireAct, xLAM, ToolACE, ReTool, ARTIST, RAGEN, Search-R1, ToolRL |
| benchmarks | ToolBench, APIBench, AgentBench, WebArena, SWE-bench, tau-bench, BFCL, APIGen-MT |
| latest RL methods | 2024-2026 GRPO/RLOO/RLAIF/DPO/tool-use RL/process reward |
| failure modes | cascade drift, silent errors, reward hacking, benchmark limitations |

Shared files owned only by the main agent:

- `sources/sources.jsonl`
- `sources/source_conflicts.md`
- `matrices/*.csv`
- `drafts/final_survey.md`
- `audits/*.md`

## Quality Gates

Before writing drafts:

- `sources/sources.jsonl` has canonical URLs and confidence levels.
- `matrices/work_taxonomy.csv` separates RL training, benchmark-only, prompting, inference-time, and adjacent works.
- each major section has enough accepted sources or explicitly says coverage is insufficient.

Before final synthesis:

- citation audit confirms every strong claim has evidence.
- claim audit flags unsupported generalizations.
- coverage audit checks representative works, benchmarks, latest 2024-2026 methods, and failure modes.
- adversarial review checks whether reward categories are conflated.

## Current Stop Point

Prompt 2/3 source collection has completed one pass. Resume from `HANDOFF.md`, then execute `agentic-rl-reward-survey-prompts/prompt-04-extract-reward-info.md`.
