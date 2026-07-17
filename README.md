# Earnings Options Platform

A multi-agent AI platform for researching, evaluating, and managing options trades around earnings events.

## Quick Start

1. Install [Claude Code](https://claude.com/claude-code) and clone this repository.
2. Open the repo folder in Claude Code.
3. Start a run — natural language or the packaged skill, your choice:
   - `Run the pipeline on <TICKER>. Earnings <date> <AMC|BMO>.`
   - `/run-pipeline <TICKER>`

   **Required:** the ticker only (one per run; pick a stock with earnings ~10 days out). **Optional:** the earnings date and session — AMC = after market close, BMO = before market open (fetched for you if omitted), a private note for your records, and pre-supplied data files. All forms: [examples/run-trigger-examples.md](examples/run-trigger-examples.md).
4. Confirm the earnings date when echoed back; supply data files in `agent-data-source/` if asked.
5. The run ends with a verdict: a recommended trade that survived independent AI peer review (with strikes, expirations, cost, and exit plan) — or a reasoned "no trade."

Trigger examples: [examples/run-trigger-examples.md](examples/run-trigger-examples.md)

> **Not financial advice.** This platform produces peer-reviewed analysis artifacts. All capital decisions are yours, made manually, by design. Your run records stay local (`runs/` is gitignored).

## Getting Updates

This platform ships new versions (new agent capabilities, strategy fixes, doc improvements) directly to this GitHub repository. **Updates are not automatic** — you pull them yourself, whenever you want the latest version:

```
cd earnings-options-platform
git pull
```

That's it — no reinstall, no separate updater. Your private files (`runs/`, `agent-data-source/`, any personal `policy/` draft) are gitignored and untouched by an update; only the platform's own files change.

**Check what's new:** [ROADMAP.md](ROADMAP.md) tracks current status and milestones; `git log --oneline -10` shows the most recent changes directly.

## Cost — What You Actually Pay

**This platform is free for early adopters.** There is no license fee, no subscription, and no hidden charge today. This is an early-access period — the project may introduce pricing for the platform itself in the future; early adopters get free access now, ahead of any future change. Nothing about this repository requires payment to use it as-is.

Separately, regardless of pricing changes here, you always need **your own Claude Code usage**, because each run has you invoking Claude Code to execute five agents:

- You need an **active Claude Code subscription/usage plan** (see [claude.com/claude-code](https://claude.com/claude-code) for current plans and pricing) — this is a cost you already have or would pay Anthropic directly for using Claude Code at all, not a fee to this platform.
- Each pipeline run consumes your own usage/tokens under that plan, the same as any other Claude Code session you run.
- **Optional add-ons cost extra, only if you choose them.** Nothing in the platform requires them — see "Built-In vs. Add-On Capabilities" below.

No data fees (Yahoo Finance is free), no broker integration fee, no per-run charge from this project. Your money only ever leaves your account when *you* place a trade at your own broker.

## What a Run Does

One run takes one ticker through five AI agents, in order:

1. **Market Regime Analyst** — classifies the current market environment (from a fixed taxonomy).
2. **Technical Analyst** — reads the stock's trend, key levels, and price structure into earnings.
3. **Options Market Analyst** — reads the options market: expected move, IV, liquidity.
4. **Earnings Options Strategist** — recommends ONE approved strategy, or NO TRADE, or INSUFFICIENT EVIDENCE.
5. **Strategy Peer Reviewer** — independently challenges the recommendation before you ever see it as actionable: APPROVED / APPROVED WITH OBSERVATIONS / REJECTED.

Then **you** decide: trade or not, and how big. The pipeline never touches your money.

**Starting a run:** two equivalent options — natural language (Claude Code auto-loads `CLAUDE.md`, which teaches the full orchestration procedure) or the packaged skill (`/run-pipeline <TICKER>`); both follow the same governed procedure (`docs/design/orchestration-design.md`). **The orchestrator always echoes the earnings date and session back to you and waits for confirmation** before running anything — check it; a wrong date corrupts the whole run.

**During the run:** market data is fetched automatically from free public sources (Yahoo Finance). If something isn't available or good enough, the run **pauses and tells you exactly what's missing** — drop the requested file into `agent-data-source/` and say so; the run resumes. Nothing is ever guessed. The agents hand structured artifacts down the chain; the reviewer independently re-derives the reasoning before it's allowed to see the strategist's answer.

**Reading the result:**

- **APPROVED / APPROVED WITH OBSERVATIONS** — a fully specified trade (strategy, strikes, expirations, per-unit cost and max loss, entry conditions, exit plan) that survived adversarial review. Observations are concerns worth your attention that weren't severe enough to reject.
- **REJECTED** — the recommendation didn't survive scrutiny; the defect categories tell you why.
- **NO TRADE / INSUFFICIENT EVIDENCE** — the strategist declined to recommend; the reviewer audited that reasoning too. This is the system working, not failing.

## Your Decision (the Human Step)

If a trade was approved: decide **whether** to take it and **how many units**, against your own account and risk tolerance. The strategist's artifact gives you the per-unit cost and maximum loss — the arithmetic is `units × max loss per unit` against whatever you're willing to risk. Tell the orchestrator your decision; it's recorded in the run record.

Execution (placing orders with your broker) is also yours — manually, or with the Trade Execution Manager agent's order plan as a checklist. If you take the trade, the strategy's exit plan is the plan; follow it.

Every run writes its full record to `runs/<date>-<ticker>/` — data snapshots, every agent's artifact, the reviewer's verdict, and your decision. **This folder is gitignored: your trading record stays on your machine** and is never pushed to GitHub.

## Customizing (Advanced)

- **Strategies:** the pipeline only recommends strategies defined in `strategies/` (max five, defined-risk only). Add your own from `strategies/strategy-template.md` — but note the governance rule: a strategy definition should be reviewed before you trust runs against it.
- **Regime taxonomy:** the market classifications live in `docs/contracts/regime-taxonomy.md`.
- **How it all works:** [docs/architecture.md](docs/architecture.md) (why), [docs/workflow.md](docs/workflow.md) (the pipeline), `docs/design/` (each agent's specification), `docs/review/` (how every piece was reviewed and decided).

## Troubleshooting

| Symptom | Meaning |
|---|---|
| Run ends with INSUFFICIENT EVIDENCE | Upstream data couldn't support a conclusion — check the Missing Evidence field; supply better data and rerun if you can. |
| Run pauses asking for data | Yahoo couldn't provide something; the message names the exact file to drop in `agent-data-source/`. |
| Reviewer REJECTED a proposal | Working as designed — read the defect categories before considering a rerun. |
| Everything is NO TRADE | Also working as designed. The pipeline is a filter; most earnings events don't deserve a trade. |

## Built-In vs. Add-On Capabilities

Everything this platform needs ships in this repository and runs on Claude Code's **built-in capabilities** — web fetch/search for market data (Yahoo Finance), file access, and the packaged `/run-pipeline` skill. Clone and run; no keys, no installs.

**Optional add-ons are controlled by your environment, not this repo.** You may equip your own Claude Code session with additional tools — e.g., an MCP server such as Tavily (better search) or a market-data server (structured options chains). These are yours to install, configure, and pay for (API keys live in your environment, never in this repository). The pipeline works without them; when present, the orchestrator can use them to build higher-quality context for the agents. The agents themselves never use tools — they only read what the orchestrator delivers.

## Purpose

Trading options around earnings requires synthesizing several distinct kinds of analysis — market conditions, price action, options pricing, strategy design, risk review, and portfolio fit. Doing all of this in a single prompt produces opaque, unrepeatable decisions.


## High-Level Architecture

The platform is a sequential analysis pipeline. Analysts gather and interpret evidence; the strategist proposes; a reviewer challenges; managers decide and execute.

```
Watchlist
   ↓
Market Regime Analyst      — what environment are we trading in?
   ↓
Technical Analyst          — what is the price action telling us?
   ↓
Options Market Analyst     — how are the options priced?
   ↓
Earnings Options Strategist — what trade structure fits the evidence?
   ↓
Strategy Peer Reviewer     — does the proposal survive scrutiny?
   ↓
Portfolio Manager (YOU)    — does it fit the portfolio? (manual decision; agent is a release candidate)
   ↓
Trade Execution Manager    — how is the approved trade placed and managed? (optional per trade)
```

Each stage consumes the outputs of the stages before it. No stage skips ahead, and no stage revisits a decision that belongs to another stage. See [docs/architecture.md](docs/architecture.md) and [docs/workflow.md](docs/workflow.md) for details.

## Agent Overview

| Agent | Role |
|---|---|
| [Market Regime Analyst](agents/market-regime-analyst.md) | Characterizes the broad market environment and its implications for risk-taking. |
| [Technical Analyst](agents/technical-analyst.md) | Analyzes price action, levels, and trend for candidate symbols. |
| [Options Market Analyst](agents/options-market-analyst.md) | Assesses options pricing, implied volatility, and the expected move around earnings. |
| [Earnings Options Strategist](agents/earnings-options-strategist.md) | Designs a trade structure (or recommends no trade) based on the analysts' evidence. |
| [Strategy Peer Reviewer](agents/strategy-peer-reviewer.md) | Independently challenges the strategist's proposal before it can advance. |
| Portfolio Manager | **Feature release candidate** — currently performed manually by the user (ADR-005). The reviewed design spec ([docs/design/portfolio-manager-design.md](docs/design/portfolio-manager-design.md)) is the blueprint for a future release. |
| [Trade Execution Manager](agents/trade-execution-manager.md) | Handles order placement and lifecycle management of accepted trades. |

## Repository Structure

```
.
├── README.md
├── ROADMAP.md            # Phase/milestone status
├── agents/               # Agent prompt definitions (one file per agent)
├── docs/
│   ├── architecture.md, workflow.md, design-principles.md, glossary.md
│   ├── contracts/        # Output/handoff/decision contracts, regime taxonomy
│   ├── design/           # Agent design specifications + orchestration design
│   └── review/           # Review methodology, dashboard, log, ADRs
├── strategies/           # Approved strategy definitions + template (max 5, defined-risk)
├── policy/               # Portfolio policy template (PM agent release candidate)
├── examples/             # Run trigger examples
├── agent-data-source/    # Your data files when asked (gitignored)
└── runs/                 # Your run records (gitignored — local only)
```

## Governance

All architecture and agent reviews follow the formal process in [docs/review/review-plan.md](docs/review/review-plan.md), with progress tracked in the [review status dashboard](docs/review/review-status.md), sessions journaled in the [review log](docs/review/review-log.md), and significant decisions recorded as [Architecture Decision Records](docs/review/architecture-decisions.md).

> **Repository Rule:** No agent prompt may be considered complete until it has successfully passed the review checklist. Likewise, documentation is not complete until it has been reviewed against the architecture.

## Philosophy

- **Analysis is separated from decision-making.** Analysts report evidence; they do not recommend trades. Decisions are made downstream, by agents whose job is to decide.
- **Evidence before opinion.** Every recommendation must trace back to observable inputs produced earlier in the pipeline.
- **Peer review before execution.** No strategy reaches the portfolio without surviving an independent challenge.
- **"No trade" is a valid output.** The pipeline is designed to filter, not to force a trade out of every earnings event.

The full set of principles lives in [docs/design-principles.md](docs/design-principles.md).

## Development Roadmap

See [ROADMAP.md](ROADMAP.md) for the phased project roadmap. Current state: Phases 0-1 and 3 complete; Phase 2 (documentation review) deferred post-MVP; Phase 4 (orchestration) in progress toward the first live run.

## Future Expansion

**Feature release candidates** (designed and/or planned, not in the current release):

- Portfolio Manager agent — automated portfolio-fit decisions and sizing (passed design spec exists; currently a manual user step per ADR-005)
- Earnings History Analyst — historical earnings reactions, realized-vs-implied volatility, post-earnings IV crush (OAQ-1)
- Portfolio policy artifact (`policy/`) — versioned risk-budget and sizing rules feeding the PM agent

Areas under consideration, not yet committed:

- Additional analyst agents (e.g., fundamentals or news/sentiment analysis)
- Strategy library growth beyond earnings-centric structures
- Automation of the pipeline hand-offs between agents
- Post-trade review and feedback loops into agent prompts
