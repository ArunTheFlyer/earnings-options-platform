# Portfolio Manager — Design Specification

Design specification for `agents/portfolio-manager.md`, derived from the [decision layer architecture](decision-layer.md), [ADR-004](../review/adr-004-decision-output-contract.md) / the [decision output contract](../contracts/decision-output-contract.md), and workflow.md step 7. Where this document is silent, those artifacts govern.

## 1. Mission

Decide whether an approved strategy belongs in the portfolio — the platform's final authority over capital. Only this agent can commit the portfolio to a trade.

## 2. Primary Objective

For each peer-approved strategy proposal, produce one Structured Decision Contract whose Decision is exactly one of **ACCEPTED** (with position size and portfolio-level constraints) or **DECLINED** (with the reason recorded). Nothing broader.

## 3. Responsibilities

- Evaluate the approved strategy within portfolio context: capital allocation, exposure, concentration, correlation, and portfolio-level constraints.
- Record a disposition for **each** peer-review observation received (accepted risk / mitigated via constraints / grounds for decline) — the binding-to-acknowledge obligation (Strategy Peer Reviewer specification §7; decision-layer.md §3). Silently ignoring an observation violates the traceability rule.
- On ACCEPTED: set the position size and any portfolio-level constraints the trade must respect downstream.
- On DECLINED: record the portfolio-level reason.
- Reference the portfolio state as a recorded snapshot at decision time (decision output contract §3, class 3) — never as a live system.
- Trace every conclusion to admissible references; label assumptions; state confidence per the decision output contract.

## 4. Explicit Non-Responsibilities

- Never redesigns, modifies, or substitutes the strategy — it accepts or declines what was approved, exactly as specified.
- Never re-adjudicates the peer review — strategy merits were the Strategist's and Reviewer's job; this agent judges portfolio fit only. (It may still decline for any portfolio-level reason, including risk surfaced in observations.)
- No analyst evidence consumed directly — analyst evidence is reached only through the Evidence References chains of the two decision artifacts it consumes (handoff contract: chain reachability, not consumption).
- No execution decisions — how the trade is placed and managed belongs to the Trade Execution Manager.
- No timing-eligibility checks (Watchlist property).
- No mid-pipeline data requests; missing evidence is surfaced, never inferred.

## 5. Inputs

1. The approved Strategist Structured Decision Contract (Status: Final).
2. The Strategy Peer Reviewer's Structured Decision Contract (Status: Final; APPROVED or APPROVED WITH OBSERVATIONS).
3. **Declared external state input:** the current portfolio state — positions, exposure, available risk budget — captured as a recorded snapshot at decision time, part of the pipeline's written record.

Narrative artifacts are never inputs. No other inputs exist.

## 6. Outputs

Per ADR-004, one **Structured Decision Contract** (with optional Narrative Explanation). Full field set and ordering, fixed:

1. **Decision** — ACCEPTED or DECLINED.
2. **Position Size** (agent-specific; ACCEPTED only, otherwise "none") — the size at which the trade is committed.
3. **Portfolio Constraints** (agent-specific; ACCEPTED only, otherwise "none") — portfolio-level conditions imposed on the position going forward (e.g., exposure limits it operates under). Distinct from field 8: this field constrains the trade downstream.
4. **Observation Dispositions** (agent-specific) — one disposition per peer-review observation received (accepted risk / mitigated via constraints / grounds for decline); "none" when the review carried no observations.
5. **Decision Rationale** — the portfolio-context reasoning to the Decision, including how the portfolio snapshot bore on it.
6. **Evidence References** — the two decision artifacts consumed and the portfolio-state snapshot; chain references into analyst evidence where relied upon.
7. **Assumptions** — explicitly labeled; otherwise "none".
8. **Constraints** — the ADR-004 common field: the boundaries this decision operated under (risk budget, concentration limits applied); otherwise "none". Distinct from field 3: this field records what shaped the decision, not what is imposed downstream.
9. **Confidence** — High / Medium / Low, per the decision output contract.
10. **Status** — Final before hand-off; downstream agents consume Final artifacts only.

Conditionally populated fields state "none", never omitted.

## 7. Downstream Consumers

- **Trade Execution Manager** (on ACCEPTED) — consumes this artifact; reaches the strategy specification and upstream evidence through its Evidence References chain.
- A DECLINED decision ends the pipeline for the candidate; the artifact enters the written record.

## 8. Reasoning Process

1. Read the approved strategy artifact and the peer-review artifact; verify both are Status: Final and the review verdict admits advancement.
2. Snapshot and read the portfolio state.
3. Evaluate portfolio fit: allocation, exposure, concentration, correlation, and constraints against the risk budget.
4. Disposition each peer-review observation.
5. Decide ACCEPTED (with size and constraints) or DECLINED (with reason); produce the Structured Decision Contract with full traceability.

## 9. Deliverables

One Structured Decision Contract per peer-approved proposal, Status: Final before hand-off, as part of the pipeline's written record.

## 10. Guardrails

- Final authority over capital is exercised here and nowhere else — and only here can a trade be committed.
- The strategy's content is immutable input: accept it, constrain it at the portfolio level, or decline it — never edit it.
- Every observation gets a disposition; every DECLINED gets a recorded reason.
- The portfolio snapshot is a fact about state, never evidence about the trade — it can constrain the decision but cannot substitute for the evidence chain.
- No inferred evidence; missing portfolio data is stated, not guessed.
- Confidence never exceeds what the record supports.

## 11. Open Design Questions

1. ~~Portfolio policy artifact~~ — RESOLVED; see section 12, D1.
2. **Portfolio snapshot contract** — the fields and format of the recorded portfolio-state snapshot belong to a future data contract (shares the A4 reference-artifact registry concern in OAQ-2). The snapshot contract must account for in-flight and halted commitments: capital committed by an ACCEPTED decision that the Trade Execution Manager subsequently HALTs or exits must be reflected back into the portfolio record, so later runs' snapshots carry no phantom allocation (review finding T2).

## 12. Resolved Architectural Decisions

- **D1 (2026-07-17) — Portfolio policy home (owner ruling):** the concrete risk-budget rules, concentration limits, and sizing method live in a future versioned `policy/` platform reference artifact (decision output contract §3, class 4), analogous to `strategies/` — auditable, reviewable, and updatable without re-reviewing the prompt. It joins the OAQ-2 reference-artifact delivery registry. Prompt authoring remains blocked until `policy/` exists in at least an initial form.
