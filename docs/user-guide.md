# User Guide — Running the Earnings Options Pipeline

Step-by-step instructions for running this platform. No coding required: the pipeline runs inside a Claude Code session, and every artifact is a markdown file.

> **Not financial advice.** This platform produces peer-reviewed *analysis artifacts*. Every capital decision — whether to trade, and at what size — is yours, made manually, by design (ADR-005).

---

## 1. What you need

- **Claude Code** (CLI, desktop app, or VS Code extension) with an active subscription.
- This repository cloned locally:
  ```
  git clone https://github.com/<owner>/earnings-options-platform.git
  ```
- A stock you're interested in, with **earnings roughly ten calendar days out** (that's the pipeline's entry window).

## 2. What a run does

One run takes one ticker through five AI agents, in order:

1. **Market Regime Analyst** — classifies the current market environment (from a fixed taxonomy).
2. **Technical Analyst** — reads the stock's trend, key levels, and price structure into earnings.
3. **Options Market Analyst** — reads the options market: expected move, IV, liquidity.
4. **Earnings Options Strategist** — recommends ONE approved strategy, or NO TRADE, or INSUFFICIENT EVIDENCE.
5. **Strategy Peer Reviewer** — independently challenges the recommendation before you ever see it as actionable: APPROVED / APPROVED WITH OBSERVATIONS / REJECTED.

Then **you** decide: trade or not, and how big. The pipeline never touches your money.

## 3. Start a run

Open the repository folder in Claude Code and type a run trigger:

```
Run the pipeline on AVGO. Earnings Jul-29 after close.
```

Ticker is required; everything else is optional (see `examples/run-trigger-examples.md` for all forms). If you omit the earnings date, the orchestrator fetches it.

**The orchestrator always echoes the earnings date and session (before/after market) back to you and waits for your confirmation** — check it; a wrong date corrupts the whole run.

## 4. During the run

- Market data is fetched from free public sources (Yahoo Finance) automatically.
- If some data isn't available or good enough, the run **pauses and tells you exactly what's missing**. Drop the requested file into `agent-data-source/` and say so; the run resumes. Nothing is ever guessed.
- The agents hand structured artifacts down the chain; the reviewer independently re-derives the reasoning before it's allowed to see the strategist's answer.

## 5. Read the result

The run ends with the Peer Reviewer's verdict:

- **APPROVED / APPROVED WITH OBSERVATIONS** — a fully specified trade (strategy, strikes, expirations, per-unit cost and max loss, entry conditions, exit plan) that survived adversarial review. Observations are concerns worth your attention that weren't severe enough to reject.
- **REJECTED** — the recommendation didn't survive scrutiny; the defect categories tell you why.
- **NO TRADE / INSUFFICIENT EVIDENCE** — the strategist declined to recommend; the reviewer audited that reasoning too. This is the system working, not failing.

## 6. Your decision (the human step)

If a trade was approved: decide **whether** to take it and **how many units**, against your own account and risk tolerance. The strategist's artifact gives you the per-unit cost and maximum loss — the arithmetic is `units × max loss per unit` against whatever you're willing to risk. Tell the orchestrator your decision; it's recorded in the run record.

Execution (placing orders with your broker) is also yours — manually, or with the Trade Execution Manager agent's order plan as a checklist. If you take the trade, the strategy's exit plan is the plan; follow it.

## 7. Your run records (private)

Every run writes its full record to `runs/<date>-<ticker>/` — data snapshots, every agent's artifact, the reviewer's verdict, and your decision. **This folder is gitignored: your trading record stays on your machine** and is never pushed to GitHub.

## 8. Customizing (advanced)

- **Strategies:** the pipeline only recommends strategies defined in `strategies/` (max five, defined-risk only). Add your own from `strategies/strategy-template.md` — but note the governance rule: a strategy definition should be reviewed before you trust runs against it.
- **Regime taxonomy:** the market classifications live in `docs/contracts/regime-taxonomy.md`.
- **How it all works:** `docs/architecture.md` (why), `docs/workflow.md` (the pipeline), `docs/design/` (each agent's specification), `docs/review/` (how every piece was reviewed and decided).

## 9. Troubleshooting

| Symptom | Meaning |
|---|---|
| Run ends with INSUFFICIENT EVIDENCE | Upstream data couldn't support a conclusion — check the Missing Evidence field; supply better data and rerun if you can. |
| Run pauses asking for data | Yahoo couldn't provide something; the message names the exact file to drop in `agent-data-source/`. |
| Reviewer REJECTED a proposal | Working as designed — read the defect categories before considering a rerun. |
| Everything is NO TRADE | Also working as designed. The pipeline is a filter; most earnings events don't deserve a trade. |
