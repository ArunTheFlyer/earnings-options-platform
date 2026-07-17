# Analyst Handoff Contract

The canonical analyst-to-consumer interface: how downstream agents consume analyst outputs. This contract governs every hand-off from an analyst to a decision-making agent, complementing the [analyst output contract](analyst-output-contract.md) (which defines what analysts produce) by defining how those outputs are consumed.

## Consumption Rules

1. Downstream agents consume **ONLY the Structured Output Contract**.
2. The Narrative Explanation is **never** used as an input for automated decision-making.
3. The narrative exists for human inspection, debugging, and peer review.
4. Structured fields are the **single source of truth** for everything an analyst observed.
5. If the Structured Output and the Narrative ever disagree, the **Structured Output is authoritative**.

A downstream agent that cites narrative content as evidence for a decision has violated this contract, even if the same content exists in the structured artifact — decisions cite structured fields.

## Evidence Precedence

When a consumer receives multiple analyst outputs, the following conflict-resolution principles apply. These are principles, not business weights: no scoring, no voting, no numeric blending of analyst outputs.

1. **Each analyst owns its own domain.** Within its domain (see the ownership matrix below), an analyst's structured output is the authoritative evidence. No other agent's opinion of that domain competes with it.
2. **Analysts never overwrite another analyst's observations.** An analyst that receives upstream structured output passes it through as received; it never restates, adjusts, or re-derives another domain's evidence.
3. **Consumers synthesize evidence across analysts.** Combining regime, technical, and options evidence into a judgment is exclusively the consumer's job — synthesis is where decision authority lives, and it happens downstream of all analysts.
4. **Contradictions are surfaced, never silently resolved.** When evidence from different analysts conflicts, the consumer must state the contradiction and how its decision accounts for it. Dropping or quietly averaging conflicting evidence violates the explainability principle.
5. **Missing evidence is preferable to inferred evidence.** A consumer must never fill a gap in one analyst's output using another analyst's domain or its own inference. Absent evidence is handled by the consumer's own contract for missing inputs (e.g., the strategist's INSUFFICIENT EVIDENCE outcome).

## Analyst Ownership Matrix

The canonical map of which analyst owns which capability, and who consumes it. Only capabilities already defined in the current architecture are listed.

**Consumed By** lists *direct consumption only* — agents that read the artifact as an input. Later stages (Portfolio Manager, Trade Execution Manager) do not consume analyst artifacts directly; they reach analyst evidence exclusively through the Evidence References chain of the decision artifacts they consume (see the [decision output contract](decision-output-contract.md), §4). That is *chain reachability*, not consumption, and it is deliberately excluded from this matrix.

| Capability | Owning Analyst | Consumed By (direct) |
|---|---|---|
| Market regime (environment classification, environment risks) | Market Regime Analyst | Technical Analyst (context), Options Market Analyst (context), Earnings Options Strategist, Strategy Peer Reviewer |
| Technical state (trend, key levels, patterns, current price structure into earnings) | Technical Analyst | Options Market Analyst (price-level context), Earnings Options Strategist, Strategy Peer Reviewer |
| Options environment (implied volatility, expected move, liquidity, flow, risk characteristics) | Options Market Analyst | Earnings Options Strategist, Strategy Peer Reviewer |

Capabilities without an owner are not listed here; they are tracked in [open-architecture-questions.md](../review/open-architecture-questions.md) until ownership is decided.
