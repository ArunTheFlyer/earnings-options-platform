# Orchestration Design (Phase 4)

How a pipeline run actually executes for the MVP. Derived from the workflow, the contracts, and the owner's Phase 4 rulings (2026-07-17). Where this document is silent, the agent specifications and contracts govern.

## 1. Run Model

**The orchestrator is a Claude Code session** operated by the owner. A run is triggered by the owner naming a candidate ("run the pipeline on XYZ"). The session then executes each agent as an isolated subagent, in workflow order, enforcing the hand-off rules below. No standing scheduler exists in the MVP; runs are on-demand.

The owner performs the portfolio stage manually (ADR-005) between the Peer Reviewer's verdict and any execution. The Trade Execution Manager agent is available per trade at the owner's choice; execution may also be fully manual.

## 2. Agent Execution

- Each agent runs as its **own subagent with a fresh context**, receiving exactly: its prompt (`agents/<agent>.md`), its declared inputs, and its reference artifacts. Nothing else — the orchestrator's own conversation never leaks in.
- Agents run in workflow order: Market Regime Analyst → Technical Analyst → Options Market Analyst → Earnings Options Strategist → Strategy Peer Reviewer.
- **Reviewer independence is structural (resolves OAQ-2):** the Peer Reviewer subagent receives the analyst structured contracts and the strategy definitions FIRST and must emit its independent reconstruction; only then is the Strategist's artifact delivered (follow-up message), and comparison begins. The reconstruction is part of the run record.

## 3. Data Sourcing (owner ruling)

- **Primary: Yahoo Finance** (and comparable free public sources) fetched by the orchestrator at run time — index levels and volatility readings for the regime analyst; price history for the technical analyst; options chain, IV, and expiries for the options analyst.
- **Fallback: `agent-data-source/`** — when required data is unavailable or of insufficient quality, the orchestrator STOPS and asks the owner for the specific items, naming exactly what is needed and for which analyst. The owner supplies files in `agent-data-source/`; the orchestrator resumes and cites those files in the run record.
- The orchestrator never fabricates data and never lets an analyst run with silently missing inputs — an analyst that receives thin data will reflect it per its own contract (Assumptions, Confidence), but *known* gaps are routed to the owner first.
- Data snapshots used in a run are saved into the run record (section 5) so every assessment is reconstructible.

## 4. Reference-Artifact Delivery Registry (per OAQ-2 / finding A4)

The orchestrator is responsible for delivering into each agent's context:

| Agent | Reference artifacts delivered |
|---|---|
| Market Regime Analyst | The approved regime taxonomy (see section 7) |
| Technical Analyst | — |
| Options Market Analyst | — |
| Earnings Options Strategist | All approved strategy definitions in `strategies/` (currently: pmcc-calendar-overlay), pinned at the run's commit |
| Strategy Peer Reviewer | Same `strategies/` definitions, same pin |

## 5. Run Records

Every run writes its artifacts to **`runs/<YYYY-MM-DD>-<TICKER>/`**:

- `data/` — the market-data snapshots used (and any owner-supplied files copied from `agent-data-source/`).
- One file per structured artifact: `01-regime.md`, `02-technical.md`, `03-options.md`, `04-strategist.md`, `05-reviewer-reconstruction.md`, `05-reviewer.md` (narratives included beneath each structured contract, clearly separated).
- `06-owner-decision.md` — the owner's manual portfolio decision (accept/decline, units), recorded by the orchestrator after the owner states it. Keeps the written record whole despite the human stage.
- Run records are committed to the repository; the written record is the point of the platform.

## 6. Failure Handling

- An analyst or decision-agent contract violation observed by the orchestrator (missing field, wrong outcome value) aborts the run with the defect recorded — the orchestrator does not repair agent output.
- A REJECTED review or non-trade outcome ends the run per the workflow; the record stands.
- Data unavailability follows section 3 (ask the owner; no fabrication).

## 7. Pre-Run Requirements (open)

1. **Regime taxonomy** — the Market Regime Analyst prompt requires "the project's approved taxonomy... provided to you," and no taxonomy artifact exists yet. An initial owner-approved taxonomy is required before the first run. (Tracked as the MVP's last content gap.)
2. Yahoo-sourced IV granularity (IV Rank, term structure) may be approximate; the entry criteria's data-contract guards ("where the data contract provides it") tolerate this, with quality gaps surfacing in the options analyst's Assumptions/Confidence per contract.
