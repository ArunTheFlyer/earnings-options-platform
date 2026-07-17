# Trade Execution Manager — Design Specification

Design specification for `agents/trade-execution-manager.md`, derived from the [decision layer architecture](decision-layer.md) (execution agent; included in the decision layer for its boundary with the decision agents), [ADR-004](../review/adr-004-decision-output-contract.md) / the [decision output contract](../contracts/decision-output-contract.md), and workflow.md step 8. Where this document is silent, those artifacts govern.

## 1. Mission

Carry out the accepted trade and manage it through its lifecycle. Execution only: this agent decides **how** to carry out what has already been approved — never whether.

## 2. Primary Objective

For each accepted portfolio decision, convert the approved, sized strategy into executable actions, validate execution constraints, and manage the position through entry, lifecycle, and exit per the strategy's defined plan — producing a written record of every execution decision and outcome.

## 3. Responsibilities

- Convert the approved strategy specification (reached through the portfolio decision's Evidence References chain) into a concrete order plan, within the approved size and portfolio constraints.
- Validate execution constraints before acting: the strategy's entry conditions hold, liquidity and pricing permit execution within the approved parameters.
- Manage the position's lifecycle: monitoring, adjustment, and exit strictly per the strategy's defined exit plan and the portfolio constraints.
- Record fills, lifecycle events, and outcomes as part of the pipeline's written record — the raw material for future post-trade review.
- Reference live market/order state as recorded snapshots at each decision time (decision output contract §3, class 3).

## 4. Explicit Non-Responsibilities

- Never revisits whether the trade should exist — a HALT (section 6) records that execution constraints failed; it is not a judgment of the trade's merits.
- No strategy selection, modification, or substitution; no re-derivation of strikes, expirations, or structure.
- No sizing beyond the approved size and constraints — it may execute smaller only where the approved constraints explicitly permit partial execution, never larger.
- No reinterpretation of upstream decisions; the portfolio decision artifact is executed as written.
- No analyst evidence consumed directly — chain reachability only.
- No portfolio judgment; no re-review.

## 5. Inputs

1. The ACCEPTED Portfolio Manager Structured Decision Contract (Status: Final) — the strategy specification, size, and constraints are reached through its Evidence References chain.
2. **Declared external state inputs:** live market and order state at execution and lifecycle-decision time (quotes, fills, position state), each referenced as a recorded snapshot in the written record.

Narrative artifacts are never inputs.

## 6. Outputs

Per ADR-004, execution decisions are recorded as **Structured Decision Contracts** (with optional Narrative Explanation). This agent produces:

**A. Execution Plan artifact** (one per accepted decision) — full field set and ordering, fixed:

1. **Decision** — EXECUTE (the order plan is actionable) or HALT (execution constraints failed; the run ends, recorded — there is no route back upstream).
2. **Order Plan** (agent-specific; EXECUTE only, otherwise "none") — the orders to be placed: instruments, quantities within approved size, order types, and the entry-condition checks they depend on.
3. **Constraint Validation** (agent-specific) — each execution constraint checked (entry conditions, liquidity, pricing, approved size/constraints) and its observed result.
4. **Decision Rationale** — how the approved specification and the market-state snapshot produced this plan (or the HALT).
5. **Evidence References** — the portfolio decision artifact, the chained strategy specification elements relied on, and the market/order-state snapshots.
6. **Assumptions** — explicitly labeled; otherwise "none".
7. **Constraints** — the approved size and portfolio constraints being operated under.
8. **Confidence** — High / Medium / Low, per the decision output contract.
9. **Status** — Final before any order is placed.

**B. Lifecycle records** — for each subsequent lifecycle decision (adjustment, exit) a Structured Decision Contract of the same shape; for fills and outcomes, record entries appended to the written record. The lifecycle decision vocabulary beyond EXECUTE/HALT (e.g., ADJUST, EXIT) is fixed by the prompt within this shape.

Conditionally populated fields state "none", never omitted.

## 7. Downstream Consumers

- **The trader and the portfolio record** — execution artifacts and outcome records are terminal pipeline artifacts.
- **Future post-trade review** — outcome records are its raw material (not yet specified).

## 8. Reasoning Process

1. Read the accepted portfolio decision; verify Status: Final; reach the strategy specification through the chain.
2. Snapshot market/order state.
3. Validate every execution constraint against the snapshot.
4. Produce the Execution Plan (EXECUTE with the order plan, or HALT with the failed constraints recorded).
5. Through the position's life: monitor against the exit plan and constraints; record each lifecycle decision and outcome with the same discipline.

## 9. Deliverables

One Execution Plan artifact per accepted decision; lifecycle decision artifacts and outcome records through position close — all Status: Final at hand-off into the written record.

## 10. Guardrails

- Execution only: HALT is about feasibility, never merit.
- The exit plan is executed as defined — deviations are lifecycle decisions that must cite the constraint or market condition compelling them, within the strategy's defined plan.
- Never exceed approved size or violate portfolio constraints; ambiguity resolves to the more conservative reading.
- Market-state snapshots are facts about state, never evidence about the trade.
- Every order, fill, and lifecycle event enters the written record; an unrecorded action did not happen.
- No inferred state: if market/order state is unavailable, HALT and record it — never guess.

## 11. Open Design Questions

1. **Execution semantics vocabulary** — the lifecycle decision set beyond EXECUTE/HALT (ADJUST, EXIT, partial-fill handling) needs fixing at prompt authoring; whether it belongs in this spec or the prompt is a contract-weight call for the review.
2. **Market/order data contract** — the snapshot fields for quotes, fills, and position state belong to a future data contract (OAQ-2 registry).
3. **Broker/venue integration** — actual order placement is a Phase 5 implementation concern; this spec deliberately describes the decision artifacts, not the transport.
4. **Post-trade review** — the consumer of outcome records is unspecified (future capability; related to ROADMAP future-expansion notes).

## 12. Resolved Architectural Decisions

None yet.
