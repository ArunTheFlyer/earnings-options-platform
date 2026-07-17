# Architecture Decision Records

A living record of every significant architectural decision in this project. Decisions are appended chronologically and never deleted; a superseded decision has its Status changed and points to its successor.

## Decision Template

```markdown
## ADR-NNN: <Title>

- **Decision ID:** ADR-NNN
- **Title:** <short imperative title>
- **Status:** Proposed | Accepted | Superseded by ADR-NNN
- **Context:** <the situation and forces that made a decision necessary>
- **Decision:** <what was decided, stated plainly>
- **Rationale:** <why this option won>
- **Alternatives Considered:** <options rejected, and why>
- **Consequences:** <what becomes easier, what becomes harder, what is now constrained>
- **Related Files:** <paths affected by or embodying this decision>
```

---

## ADR-001: Architecture-First Development

- **Decision ID:** ADR-001
- **Title:** Architecture-First Development
- **Status:** Accepted
- **Context:** The project began as a set of empty agent prompt files. Writing the prompts immediately would have let each agent implicitly define its own responsibilities, contracts, and authority, with conflicts discovered only after the fact — producing a collection of prompts rather than an engineered platform.
- **Decision:** Establish the architecture, documentation, review process, and agent responsibilities **before** implementing any agent prompts. The repository structure, architecture and workflow documents, design principles, glossary, and this governance process all precede agent authoring, and agents are written to conform to them.
- **Rationale:** Agent prompts are implementations of contracts. Defining the contracts first — each agent's single responsibility, its inputs and outputs, its place in the decision hierarchy — makes every prompt reviewable against a canonical reference instead of against opinion, and localizes future changes to the agent that owns them.
- **Alternatives Considered:** (a) Write agent prompts first and extract the architecture from them afterwards — rejected because the architecture would then codify accidents of drafting rather than deliberate design. (b) Evolve structure informally without governance documents — rejected because consistency across seven agents cannot be maintained by memory alone.
- **Consequences:** Agent authoring is slower to start but each prompt has an unambiguous specification to meet; reviews have objective criteria; documentation must be kept current as the architecture evolves, which is accepted overhead. All future agents and documents are bound by the review process in [review-plan.md](review-plan.md).
- **Related Files:** `README.md`, `docs/architecture.md`, `docs/workflow.md`, `docs/design-principles.md`, `docs/glossary.md`, `docs/review/`

---

## ADR-002: Analyst Output Contract

Recorded as a standalone document: [adr-002-analyst-output-contract.md](adr-002-analyst-output-contract.md).

- **Status:** Accepted
- **Summary:** All analyst agents produce two complementary artifacts — a Structured Output Contract (machine-consumable interface) and a Narrative Explanation (human-readable support). Canonical interface: `docs/contracts/analyst-output-contract.md`.
