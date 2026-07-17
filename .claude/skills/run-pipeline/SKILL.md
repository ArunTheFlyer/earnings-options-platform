---
name: run-pipeline
description: Run the earnings options pipeline on one ticker — five agents in sequence (regime, technical, options, strategist, peer reviewer) with structured hand-offs, Yahoo Finance data, and a local run record. Invoke when the user says "run the pipeline on <TICKER>", "/run-pipeline <TICKER>", or any request to analyze a stock's upcoming earnings through the platform.
---

# Run Pipeline

Execute one full pipeline run for one ticker. `docs/design/orchestration-design.md` is the governing document; this skill is its operational checklist.

## Step 0 — Parse the trigger

Fields: **Ticker** (required), **Earnings date + session** (AMC/BMO; fetch from Yahoo if absent), **Owner note** (record-only — never delivered to any agent), **Data files** (paths in `agent-data-source/`). One ticker per run; reject multi-ticker triggers.

**Echo the earnings date and session back to the user and wait for confirmation before proceeding.**

## Step 1 — Prepare the run record

Create `runs/<YYYY-MM-DD>-<TICKER>/` with a `data/` subfolder. Everything produced below is written there as it happens. Copy any user-supplied files from `agent-data-source/` into `data/`.

## Step 2 — Market Regime Analyst

Fetch broad market data (indices, volatility readings, trend/sentiment conditions) from Yahoo Finance; save the snapshot to `data/`. Run `agents/market-regime-analyst.md` as an isolated subagent with: the snapshot + the regime taxonomy (`docs/contracts/regime-taxonomy.md`). **No ticker information** — this agent is symbol-agnostic. Save output as `01-regime.md`.

## Step 3 — Technical Analyst

Fetch the candidate's price history (daily, ~1 year, plus recent detail) from Yahoo; save snapshot. Run `agents/technical-analyst.md` with: ticker + earnings date, the price data, and the regime structured contract. Save as `02-technical.md`.

## Step 4 — Options Market Analyst

Fetch the options chain (expirations around the event, IV, volumes, open interest, spreads) from Yahoo; save snapshot. If chain quality is inadequate, STOP and ask the user for a file in `agent-data-source/` naming exactly what is needed. Run `agents/options-market-analyst.md` with: ticker + earnings date, the options data, and the regime + technical structured contracts. Save as `03-options.md`.

## Step 5 — Earnings Options Strategist

Run `agents/earnings-options-strategist.md` with: the three analyst structured contracts + ALL approved strategy definitions (`strategies/*.md`, excluding `strategy-template.md`). Save as `04-strategist.md`.

## Step 6 — Strategy Peer Reviewer (staged delivery — mandatory)

1. Start `agents/strategy-peer-reviewer.md` as a subagent with ONLY: the three analyst contracts + the strategy definitions. Instruct it to emit its **independent reconstruction** (Thinking Process steps 1-2). Save as `05-reviewer-reconstruction.md`.
2. Only then send the Strategist's structured contract; it completes comparison and verdict. Save as `05-reviewer.md`.

## Step 7 — Present and record the user's decision

Present the verdict to the user:
- APPROVED / APPROVED WITH OBSERVATIONS → show the full strategy specification (per-unit cost, max loss) and any observations. Ask for their portfolio decision (take it? how many units?). Record verbatim as `06-owner-decision.md`.
- REJECTED / NO TRADE / INSUFFICIENT EVIDENCE → the run ends; present the reasoning; record the outcome.

Never advise on sizing; the decision is the user's (ADR-005).

## Rules (always)

- Never fabricate data; unavailable data → ask the user, naming the exact need.
- Never repair agent output; a contract violation (missing field, invalid outcome value) aborts the run, recorded.
- Narratives are never passed between agents — structured contracts only.
- Run records stay local (`runs/` is gitignored); never commit or push their contents.
