
_Template only — not an approved strategy. Copy to `strategies/<strategy-name>.md`, fill every section, and submit for review. A strategy is approved only after passing review (Repository Rule). The universe is capped at five strategies (decision-layer.md §3); all must be defined-risk._

## Strategy Name

<!-- The canonical name the Strategist's artifacts will cite. -->

## Structure

<!-- The legs: instruments, long/short, relative strikes and expirations. This is what the Strategist specifies concretely and the Execution Manager places. -->

## Defined-Risk Profile

<!-- Required: maximum loss (how it is bounded), payoff characteristics. Undefined-risk structures are prohibited at the platform level. -->

## Directional Fit

<!-- What directional/volatility view this structure expresses (or that it is non-directional). -->

## Volatility Exposure

<!-- Exposure to implied volatility (entering and through the event) and time decay characteristics. -->

## Entry Criteria

<!-- The evidence conditions under which this strategy is a candidate: what the options-market/technical/regime assessments must show. Written against structured-contract fields, not raw data. -->

## Exit Plan

<!-- The defined plan the Execution Manager executes: profit/loss/time exits, post-earnings handling. Deviations outside this plan are not available to any agent. -->

## Management Rules

<!-- Permitted adjustments (the Execution Manager's ADJUST outcome is bounded by this section), or "none". -->

## Not This Strategy When

<!-- Disqualifying conditions, stated against structured-contract fields (e.g., liquidity below X, term structure shaped Y). The Strategist's per-strategy comparison lines and the Peer Reviewer's Strategy-selection-error checks test directly against this section. -->

## Primary Risks

<!-- The known failure modes of the structure. -->

## Evaluation Notes

<!-- How this strategy tends to score on the comparison criteria (risk-adjusted return, edge capture, probability of profit) and in which conditions it typically wins or loses the comparison. -->
