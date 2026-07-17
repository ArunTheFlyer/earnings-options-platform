# ADR-002: Analyst Output Contract

- **Decision ID:** ADR-002
- **Title:** Analyst Output Contract — Structured Artifact plus Narrative Explanation
- **Status:** Accepted
- **Context:** Analyst output contracts were becoming increasingly narrative. Downstream agents would have had to parse prose to extract evidence, making orchestration fragile and consumption non-deterministic. Raised as Technical Analyst Open Design Question 5 and escalated as a platform-level architectural decision blocking further analyst specifications.
- **Decision:** All analyst agents produce two complementary artifacts per assessment: (1) a **Structured Output Contract** — the machine-consumable interface with stable field names, deterministic structure, no prose paragraphs, no recommendations, no ambiguity; and (2) a **Narrative Explanation** — a human-readable explanation that supports the structured contract, references evidence, stays consistent with it, and never introduces conclusions or recommendations absent from it. The canonical interface, including the required common fields (Facts, Analysis, Assumptions, Confidence, Executive Summary) and the domain-extension rule, is defined in [docs/contracts/analyst-output-contract.md](../contracts/analyst-output-contract.md).
- **Rationale:**
  - Stable machine interfaces.
  - Easier orchestration.
  - Deterministic downstream consumption.
  - Better peer review.
  - Human-readable explanations without forcing downstream agents to parse prose.
- **Alternatives Considered:** (a) Narrative-only outputs — rejected: non-deterministic to consume, fragile for orchestration. (b) Structured-only outputs — rejected: loses the explanatory reasoning chain that peer review and human inspection require. (c) A single hybrid artifact mixing structure and prose — rejected: reintroduces ambiguity about which content is the interface.
- **Consequences:**
  - All future analyst specifications inherit this contract.
  - Existing analyst specifications (Market Regime Analyst, Technical Analyst) align to it.
  - Downstream agents consume the structured artifact.
  - The narrative exists primarily for human inspection and debugging.
- **Related Files:** `docs/contracts/analyst-output-contract.md`, `docs/design/analyst-template.md`, `docs/design/market-regime-analyst-design.md`, `docs/design/technical-analyst-design.md`
