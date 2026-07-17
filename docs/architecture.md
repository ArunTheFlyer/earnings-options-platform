# Architecture

This document explains why the platform is structured as a pipeline of specialized agents. It deliberately avoids implementation detail; for the step-by-step process see [workflow.md](workflow.md).

## Vision

A trading decision around an earnings event should be the product of a disciplined process, not a single act of judgment. The vision of this platform is to encode that process as a set of cooperating AI agents, so that every recommendation is the visible output of a chain of evidence, challenge, and deliberate decision — and so the process itself can be inspected, criticized, and improved over time.

## System Philosophy

A single large prompt asked to "analyze this earnings trade" mixes many jobs: reading the market, reading the chart, reading the options board, inventing a structure, judging the risk, and sizing the position. When those jobs are mixed, failures are impossible to localize and improvements to one job degrade another.

This platform takes the opposite approach: **many small agents, each doing one job well, connected by explicit hand-offs.** The pipeline is intentionally sequential — later stages depend on earlier evidence, and the ordering itself encodes the discipline (understand the environment before the chart, the chart before the options, the evidence before the strategy, the review before the money).

## Agent Responsibilities

Agents fall into three tiers:

**Analysts** — produce evidence, not opinions about trades.
- *Market Regime Analyst*: identifies and explains the current market regime; symbol-agnostic.
- *Technical Analyst*: interprets price action, trend, and levels for the candidate symbol.
- *Options Market Analyst*: assesses how the options are priced — implied volatility, expected move, and the cost of expressing a view.

Analyst outputs are **observational**: statements of what is, with facts separated from interpretation, and never instructions to any other agent. Responsibility for interpreting those observations and acting on them rests entirely with the downstream decision-making agents that consume them.

**Strategist and Reviewer** — turn evidence into a challenged proposal.
- *Earnings Options Strategist*: designs a trade structure that fits the assembled evidence, or concludes that no trade is warranted.
- *Strategy Peer Reviewer*: independently challenges the proposal — its assumptions, its risk, its fit with the evidence — before it may advance.

**Managers** — decide and act.
- *Portfolio Manager*: judges whether an approved strategy fits the portfolio's risk budget, concentration, and objectives.
- *Trade Execution Manager*: places and manages accepted trades through their lifecycle.

## Separation of Concerns

Three separations are load-bearing:

1. **Analysis vs. decision-making.** Analysts describe what is; they never say what to do. This keeps evidence untainted by the desire for a trade.
2. **Proposal vs. approval.** The agent that designs a strategy never approves it. The reviewer and the portfolio manager hold that power, which builds adversarial checking into the process.
3. **Decision vs. execution.** Deciding that a trade belongs in the portfolio is distinct from placing and managing it. Execution concerns (orders, fills, lifecycle) never leak upstream into strategy design.

## Decision Hierarchy

Authority increases as work flows downstream:

- Analysts have **no decision authority** — their output is evidence.
- The Strategist decides only **what to propose**, including proposing nothing.
- The Peer Reviewer can **block or return** a proposal, but cannot originate one.
- The Portfolio Manager holds **final authority over capital** — only it can commit the portfolio to a trade.
- The Execution Manager decides only **how** to carry out what has already been approved, never whether.

A rejection at any stage ends the pipeline for that candidate. There is no route around the hierarchy.

## Information Flow

Information flows strictly forward. Each stage receives the accumulated outputs of all prior stages and adds its own contribution. Nothing is passed implicitly: if a downstream agent needs a fact, an upstream agent must have stated it. This makes every hand-off a reviewable artifact and every final decision reconstructible from the written record.

The one deliberate exception to forward flow is rejection: the Peer Reviewer and Portfolio Manager may send a proposal back or end it, which is itself a recorded outcome, not a silent loop.

## Design Goals

- **Explainability** — any recommendation can be traced back through the pipeline to the evidence that produced it.
- **Auditability** — every stage leaves a written artifact; the full decision record can be reviewed after the fact.
- **Localized failure** — when the system errs, the erring stage can be identified and improved without rewriting everything.
- **Reusability** — analysts are strategy-agnostic and can serve future pipelines beyond earnings trades.
- **Restraint by default** — the pipeline filters aggressively; "no trade" is a first-class, expected outcome.

## Guiding Principles

The engineering principles behind these choices — single responsibility, evidence before opinion, peer review before execution, and others — are elaborated in [design-principles.md](design-principles.md). Terminology used throughout the project is defined in [glossary.md](glossary.md).
