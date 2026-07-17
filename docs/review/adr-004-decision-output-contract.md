# ADR-004: Decision Output Contract

- **Decision ID:** ADR-004
- **Title:** Canonical Decision Output Contract for All Decision Agents
- **Status:** Accepted
- **Context:** The analyst layer has a canonical output interface ([ADR-002](adr-002-analyst-output-contract.md)) and hand-off rules, and the decision-layer architecture is established with the reviewer evidence model ([ADR-003](adr-003-peer-review-independence.md)). Without an equivalent common interface for decision agents, each decision specification would invent its own output shape, making the decision chain non-deterministic to consume and the audit trail inconsistent.
- **Decision:** All decision agents produce a canonical **Structured Decision Contract** plus an optional **Narrative Explanation**. The contract's mandatory common fields (Decision, Decision Rationale, Evidence References, Assumptions, Constraints, Confidence, Status), the evidence-traceability rule, the decision-inheritance pattern, and the cross-agent consistency rules are defined canonically in [docs/contracts/decision-output-contract.md](../contracts/decision-output-contract.md).
- **Rationale:**
  - Deterministic downstream interfaces.
  - Complete audit trail.
  - Evidence traceability.
  - Easier peer review.
  - Consistent orchestration.
- **Alternatives Considered:** (a) Let each decision-agent specification define its own output format — rejected: inconsistent interfaces, fragile orchestration, no uniform audit trail. (b) Reuse the analyst output contract unchanged for decision agents — rejected: decisions need fields evidence artifacts do not (Decision, Evidence References, Constraints, Status), and analyst outputs must remain recommendation-free, which decision outputs by definition are not.
- **Consequences:**
  - Every decision specification inherits the same interface.
  - Structured decision artifacts become the canonical automation interface.
  - Narratives remain human-facing only (consistent with ADR-002 and the reviewer evidence model of ADR-003).
- **Related Files:** `docs/contracts/decision-output-contract.md`, `docs/design/decision-layer.md`, `docs/contracts/analyst-output-contract.md`, `docs/review/adr-002-analyst-output-contract.md`, `docs/review/adr-003-peer-review-independence.md`
