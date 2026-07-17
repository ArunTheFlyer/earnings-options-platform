# Open Architecture Questions

Platform-level architectural questions that span more than one agent or document. Consolidating them here prevents drift: a cross-cutting question lives in exactly one place, and affected specifications reference it rather than maintaining independent copies. When a question is resolved, it becomes an ADR (or a D-record in the owning specification) and is marked resolved here.

---

## OAQ-1: Who owns historical earnings-event analytics?

**Status:** Open

**Question:** Which component owns analytics derived from a candidate's historical earnings events?

This explicitly includes:

- Historical earnings price reactions.
- Realized vs. implied volatility comparisons.
- Post-earnings volatility crush.
- Other analytics derived from historical earnings events.

**Context:** This capability cluster was surfaced independently three times — as Technical Analyst Open Design Question 2 (historical earnings price reactions), and Options Market Analyst Open Design Questions 2 and 3 (realized-vs-implied comparison; post-earnings volatility crush). All depend on the same underlying historical earnings data, so ownership should be decided once for the whole cluster — the candidates include an existing analyst or a future dedicated analyst. Ownership is intentionally undecided.

**Known dependents:**

- The Earnings Options Strategist's prompt currently expects "historical earnings behavior where provided" from the technical assessment; that expectation is unbacked until ownership is decided and will need realignment.
- The Analyst Ownership Matrix in [analyst-handoff-contract.md](../contracts/analyst-handoff-contract.md) lists no owner for this capability until resolved.

**Referenced by:** Technical Analyst design specification (Open Design Question 2); Options Market Analyst design specification (Open Design Questions 2 and 3).

---

## OAQ-2: Structural enforcement of reviewer independence (Phase 4)

**Status:** Open — deferred to orchestration design (Phase 4)

**Question:** Should orchestration deliver the Strategy Peer Reviewer's inputs in stages — analyst structured contracts first, the Strategist's artifact only after the independent reconstruction is emitted — so that reconstruction-first independence is structural rather than behavioral?

**Context:** The reviewer prompt enforces reconstruction-before-comparison by instruction only; all inputs share one context, so the model *can* see the Strategist's artifact before reconstructing. Raised during the 2026-07-17 prompt review of agents/strategy-peer-reviewer.md. No action until orchestration (Phase 4) is designed.

**Referenced by:** agents/strategy-peer-reviewer.md (Thinking Process); Strategy Peer Reviewer design specification (§10).
