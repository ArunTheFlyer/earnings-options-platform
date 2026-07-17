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
2. **Order Plan** (agent-specific; populated when the Decision places or changes orders — EXECUTE, ADJUST, EXIT; otherwise "none") — the orders to be placed or changed: instruments, quantities within approved size, order types, and the condition checks they depend on.
3. **Constraint Validation** (agent-specific) — each execution constraint checked (entry conditions, liquidity, pricing, approved size/constraints) and its observed result.
4. **Decision Rationale** — how the approved specification and the market-state snapshot produced this plan (or the HALT).
5. **Evidence References** — the portfolio decision artifact, the chained strategy specification elements relied on, and the market/order-state snapshots.
6. **Assumptions** — explicitly labeled; otherwise "none".
7. **Constraints** — the approved size and portfolio constraints being operated under.
8. **Confidence** — High / Medium / Low, per the decision output contract.
9. **Status** — Final before any order is placed.

**B. Lifecycle records** — for each subsequent lifecycle decision a Structured Decision Contract of the same shape; for fills and outcomes, record entries appended to the written record. The lifecycle decision vocabulary is fixed here (decision output contract §2: outcome values belong to the design specification, never the prompt):

- **ADJUST** — a change to the open position, permitted only within the strategy's defined plan and the approved portfolio constraints.
- **EXIT** — closing the position: per the exit plan, or a constraint-compelled early close with the compelling condition cited.

Partial fills are a fill state handled within EXECUTE's Constraint Validation — not a decision outcome. The complete outcome set is: EXECUTE, HALT, ADJUST, EXIT.

**HALT lifecycle scope (review finding E2):** HALT is available only before any order is placed; once a position exists, HALT is not in the vocabulary. If state becomes unavailable post-fill, the agent records the unavailability and takes no action (acting requires state; no order requires none), reassessing at the next valid snapshot — and EXITs per the defined plan if, at that point, the exit plan's conditions are met or a constraint compels it. Invariant: **the record must never show a position as closed or capital as released that is not.**

Conditionally populated fields state "none", never omitted.

## 7. Downstream Consumers

- **The trader and the portfolio record** — execution artifacts and outcome records are terminal pipeline artifacts. HALT and exit outcomes enter the portfolio record, so committed-but-undeployed or released capital is reflected there and later runs' portfolio snapshots carry no phantom allocation (review finding T2; the snapshot contract accounts for this — PM spec, Open Design Question 2).
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
- No inferred state: never guess. Pre-fill, unavailable state means HALT, recorded. Post-fill, unavailable state means record-and-hold per section 6B's HALT lifecycle scope — a forced exit without valid state would violate the same principle.

## 11. Open Design Questions

1. ~~Execution semantics vocabulary~~ — RESOLVED; see section 12, D1.
2. **Market/order data contract** — the snapshot fields for quotes, fills, and position state belong to a future data contract (OAQ-2 registry).
3. **Broker/venue integration** — actual order placement is a Phase 5 implementation concern; this spec deliberately describes the decision artifacts, not the transport.
4. **Post-trade review** — the consumer of outcome records is unspecified (future capability; related to ROADMAP future-expansion notes).

## 12. Resolved Architectural Decisions

- **D1 (2026-07-17) — Lifecycle vocabulary fixed in-spec (review finding T1; owner-accepted set):** the outcome set is EXECUTE, HALT, ADJUST, EXIT, defined in section 6B; partial fills are a fill state within EXECUTE's Constraint Validation, not an outcome. Vocabulary lives in this specification per decision output contract §2 — outcome values are never defined by the prompt. Closes the former Open Design Question 1.
- **D2 (2026-07-17) — Order Plan population (review finding E1, lockstep):** Order Plan is populated for every order-placing or order-changing Decision (EXECUTE, ADJUST, EXIT) — an EXIT without an order plan is unexecutable; §6B lifecycle artifacts share the §6A shape.
- **D3 (2026-07-17) — HALT lifecycle scope (review finding E2, reviewer's record-and-hold rule adopted):** HALT is pre-fill only; post-fill unavailable state is recorded-and-held with reassessment at the next valid snapshot. Invariant: the record never shows a position closed or capital released that is not.
