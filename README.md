# Earnings Options Platform

A multi-agent AI platform for researching, evaluating, and managing options trades around earnings events.

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
Portfolio Manager          — does it fit the portfolio and risk budget?
   ↓
Trade Execution Manager    — how is the approved trade placed and managed?
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
| [Portfolio Manager](agents/portfolio-manager.md) | Decides whether an approved strategy fits portfolio-level risk and allocation. |
| [Trade Execution Manager](agents/trade-execution-manager.md) | Handles order placement and lifecycle management of accepted trades. |

## Repository Structure

```
.
├── README.md
├── agents/       # Agent prompt definitions (one file per agent)
├── docs/         # Architecture, workflow, principles, glossary
│   └── review/   # Review methodology, status dashboard, review log, ADRs
├── strategies/   # Strategy documents (future)
└── examples/     # Worked examples and sample runs (future)
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

> Placeholder — to be defined as the platform matures.

- [ ] Author the seven agent prompt definitions
- [ ] Define the strategy document format and initial strategies
- [ ] Add worked examples of full pipeline runs
- [ ] Evaluate and iterate on agent outputs against real earnings events

## Future Expansion

Areas under consideration, not yet committed:

- Additional analyst agents (e.g., fundamentals or news/sentiment analysis)
- Strategy library growth beyond earnings-centric structures
- Automation of the pipeline hand-offs between agents
- Post-trade review and feedback loops into agent prompts
