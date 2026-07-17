## Structure

- Entered at pipeline timing (a Watchlist property; no strategy-specific window is imposed).
- Buy one deep ITM LEAPS call (Delta 0.80–0.95) with 9–24 months until expiration to serve as the covering leg — a synthetic long stock position for the duration of this trade only (see Exit Plan; the LEAPS is not a standing holding).
- Sell one OTM call (Delta 0.20–0.30) at the nearest expiration AFTER the scheduled earnings announcement, to collect elevated pre-earnings implied volatility through the event.
- Buy one call at the same strike as the short call at the next available expiration after the short call's (weekly if listed, otherwise the next monthly), creating a calendar overlay that spans the earnings event.
- The calendar overlay improves management flexibility around earnings while keeping every leg of the position covered.

---

## Defined-Risk Profile

- Maximum loss is limited to the total net debit paid (LEAPS debit + calendar overlay debit − short call premium collected).
- The short call is fully covered by the same-strike calendar long; the LEAPS is a standalone long. No leg carries undefined risk and no margin-expansion path exists.
- As a descriptive structural fact: the position requires substantially less capital than purchasing 100 shares of stock. (This is a property of the structure, not an evaluation criterion — capital efficiency is excluded from strategy comparison by the Strategist's contract; if it is to influence sizing, that lives in `policy/`.)

---

## Directional Fit

Moderately bullish, expressed strictly as structured evidence. This strategy is a candidate only when ALL of the following hold in the analyst structured contracts:

- **Technical Analyst — Trend Assessment:** the current technical state is classified as an uptrend (or equivalent bullish classification in the approved vocabulary), and
- **Technical Analyst — Key Levels / Earnings Context:** price is above the nearest identified support level heading into the event.

No agent supplies or requires a discretionary "outlook"; the bullish condition is exactly the evidence above.

---

## Volatility Exposure

- Positive Delta from the LEAPS.
- Positive Vega from the LEAPS; Negative Theta from the LEAPS.
- Positive Theta from the short call.
- The calendar overlay partially offsets implied volatility changes across the event.
- The structure is designed to collect elevated pre-earnings implied volatility on the short leg while the covered structure rides through the post-earnings volatility crush.

---

## Entry Criteria

All conditions stated against structured-contract fields; every condition must hold:

- Directional Fit conditions above are satisfied (Technical Analyst structured contract).
- **Options Market Analyst — Volatility Assessment:** implied volatility on the expiration spanning earnings is elevated for this symbol (e.g., IV Rank ≥ 50 where the data contract provides it, or the Volatility Assessment classifies the pre-earnings volatility environment as elevated).
- **Options Market Analyst — Liquidity Assessment:** liquidity is adequate on ALL legs — LEAPS, short call, and overlay expirations (tight spreads and open interest per the assessment; a poor liquidity classification on any leg disqualifies).
- Deep ITM LEAPS meeting the Delta 0.80–0.95 band is available.

---

## Exit Plan

All conditions are testable from market/order-state snapshots alone. Exit when the FIRST of the following occurs:

1. **Profit:** after the earnings announcement, the short call can be bought back for 20% or less of the premium collected — close the short call and the overlay leg, then close the LEAPS.
2. **Loss limit:** the total position value falls to 70% of the total net debit paid (a 30% loss on the position) — close all legs.
3. **Time:** in all cases, all legs — including the LEAPS — are closed no later than 5 trading days after the earnings announcement. This strategy is an earnings trade: no leg outlives the post-event window.

---

## Management Rules

These rules are the complete ADJUST rulebook; no other adjustments are available.

- **Pre-earnings roll:** the short call may be rolled only if the roll is executed for a net credit at the same or a higher strike, into an expiration still after the earnings announcement.
- **Assignment risk:** if the short call is in the money with fewer than 2 trading days to its expiration, close or roll it (per the rule above) at the next valid snapshot.
- No adjustments to the LEAPS or the overlay leg; they are opened at entry and closed per the Exit Plan.

---

## Not This Strategy When

Disqualifying conditions, tested against structured-contract fields — any one disqualifies:

- Technical Analyst Trend Assessment classifies the state as downtrend, neutral, or deteriorating (any non-bullish classification).
- Options Market Analyst Liquidity Assessment reports poor liquidity or wide spreads on any required leg.
- Options Market Analyst Volatility Assessment does not classify pre-earnings implied volatility as elevated (insufficient premium on the short leg).
- Implied volatility is so elevated across all expirations that the LEAPS itself is priced at extreme volatility (the Volatility Assessment or term structure indicates long-dated IV is also extreme), removing the calendar edge.
- Another approved strategy provides a superior risk-adjusted opportunity in the Strategist's comparison.

---

## Primary Risks

- Time decay on the LEAPS over the trade window.
- Adverse implied volatility changes, including the post-earnings volatility crush reaching the LEAPS.
- Early assignment of the short call.
- Large adverse price movement through the event.
- Increased management complexity from three option legs.
- Execution risk when rolling or closing legs around earnings.

---

## Evaluation Notes

For the Strategist's comparison (per its success criteria):

- **Risk-adjusted return:** maximum profit potential across the event window relative to the bounded net debit; the short premium collected is the primary return driver.
- **Edge:** the strategy's edge is the differential between elevated event-priced IV on the short leg and the flatter IV on the covering legs; it weakens when the term structure is flat or long-dated IV is also extreme.
- **Probability of profit:** favored by a modest upward drift or contained move; hurt by a large gap in either direction (down through support, or up through the short strike faster than the covered structure gains).

This strategy typically wins the comparison when the technical state is bullish, event IV is rich, and term structure is steep; it typically loses to simpler defined-risk structures when liquidity is thin across three legs or long-dated IV is elevated.
