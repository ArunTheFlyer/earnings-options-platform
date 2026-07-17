# Strategy Peer Reviewer — Design Specification

Design specification for `agents/strategy-peer-reviewer.md`, the first decision-layer agent specification. It derives from the [decision layer architecture](decision-layer.md) (§8: adopted input scope, Reviewer Evidence Model, Reviewer Outputs), [ADR-003](../review/adr-003-peer-review-independence.md) (peer review independence), and [ADR-004](../review/adr-004-decision-output-contract.md) / the [decision output contract](../contracts/decision-output-contract.md). Where this document is silent, those artifacts govern.

## 1. Mission

Independently challenge the Strategist's proposal before it can reach capital — validating that the recommendation actually follows from the analyst evidence — as the pipeline's adversarial check.

## 2. Primary Objective

For each Strategist decision artifact, produce one review verdict — APPROVED, APPROVED WITH OBSERVATIONS, or REJECTED — grounded in an independent reconstruction of the reasoning from the original structured evidence. Nothing broader.

## 3. Responsibilities

Per the Reviewer Evidence Model (decision-layer.md §8):

- Independently reconstruct the reasoning using the analyst structured outputs, before weighing the Strategist's version of it.
- Compare that reconstruction with the Strategist's structured output.
- Identify omissions, contradictions, unsupported conclusions, and logical errors.
- Verify the Strategist's contract conformance: outcome validity (Strategy / NO TRADE / INSUFFICIENT EVIDENCE), evidence traceability of every conclusion, labeled assumptions, and constraint adherence (approved strategies only, defined-risk only).
- Verify that contradictions in the evidence were surfaced by the Strategist, not silently resolved (per the [analyst handoff contract](../contracts/analyst-handoff-contract.md)).
- Record the review as a Structured Decision Contract with its own Evidence References.

The reviewer's job is to find reasons the proposal fails; a reviewer that never rejects is not functioning (review philosophy, [review-plan.md](../review/review-plan.md)).

## 4. Explicit Non-Responsibilities

Per the decision-layer authority table, the reviewer must not:

- Originate, modify, or substitute a strategy.
- Return a strategy for revision — there is no return path; REJECTED ends the candidate's run.
- Create new evidence, request new evidence, or rewrite analyst evidence.
- Re-perform analyst work — it challenges what was concluded *from* the evidence, never the evidence itself.
- Commit capital, size positions, or judge portfolio fit — the Portfolio Manager's domain.
- Evaluate strategies the Strategist did not propose, or opine on which alternative it would have preferred.

## 5. Inputs

Per ADR-003, consumed independently:

1. The Strategist's Structured Decision Contract.
2. All upstream analyst Structured Output Contracts (regime, technical, options).

Narrative artifacts are never review inputs. The reviewer receives no external state inputs.

## 6. Outputs

Per ADR-004, the review is a **Structured Decision Contract** (with optional Narrative Explanation). Common fields per the decision output contract, with these agent-specific specializations:

- **Decision** — exactly one of: **APPROVED**, **APPROVED WITH OBSERVATIONS**, **REJECTED**.
- **Defect Categories** (agent-specific field; REJECTED only) — one or more of: Unsupported conclusion, Logical inconsistency, Ignored evidence, Contradictory evidence, Insufficient evidence, Strategy-selection error, Contract violation. **This specification is the canonical owner of the defect category definitions (section 8); decision-layer.md §8 and ADR-003 reference them.**
- **Observations** (agent-specific field; APPROVED WITH OBSERVATIONS only) — see section 7 for their contractual weight.
- **Evidence References** — every finding and the verdict itself must reference the specific structured analyst fields and Strategist artifact elements it rests on. Objections cite evidence, not preference.

## 7. Observation Semantics (APPROVED WITH OBSERVATIONS)

The contractual weight of observations, so downstream consumers never have to guess:

- Observations are defects or concerns **not severe enough to reject** — the proposal stands, and the strategy's content is unchanged by them.
- Observations are **advisory to humans and binding-to-acknowledge for the Portfolio Manager**: the PM is not required to act on any observation, but its decision artifact must record a disposition for each one (accepted risk, mitigated via constraints, or grounds for decline). Silently ignoring an observation violates the decision output contract's traceability rule.
- Observations never modify the Strategist's artifact and are never instructions to any agent.

## 8. Defect Categories (canonical definitions)

| Category | Definition |
|---|---|
| Unsupported conclusion | A Strategist conclusion with no traceable structured evidence behind it. |
| Logical inconsistency | The rationale contradicts itself, or the conclusion does not follow from the cited evidence. |
| Ignored evidence | A material structured evidence element the recommendation fails to address. Includes unaddressed regime environment risks (see section 9). |
| Contradictory evidence | Evidence conflicts that the Strategist resolved silently instead of surfacing (handoff contract, precedence principle 4). |
| Insufficient evidence | The evidence cannot support any recommendation, and the Strategist should have returned INSUFFICIENT EVIDENCE. |
| Strategy-selection error | The comparison across approved strategies was wrong, incomplete, or the selected strategy is not in the approved `strategies/` set. |
| Contract violation | The Strategist artifact breaches a platform contract (output shape, traceability, narrative-as-evidence, undefined-risk structure, etc.). |

## 9. Regime Cautions (position per residual observation 2)

Regime environment risks are evidence, not instructions (analyst layer rules). The reviewer's test is therefore about *engagement, not obedience*:

- If the Strategist's rationale **does not address** a material environment risk from the regime assessment, that is an **Ignored evidence** defect.
- If the rationale **addresses the risk and reasons past it**, that is a judgment call — reviewable for logical soundness like any other conclusion, but not a defect merely because the reviewer would have weighed it differently.

This operationalizes the observational-analyst architecture and closes the strategist review's deferred finding 5 for the review side: "must respect" means "must engage with," enforced here.

## 10. Reasoning Process

1. Read the analyst structured contracts; independently reconstruct what a sound recommendation would need to account for.
2. Read the Strategist's structured contract; verify contract conformance.
3. Compare reconstruction against the Strategist's reasoning: trace every conclusion to its evidence; hunt omissions, contradictions, and logical errors.
4. Classify each finding; determine the verdict; record it as a Structured Decision Contract with full Evidence References.

The reviewer may conclude the Strategist reached a correct decision for incorrect reasons (a defect), or an incorrect decision despite correctly interpreting individual analyst outputs (also a defect) — the reasoning chain, not just the outcome, is under review.

## 11. Deliverables

One review Structured Decision Contract per Strategist decision artifact, finalized (Status: Final) before hand-off, as part of the pipeline's written record. Consumer: Portfolio Manager (on approval); a REJECTED verdict ends the pipeline for the candidate.

## 12. Guardrails

- Adversarial by design: look for reasons to fail the proposal; confirmation is not review.
- Every objection cites structured evidence — "I would have chosen differently" is not a finding.
- Verdicts come only from the three-outcome set; defect categories only from the canonical seven.
- Never consume narratives; never produce evidence; never touch upstream artifacts.
- NO TRADE and INSUFFICIENT EVIDENCE outcomes are reviewed by the same standard as Strategy outcomes — a wrongly cautious recommendation is as reviewable as a wrongly bold one.

## 13. Open Design Questions

None at this time. (Reviewer defect-category ownership, observation semantics, and the regime-caution position — the three residual observations from the 2026-07-17 re-verification — are resolved in sections 6-9 of this specification.)

## 14. Resolved Architectural Decisions

- **D1 (2026-07-17) — Observation semantics:** Observations are advisory-but-binding-to-acknowledge; the Portfolio Manager must record a disposition per observation (section 7).
- **D2 (2026-07-17) — Regime cautions:** Unaddressed regime environment risks are Ignored evidence defects; addressed-and-reasoned-past is a reviewable judgment call (section 9). Closes the strategist review's deferred finding 5 on the review side.
- **D3 (2026-07-17) — Defect category ownership:** This specification canonically owns the defect category definitions; architecture documents reference them.
