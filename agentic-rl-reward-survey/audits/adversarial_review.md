# Adversarial Review

Status: completed on 2026-05-21.

## Review Notes

| Target | Challenge | Why it matters | Fix |
|---|---|---|---|
| Overall framing | The survey could overstate "agentic RL" when many sources are benchmark-only, SFT, inference-time, or diagnostic | Mislabeling evaluation as training would weaken the paper | Final survey must keep a paradigm column and explicitly label benchmark-only sources |
| Reward taxonomy | The taxonomy might imply every reward component should be combined | Over-complex reward can create more proxy hacking | State that components are templates; choose by oracle availability and risk |
| Cascade drift | Some Prompt 7 evidence is from ALFWorld/WebShop/search rather than API transactions | Transfer may be valid but not direct | Mark as long-horizon support, not proof of production API-agent reward design |
| Silent errors | It is tempting to call all silent errors hallucination | Wrong-entity, stale-state, policy-violation, and aggregation errors need different oracles | Preserve the distinction in Part 4 and final survey |
| Engineering checklist | Checklist items could read like paper-backed facts | Several are senior-engineering synthesis from the evidence, not individually proven | Label Part 5 as engineering practice and dashboard guidance |
| Gaps | Gaps could sound like solved paper ideas | The value is in making them testable | Keep each gap tied to possible experiment and reward design |
| Adjacent fields | Transfer analogies could be overclaimed | Classical RL/HER/robotics/formal methods do not automatically solve LLM tool-use | Use "可迁移版本" and "why not fully transferred" language |
| Final synthesis | The final survey may become too long by concatenating drafts | Repetition would obscure the core thesis | Integrate and compress; preserve tables and core formulas |

## Final-Survey Guardrails

- Lead with the three-way distinction: format, execution, semantic/task success.
- Keep source confidence caveats visible.
- Use "coverage insufficient" for KTO, SimPO, RLOO, cost reward, harmful-call refusal, rollback, and dynamic environment reward.
- Do not treat LLM-as-judge as an oracle.
- Do not imply benchmark score equals deployment readiness.
