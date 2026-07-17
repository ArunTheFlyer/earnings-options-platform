# Workflow

The end-to-end pipeline from candidate symbols to managed trades. Each stage consumes the accumulated outputs of the stages before it and produces a written artifact for the next.

```
Watchlist
   ↓
Market Regime Analyst
   ↓
Technical Analyst
   ↓
Options Market Analyst
   ↓
Earnings Options Strategist
   ↓
Strategy Peer Reviewer
   ↓
Portfolio Manager
   ↓
Trade Execution Manager
```

A rejection or "no trade" conclusion at any stage ends the pipeline for that candidate.

---

## 1. Watchlist

**Purpose:** Define the universe of candidate symbols with upcoming earnings events that are worth analyzing.

**Inputs:** The trader's curated list of symbols and their scheduled earnings dates.

**Outputs:** The candidate symbol(s) and earnings event(s) to be analyzed in this run.

**Consumer:** Technical Analyst — the first consumer of the watchlist — and all subsequent stages, which analyze the same candidates. (The Market Regime Analyst is symbol-agnostic and does not consume the watchlist.)

---

## 2. Market Regime Analyst

**Purpose:** Identify and explain the current market regime so that all downstream analysis is grounded in current conditions rather than assumptions.

**Inputs:** Market-wide information only (broad market data; the canonical indicator list will be defined in a separate data contract). This agent is completely symbol-agnostic and receives no watchlist or candidate information.

**Outputs:** A regime assessment — observations only: the regime classification, the facts and interpretation behind it, and environment-level risks present in the current environment. It contains no instructions to downstream agents.

**Consumer:** Technical Analyst, and every stage after it. Consumers are responsible for interpreting the regime observations and acting on them within their own decisions.

---

## 3. Technical Analyst

**Purpose:** Interpret the candidate's price action so the strategist knows where the stock stands and which levels matter.

**Inputs:** The watchlist candidates (this stage is the watchlist's first consumer); the candidate's price history and chart data; the regime assessment.

**Outputs:** A technical assessment — trend, key support and resistance levels, notable patterns, and how the stock is positioned heading into earnings. Evidence only; no trade recommendation.

**Consumer:** Options Market Analyst and Earnings Options Strategist.

---

## 4. Options Market Analyst

**Purpose:** Assess how the options market is pricing the earnings event, so strategy design is grounded in what a position actually costs and implies.

**Inputs:** The candidate symbol and earnings date; options chain data (implied volatility, pricing across strikes and expirations); the technical assessment for context on relevant price levels.

**Outputs:** An options-market assessment — the expected move implied by option prices, the volatility environment for this symbol, and how richly or cheaply different structures are priced. Evidence only; no trade recommendation.

**Consumer:** Earnings Options Strategist.

---

## 5. Earnings Options Strategist

**Purpose:** Synthesize all upstream evidence into a concrete trade proposal — or an explicit "no trade" conclusion.

**Inputs:** The regime assessment, technical assessment, and options-market assessment for the candidate.

**Outputs:** A recommendation: a fully specified strategy proposal (structure, strikes, expiration, rationale, risk profile, and the evidence it rests on), a reasoned "no trade" conclusion, or an "insufficient evidence" outcome.

**Consumer:** Strategy Peer Reviewer — all outcomes, including NO TRADE and INSUFFICIENT EVIDENCE, are peer-reviewed (per the C1 ruling of 2026-07-17: caution has no safe harbor from scrutiny).

---

## 6. Strategy Peer Reviewer

**Purpose:** Independently challenge the proposal before it can reach capital — checking that the strategy actually follows from the evidence, that assumptions are stated, and that risks are honestly represented.

**Inputs:** The strategist's structured decision contract plus all upstream analyst structured contracts, consumed independently (per ADR-003). Narrative artifacts are never review inputs.

**Outputs:** A review verdict — APPROVED or APPROVED WITH OBSERVATIONS (the proposal becomes an approved strategy), or REJECTED with one or more defect categories. Objections must cite evidence, not preference. There is no return-for-revision path.

For NO TRADE and INSUFFICIENT EVIDENCE outcomes, the review is a **recorded audit finding, not a gate**: the candidate's run ends after review regardless of verdict — a REJECTED verdict on a non-trade outcome cannot resurrect the candidate; it exists so wrongly-cautious reasoning is documented and reviewable after the fact.

**Consumer:** Portfolio Manager (on approval of a strategy proposal); otherwise the pipeline ends for this candidate and the review verdict enters the written record.

---

## 7. Portfolio Manager

**Purpose:** Decide whether the approved strategy belongs in the portfolio — the final authority over capital.

**Inputs:** The approved strategist decision artifact and the peer-review decision artifact (analyst evidence is reached through their Evidence References chains, not consumed directly); current portfolio state (positions, exposure, available risk budget) as a declared external state input, recorded as a snapshot per the decision output contract.

**Outputs:** An accept/decline decision. On accept: the position size and any portfolio-level constraints the trade must respect. On decline: the reason, recorded.

**Consumer:** Trade Execution Manager (on accept); a decline ends the pipeline.

---

## 8. Trade Execution Manager

**Purpose:** Carry out the accepted trade and manage it through its lifecycle. Execution only — this stage never revisits whether the trade should exist.

**Inputs:** The approved portfolio decision artifact (the accepted strategy with its approved size and constraints); upstream evidence is reached through its Evidence References chain, not consumed directly.

**Outputs:** Order plan and placement, fills, and ongoing lifecycle management (monitoring, adjustment, and exit per the strategy's defined plan), plus a record of outcomes.

**Consumer:** The trader and the portfolio record. Outcome records are also the raw material for future post-trade review.
