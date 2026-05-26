# Silent Errors Supplementary Sources

Last updated: 2026-05-21.

This topic note records the supplementary Prompt 8 search around silent failures, plausible-but-wrong tool use, semantic mismatch in function calling, hallucinated tool results, and execution-grounded verification. These sources were added after the Prompt 4 work-note pass, so they are covered here as a topic note rather than as full `notes/works/{id}.md` files.

## Included Sources

| Source id | Why included | Prompt 8 use | Caveat |
|---|---|---|---|
| `agenthallu_2026` | AgentHallu introduces hallucination attribution for multi-step LLM-agent trajectories with responsible-step labels and a taxonomy that includes tool-use hallucinations. | Detection methods: hallucination attribution and first bad step localization for silent/hallucinated trajectories. | It is an evaluation benchmark, not a reward method; reported localization difficulty means it should not be treated as a solved oracle. |
| `toolgate_2026` | ToolGate formalizes tool calls with preconditions, postconditions, symbolic state, and verified state commits. | Detection and reward design: invariant/contract checking, false-success prevention, and verified execution before state update. | Contract coverage is only as good as the written pre/postconditions; open business semantics still need domain oracles. |
| `internal_tool_hallu_2026` | This work detects tool-selection hallucinations, malformed parameters, and tool-bypass behavior from internal representations during the same forward pass. | Detection methods: early interception of hallucinated tools or parameters before execution. | It requires access to model internal states and therefore may not apply to closed black-box model APIs. |
| `butterfly_toolchains_2025` | This work gives a taxonomy of failed parameter filling in LLM tool-agent systems and studies how input perturbations cause invocation-chain failures. | Error taxonomy: schema-valid but semantically wrong arguments, parameter-name hallucination, source mismatch, and parameter consistency checks. | It is diagnostic rather than a training reward paper; reward use requires converting categories into checkers and penalties. |
| `reasoning_trap_2025` | This work studies whether reasoning enhancement, including reasoning RL, amplifies tool hallucination in no-tool and distractor-tool settings. | Gap/risk evidence: better reasoning is not automatically better tool grounding; reward design must jointly optimize capability and reliability. | Treat as a diagnostic risk source, not as evidence for a mature mitigation. |

## Search Notes

- Prompt 8 queries used: `silent failure LLM agents`, `plausible but wrong tool use`, `semantic mismatch function calling`, `hallucinated tool result`, `execution grounded verification LLM tools`, `tool call semantic correctness`, `BFCL function calling evaluation`, `tau-bench tool call correctness`, `ToolBench evaluation error`, and related arXiv-specific variants.
- Blog and Reddit results were useful for query discovery but were not added as core sources.
- `Clicking the Void: LLM Agent is Hallucinating and Where to Find Them` appeared relevant, but the accessible result was an anonymous submission PDF, so it was not added to the core source set in this pass.
- BFCL, tau-bench, APIGen, APIGen-MT, ToolPRMBench, SWE-bench, WebArena, ToolLLM/ToolBench, and CM2 were already in the source set and were reused in Part 4.

## PDF Status

PDFs were downloaded successfully for all five Prompt 8 supplementary sources:

- `pdf/agenthallu_2026.pdf`
- `pdf/toolgate_2026.pdf`
- `pdf/internal_tool_hallu_2026.pdf`
- `pdf/butterfly_toolchains_2025.pdf`
- `pdf/reasoning_trap_2025.pdf`
