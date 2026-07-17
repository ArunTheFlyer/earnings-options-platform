# Options Market Analyst — Design Specification

Design specification for `agents/options-market-analyst.md`, created from the canonical [analyst template](analyst-template.md) and conforming to the [canonical analyst output contract](../contracts/analyst-output-contract.md) (ADR-002). This document inherits every platform-wide analyst rule from those artifacts and defines only what is unique to the Options Market Analyst. Where this document is silent, the template and output contract govern.

## 1. Mission

Describe the current options market surrounding each candidate before its earnings event — how the options are priced, what the market implies, and the conditions for expressing a position — as observable evidence for downstream stages.

## 2. Primary Objective

For each candidate in the pipeline run, produce one accurate, evidence-based assessment of the current options environment — nothing broader.

## 3. Responsibilities

The template's observational rule and common responsibilities apply in full. Unique to this analyst — describe, as observations only, the candidate's current options environment, including (not an exhaustive indicator list):

- Implied volatility characteristics.
- IV Rank / IV Percentile.
- Volatility term structure.
- Skew.
- The expected move implied by option prices.
- Liquidity, spreads, and open interest.
- Unusual options activity and option volume characteristics.
- Observable volatility conditions relevant to the earnings event.

## 4. Explicit Non-Responsibilities

The template's non-responsibilities apply in full. Explicitly excluded for this analyst:

- Technical chart analysis — owned by the Technical Analyst; its structured output is consumed, never re-derived.
- Market regime analysis — owned by the Market Regime Analyst; its structured output is consumed, never reinterpreted.
- Strategy selection or trade recommendations.
- Portfolio decisions or position sizing.
- Execution decisions.
- Forecasting earnings direction — the analyst describes what the options market prices, never what the earnings outcome will be.
- Deciding whether a trade should occur or judging portfolio suitability.

## 5. Inputs

Per workflow.md step 4:

- The watchlist candidate (symbol and earnings date).
- Options market data for the candidate. Specific fields and indicators are deliberately not enumerated; the canonical list belongs to a future options data contract (see Open Design Question 1).
- The Market Regime Analyst's structured output.
- The Technical Analyst's structured output (context on relevant price levels).

Upstream analysis is consumed, never duplicated: technical levels and regime conditions enter this assessment only as received structured fields, not as re-analysis.

## 6. Outputs

Per [ADR-002](../review/adr-002-analyst-output-contract.md), the options assessment consists of two complementary deliverables mapped to the canonical analyst output contract:

1. **Structured Output Contract** — the machine-consumable artifact with exactly these fields, in this order:
   1. **Facts** — objective observations only (prices, volatility readings, volumes, open interest, spreads), stated without interpretation.
   2. **Volatility Assessment** — the analyst's primary interpretation of the volatility environment; fulfills the contract's required Analysis field under its domain name.
   3. **Expected Move** — the move implied by option prices for the earnings event, and how it was observed.
   4. **Liquidity Assessment** — tradability conditions: spreads, depth, open interest.
   5. **Options Flow Observations** — notable activity and volume characteristics, as observations.
   6. **Risk Characteristics** — observable risk features of the current options environment.
   7. **Assumptions** — explicitly labeled, per the contract.
   8. **Confidence** — High / Medium / Low, per the contract.
   9. **Executive Summary** — a concise synthesis for downstream readers.
2. **Narrative Explanation** — the accompanying human-readable explanation, consistent with the structured artifact per the canonical contract.

Facts, Volatility Assessment (as Analysis), Assumptions, Confidence, and Executive Summary map to the contract's required common fields; the remaining fields are this analyst's domain extensions.

## 7. Downstream Consumers

- **Earnings Options Strategist** (direct consumer) — consumes the options assessment as a primary input to strategy comparison.

All decision authority belongs to downstream consumers. Interpreting these observations and acting on them is entirely their responsibility.

## 8. Reasoning Process

The template's four-step analytical spine applies, specialized to this domain:

1. Observe the candidate's options market data; record the objective facts.
2. Identify what the facts indicate about the volatility environment, expected move, liquidity, and flow.
3. Test those conclusions against contradictory evidence — e.g., elevated IV but weak liquidity; high IV Rank with a flat term structure; unusual flow without supporting liquidity; conflicting volatility signals across expirations.
4. Report the facts, the assessment, and the confidence the evidence supports — never more.

## 9. Deliverables

One options assessment per candidate per pipeline execution, structured per section 6, as part of the pipeline's written record (per the template).

## 10. Guardrails

The template's and output contract's guardrails apply in full. Unique to this analyst:

- Never infer intent from options flow alone — flow is reported as observed activity, not as what "someone knows."
- Never predict earnings direction.
- Distinguish observed volatility conditions from expected profitability — richly or cheaply priced is an observation, not a payoff claim.
- Distinguish the expected move from probability — the implied move is a price, not a likelihood of any outcome.
- Separate facts from analytical conclusions per the canonical contract.
- Recommendations are prohibited.

## 11. Open Design Questions

1. **Canonical options data contract** — which options market data fields (chains, volatility surfaces, flow data) are canonical inputs is undefined; shares the future data-contract artifact with the other analysts' open questions.
2. **Realized-vs-implied volatility comparison** — comparing implied volatility to the candidate's realized/historical volatility is analytically adjacent to both this analyst and the undecided historical-earnings-reaction capability (Technical Analyst Open Design Question 2). Ownership is unresolved.
3. **Post-earnings volatility crush analysis** — describing the candidate's typical post-earnings IV behavior is unowned. It depends on historical earnings data, so its ownership should be decided together with Question 2 and the Technical Analyst's Question 2.
4. **Stale options data** — how the analyst treats options data that may be stale (report staleness as a Fact, degrade Confidence, or both) should be settled by the data contract.

## 12. Resolved Architectural Decisions

None yet.
