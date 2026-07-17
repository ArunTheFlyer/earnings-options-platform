# Earnings Options Platform — Session Instructions

This repository is a multi-agent pipeline for analyzing options trades around earnings events. When the user asks to **run the pipeline** (any phrasing naming a ticker — see `examples/run-trigger-examples.md`), you are the **orchestrator**. Follow `docs/design/orchestration-design.md` exactly. The key rules:

## Orchestration (summary — the design doc governs)

1. **Confirm the earnings date and session (AMC/BMO) with the user before any agent runs** — supplied or fetched (Yahoo Finance). One ticker per run.
2. Run each agent as an **isolated subagent** (fresh context), in order: Market Regime Analyst → Technical Analyst → Options Market Analyst → Earnings Options Strategist → Strategy Peer Reviewer. Each receives ONLY: its prompt (`agents/<name>.md`), its declared inputs, and its reference artifacts.
3. **Deliver reference artifacts:** the regime taxonomy (`docs/contracts/regime-taxonomy.md`) to the Market Regime Analyst; ALL strategy definitions (`strategies/*.md`, excluding the template) to the Strategist and the Peer Reviewer.
4. **Data:** fetch from Yahoo Finance / free public sources. If required data is unavailable or inadequate, STOP and ask the user for specific files in `agent-data-source/`. Never fabricate data. Save all data snapshots used into the run record.
5. **Reviewer independence is structural:** give the Peer Reviewer subagent the analyst contracts + strategy definitions FIRST and obtain its independent reconstruction; only then deliver the Strategist's artifact for comparison.
6. **Run records:** write every artifact to `runs/<YYYY-MM-DD>-<TICKER>/` (data snapshots, one file per structured artifact, reviewer reconstruction, and the user's manual portfolio decision when they state it). This folder is gitignored — never push run contents.
7. The **user is the Portfolio Manager** (ADR-005): after the review verdict, they decide whether to trade and how many units. Record their decision; never decide for them. The user's "Owner note" in a trigger is record-only — never show it to any agent.
8. A REJECTED verdict or non-trade outcome ends the run; record it. Never repair an agent's output — a contract violation aborts the run, recorded.

Alternatively, the user may invoke the packaged skill: `/analyze-earnings-trade <TICKER> [earnings date]` — same procedure, same rules.

## Repo conventions

- Agent prompts (`agents/`), strategy definitions (`strategies/`), contracts (`docs/contracts/`) and design specs (`docs/design/`) are governed, reviewed artifacts — do not edit them casually; changes re-trigger review per `docs/review/review-plan.md`.
- The platform produces analysis, never financial advice; it never places trades or touches accounts.
