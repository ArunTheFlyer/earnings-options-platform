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
| **Evidence References** | References to structured analyst fields and/or prior structured decision artifacts only. |
| **Assumptions** | Any assumptions made while reaching the decision. |
| **Constraints** | Any decision boundaries or conditions that affected the outcome. |
| **Confidence** | Qualitative confidence in the decision process: High / Medium / Low. |
| **Status** | The lifecycle state of the decision artifact (e.g., Draft, Final). |

This contract defines the interface only. Business-specific values — the outcomes a given agent may return, its constraint semantics, its status vocabulary beyond the lifecycle concept — are defined by each agent's own design specification within the boundaries set by the decision-layer architecture.

Each decision agent may extend the contract with agent-specific fields while preserving these common elements; its design specification fixes the full field set and ordering.

## 3. Evidence Traceability

Every decision conclusion must reference one or more structured evidence elements.

- No conclusion may exist without traceable supporting evidence.
- Narrative explanations are not valid evidence references.

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
