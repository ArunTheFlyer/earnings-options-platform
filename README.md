# Earnings Options Platform

A multi-agent AI platform for researching, evaluating, and managing options trades around earnings events.

## Quick Start

1. Install [Claude Code](https://claude.com/claude-code) and clone this repository.
2. Open the repo folder in Claude Code.
3. Type: `Run the pipeline on <TICKER>. Earnings <date> <AMC|BMO>.` (pick a stock with earnings ~10 days out)
4. Confirm the earnings date when echoed back; supply data files in `agent-data-source/` if asked.
5. Read the peer-reviewed recommendation — then make your own trade decision. The pipeline never touches your money.

Full walkthrough: **[docs/user-guide.md](docs/user-guide.md)** · Trigger examples: [examples/run-trigger-examples.md](examples/run-trigger-examples.md)

> **Not financial advice.** This platform produces peer-reviewed analysis artifacts. All capital decisions are yours, made manually, by design. Your run records stay local (`runs/` is gitignored).

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
│   ├── architecture.md, workflow.md, design-principles.md, glossary.md, user-guide.md
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
