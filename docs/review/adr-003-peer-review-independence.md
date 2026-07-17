# ADR-003: Peer Review Independence

- **Decision ID:** ADR-003
- **Title:** Peer Reviewer Independently Evaluates Original Analyst Evidence
- **Status:** Accepted
- **Context:** The Decision Layer architecture left one gate open: whether the Strategy Peer Reviewer consumes only the Strategist's output, or also the original analyst structured contracts. Reviewing only the Strategist's proposal would make the reviewer dependent on the Strategist's representation of the evidence — a summary could, deliberately or not, omit or distort what the analysts actually reported.
- **Decision:** The Strategy Peer Reviewer independently evaluates the original analyst Structured Output Contracts in addition to the Strategist's Structured Output Contract. The reviewer does not consume narrative artifacts (per [ADR-002](adr-002-analyst-output-contract.md) and the [analyst handoff contract](../contracts/analyst-handoff-contract.md), narratives are never inputs to automated reasoning).
- **Rationale:** Reviewer independence requires direct access to original evidence. Independent reconstruction of the reasoning from the same structured evidence — then comparison against the Strategist's conclusions — is what makes the review a genuine check rather than a re-reading of the proposal.
- **Alternatives Considered:** (a) Reviewer consumes only the Strategist's output — rejected: the reviewer would inherit any omission or distortion in the Strategist's evidence summary. (b) Reviewer consumes analyst narratives as well — rejected: violates ADR-002's rule that narratives never feed automated reasoning.
- **Consequences:**
  - Review quality improves.
  - Strategist summaries cannot hide omitted evidence.
  - Structured contracts become the authoritative review interface.
  - Narrative artifacts remain excluded from automated reasoning.
- **Related Files:** `docs/design/decision-layer.md` (section 8: adopted decision, Reviewer Evidence Model, Reviewer Outputs), `docs/contracts/analyst-handoff-contract.md`, `docs/review/adr-002-analyst-output-contract.md`
