# Decision Output Contract

The canonical output interface for every decision-making agent, established by [ADR-004](../review/adr-004-decision-output-contract.md). This is the decision-layer counterpart of the [analyst output contract](analyst-output-contract.md) (ADR-002): it defines the common interface only, and does not redesign the [decision layer](../design/decision-layer.md).

## 1. Purpose

Every decision agent produces a **Structured Decision Contract** — the machine-consumable record of its decision. A **Narrative Explanation** may accompany it for human inspection but is never consumed by downstream automation.

## 2. Canonical Structured Decision Contract

Mandatory common fields for every decision artifact:

| Field | Definition |
|---|---|
| **Decision** | The explicit outcome produced by the agent. |
| **Decision Rationale** | The agent's reasoning, summarized from the structured evidence. |
| **Evidence References** | References to structured analyst fields, prior structured decision artifacts, declared external state inputs, and/or platform reference artifacts (see §3) only. |
| **Assumptions** | Any assumptions made while reaching the decision. |
| **Constraints** | Any decision boundaries or conditions that affected the outcome. |
| **Confidence** | Qualitative confidence in the decision process: High / Medium / Low. |
| **Status** | The lifecycle state of the decision artifact (e.g., Draft, Final). An agent finalizes its own artifact before hand-off; §6's no-modification rule governs other agents' artifacts, and downstream agents consume Final artifacts only. |

This contract defines the interface only. Business-specific values — the outcomes a given agent may return, its constraint semantics, its status vocabulary beyond the lifecycle concept — are defined by each agent's own design specification within the boundaries set by the decision-layer architecture.

Each decision agent may extend the contract with agent-specific fields while preserving these common elements; its design specification fixes the full field set and ordering.

## 3. Evidence Traceability

Every decision conclusion must reference one or more structured evidence elements.

- No conclusion may exist without traceable supporting evidence.
- Narrative explanations are not valid evidence references.

Four classes of reference are admissible:

1. **Structured analyst fields** — fields of an analyst Structured Output Contract.
2. **Prior structured decision artifacts** — Structured Decision Contracts produced earlier in the chain.
3. **Declared external state inputs** — external, non-pipeline state (e.g., current portfolio state; live market or order state at execution time) that an agent's design specification explicitly declares as an input. Two rules govern this class:
   - An external state reference must point at a **recorded snapshot** of that state at decision time, captured as part of the pipeline's written record — never at a live, mutable system. A decision that cannot be reconstructed after the fact from its recorded references violates the platform's auditability goal.
   - Declared external inputs are **facts about state, never evidence about the trade**. They may constrain a decision (e.g., available risk budget) but can never substitute for analyst evidence.
4. **Platform reference artifacts** — versioned repository content that decisions are made against, such as the approved strategy definitions in `strategies/`. These are not external state (a version/commit pins them; no snapshot is needed) and not evidence — they are the rulebook a decision applies. An agent's design specification declares which reference artifacts it uses. The Strategist compares against, and the Peer Reviewer adjudicates against, the same versioned strategy definitions.

## 4. Decision Inheritance

The pattern by which decision agents consume previous outputs (the pattern only; the workflow itself is defined in workflow.md):

- **Strategist** — consumes analyst structured contracts.
- **Peer Reviewer** — consumes analyst structured contracts plus the Strategist's structured contract (per [ADR-003](../review/adr-003-peer-review-independence.md)).
- **Portfolio Manager** — consumes the approved Strategist decision plus the Peer Reviewer decision.
- **Execution Manager** — consumes the approved portfolio decision.

Each stage's Evidence References field points at the structured artifacts it consumed, forming a traceable chain from execution back to analyst evidence.

## 5. Narrative Explanation

The companion artifact. It exists solely for:

- Human understanding.
- Debugging.
- Audit.
- Peer review.

It is never consumed by downstream automation. If the Narrative and the Structured Decision Contract disagree, the **Structured Decision Contract is authoritative**.

## 6. Cross-Agent Consistency Rules

- Decision agents never modify upstream evidence.
- Decision agents never modify prior decision artifacts.
- Every decision artifact references its inputs.
- Every downstream decision forms a traceable chain.
- Missing evidence is explicit.
- Inferred evidence is prohibited.
