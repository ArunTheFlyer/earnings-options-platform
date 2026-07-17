# Run Trigger — Sample Prompts

Sample prompts for starting a pipeline run (template: orchestration-design.md §1a). Any of these forms works; the orchestrator maps them to the same four fields.

---

## 1. Minimal — ticker only

```
Run the pipeline on AVGO
```

The orchestrator fetches the earnings date, echoes it back ("AVGO reports Jul-29 AMC, 12 days out — proceed?"), and runs on confirmation.

---

## 2. Ticker + earnings date (recommended)

```
Run the pipeline on AVGO. Earnings Jul-29 after close.
```

Date and session (AMC) supplied; the orchestrator still echoes both back for confirmation before any agent runs.

---

## 3. Full template form

```
RUN PIPELINE
Ticker:        NFLX
Earnings:      Aug-05 AMC
Owner note:    Second look — last quarter's run ended NO TRADE on thin liquidity.
Data files:    agent-data-source/nflx-options-chain.csv
```

The owner note goes into the run record only — no agent ever sees it. The declared data file is used for the Options Market Analyst instead of (or alongside) Yahoo data.

---

## 4. Casual with a note

```
Run MSFT through the pipeline, earnings Jul-24 before open. Note for the record: I'm
mainly interested in whether the reviewer flags the elevated index vol this week.
```

Maps to Ticker: MSFT, Earnings: Jul-24 BMO, Owner note: recorded verbatim.

---

## 5. Pre-supplied data, no date

```
Run the pipeline on CRWD. I've put the options chain in
agent-data-source/crwd-chain.csv — use it for the options analyst.
```

Orchestrator fetches and confirms the earnings date, uses the supplied chain file, and copies it into the run record.

---

## Invalid triggers (rejected by the orchestrator)

```
Run the pipeline on AVGO and NFLX        ← one ticker per run; submit two triggers
Run the pipeline                          ← ticker is required
Run AVGO and tell the strategist I'm bullish   ← owner views never reach agents; the
                                                 note would be recorded but never delivered
```
