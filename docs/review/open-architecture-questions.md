# Open Architecture Questions

Platform-level architectural questions that span more than one agent or document. Consolidating them here prevents drift: a cross-cutting question lives in exactly one place, and affected specifications reference it rather than maintaining independent copies. When a question is resolved, it becomes an ADR (or a D-record in the owning specification) and is marked resolved here.

---

## OAQ-1: Who owns historical earnings-event analytics?

**Status:** RESOLVED 2026-07-17 (owner ruling) — a future dedicated analyst

**Resolution:** Historical earnings-event analytics (price reactions, realized-vs-implied volatility comparisons, post-earnings volatility crush, and related analytics) will be owned by a future dedicated **Earnings History Analyst**, to be specified later from the analyst template. No existing analyst absorbs the capability. Until that analyst exists, consumers are specified to tolerate its absence: the Earnings Options Strategist design specification consumes its output when present and proceeds without it otherwise, and the Analyst Ownership Matrix gains the capability row only when the analyst is specified. The strategist prompt's current "historical earnings behavior where provided" expectation is realigned as part of the Strategist specification work (review finding F6).

**Original status:** Open

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

**Context:** The reviewer prompt enforces reconstruction-before-comparison by instruction only; all inputs share one context, so the model *can* see the Strategist's artifact before reconstructing. Raised during the 2026-07-17 prompt review of agents/strategy-peer-reviewer.md. No action until orchestration (Phase 4) is designed. Additionally (analyst prompt review finding A4, 2026-07-17): the Phase 4 orchestration design owns a registry of every reference artifact each prompt expects delivered into context — the approved regime taxonomy, the data contracts, and the `strategies/` definitions — since these provided-reference delivery duties are accumulating across prompts.

**Referenced by:** agents/strategy-peer-reviewer.md (Thinking Process); Strategy Peer Reviewer design specification (§10).
