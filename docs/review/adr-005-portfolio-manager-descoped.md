# ADR-005: Portfolio Manager Descoped — Human Portfolio Decisions

- **Decision ID:** ADR-005
- **Title:** Portfolio Manager agent descoped; the owner performs portfolio decisions manually
- **Status:** Accepted
- **Context:** The Portfolio Manager design specification passed review, but its prompt was gated on an owner-authored `policy/` artifact (PM spec D1). While drafting the policy, the owner determined that encoding portfolio rules (capital caps, sizing) is not yet warranted — the portfolio decision is better made manually until real usage shows a need for automation.
- **Decision:** The Portfolio Manager **agent** is descoped for now. The owner performs the portfolio-decision stage manually: consuming the Strategist and Peer Reviewer artifacts, deciding accept/decline and position size against their own judgment and account state. The PM design specification remains in the repository as a passed, dormant spec — the blueprint if the agent is built later. `policy/portfolio-policy.md` authoring is paused (no longer gates anything); the draft copy remains untracked.
- **Rationale:** The pipeline's value through the Peer Reviewer (evidence, proposal, adversarial check) is intact and automated; the capital decision is the owner's own money and lowest-frequency step, where manual judgment is cheapest and automation least urgent. Deferring avoids fixing policy numbers (per-trade caps, budgets) before any live usage informs them.
- **Alternatives Considered:** (a) Author policy/ now and build the PM prompt — rejected: premature calibration of business numbers with no usage data. (b) Remove the PM from the architecture entirely — rejected: the spec is passed and the decision hierarchy still describes the stage; only its executor changes (human instead of agent).
- **Consequences:**
  - The pipeline's automated span is: analysts → Strategist → Strategy Peer Reviewer. The portfolio stage is human-executed; decision authority over capital rests with the owner directly (consistent with the decision hierarchy — final authority over capital simply stays human).
  - The Trade Execution Manager's input contract (an ACCEPTED portfolio decision artifact) is produced manually or informally by the owner; whether the TEM agent is used or execution is also manual is the owner's per-trade choice.
  - The PM row in ROADMAP/review-status is marked descoped, not pending — Phase 1 agent implementation is complete for the in-scope agent set.
  - Revisit trigger: the owner sees a need (e.g., decision volume, consistency concerns, or audit gaps in the manual step).
- **Related Files:** `docs/design/portfolio-manager-design.md` (dormant, passed), `docs/design/decision-layer.md`, `policy/policy-template.md`, `agents/portfolio-manager.md` (remains empty)
