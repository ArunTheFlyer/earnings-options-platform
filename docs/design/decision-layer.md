# Decision Layer — Architecture

Architectural specification for the platform's decision-making layer: the responsibilities, boundaries, and interaction model of every decision-making agent. This is an architectural specification, not an agent specification — individual decision agents will receive their own design specifications derived from this document.

## 1. Purpose

The platform separates agents into three kinds:

- **Analysts** produce evidence — observations of what is, delivered as structured artifacts under the [analyst output contract](../contracts/analyst-output-contract.md). They hold no decision authority.
- **Decision agents** make judgments — they synthesize evidence, propose, challenge, and approve. **Only decision agents possess decision authority.**
- **Execution agents** perform actions — they carry out decisions that have already been made, deciding only how, never whether.

This document governs the decision agents and the execution agent's boundary with them.

## 2. Decision Layer Components

The current planned components, and only these:

- **Earnings Options Strategist**
- **Strategy Peer Reviewer**
- **Portfolio Manager**
- **Trade Execution Manager** (execution agent; included here because its boundary with the decision agents is part of this layer's architecture)

## 3. Decision Ownership

**Earnings Options Strategist**
- Synthesizes analyst evidence across the regime, technical, and options assessments.
- Selects the single best approved strategy from the canonical `strategies/` set.
- May return exactly one of: **Strategy**, **NO TRADE**, **INSUFFICIENT EVIDENCE**.

**Strategy Peer Reviewer**
- Challenges the strategist's proposal.
- Validates the reasoning chain from evidence to recommendation.
- Identifies contradictions — within the proposal, and between the proposal and the analyst evidence.
- Never invents new evidence: it works only with the strategist's output and the evidence record.

**Portfolio Manager**
- Evaluates an approved strategy within portfolio context.
- Owns capital allocation, exposure, concentration, correlation, and portfolio-level constraints.
- Holds final authority over capital: only it can commit the portfolio to a trade.

**Trade Execution Manager**
- Converts an approved, sized strategy into executable actions.
- Validates execution constraints (orders, fills, lifecycle rules).
- Performs no strategy selection and never revisits whether the trade should exist.

## 4. Decision Flow

The canonical flow:

```
Analyst Outputs (structured contracts)
        ↓
Earnings Options Strategist
        ↓
Strategy Peer Reviewer
        ↓
Portfolio Manager
        ↓
Trade Execution Manager
```

**Evidence always flows upward from analysts. Decisions always flow downward.** No decision agent produces evidence; no analyst makes a decision. A rejection at any stage (peer-review rejection, portfolio decline) ends the flow for that candidate — there is no route around a decision.

## 5. Decision Authority

| Agent | Authority | Cannot Decide |
|---|---|---|
| Earnings Options Strategist | What to propose: one approved strategy, NO TRADE, or INSUFFICIENT EVIDENCE | Whether its proposal is approved; position size; whether capital is committed; how execution happens |
| Strategy Peer Reviewer | Whether a proposal advances: approve, or reject/return with objections | What to propose; it cannot originate, modify, or substitute a strategy, or commit capital |
| Portfolio Manager | Whether an approved strategy receives capital, at what size, under what portfolio constraints | The strategy's content or structure; it accepts or declines, never redesigns |
| Trade Execution Manager | How an accepted trade is placed and managed through its lifecycle | Whether the trade should exist; strategy selection; sizing beyond the approved constraints |

## 6. Consumer Rules

Per the [analyst handoff contract](../contracts/analyst-handoff-contract.md):

- Decision agents consume **Structured Output Contracts only**.
- Narrative explanations are **never** decision inputs.
- Decision agents may generate narratives explaining their decisions — for human inspection, peer review, and the written record — but those narratives never become inputs to downstream automation. Each decision agent's actionable output to the next stage is its own structured artifact.

## 7. Architectural Constraints

- Analysts never become decision-makers.
- Decision agents never rewrite analyst evidence — evidence is cited as received, per the handoff contract's precedence principles.
- Execution agents never reinterpret decisions.
- Peer reviewers challenge reasoning, not evidence — the evidence record is the analysts' domain; the reviewer tests what the strategist concluded from it.
- Missing evidence is surfaced, never fabricated — gaps route to explicit outcomes (e.g., INSUFFICIENT EVIDENCE), never to inference.

## 8. Open Architectural Questions

1. **Peer Reviewer input scope** — Should the Strategy Peer Reviewer consume only the Strategist's output, or both the Strategist's output and all analyst structured contracts independently? (Context, not an answer: workflow.md currently provides the reviewer "the strategist's proposal plus all upstream assessments"; this question must be settled before the Peer Reviewer's design specification is written, and workflow.md aligned to the ruling.)
