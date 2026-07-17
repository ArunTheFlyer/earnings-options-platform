# Project Roadmap

**Current Phase:** Phase 4 — Orchestration

**Current Target:** orchestration design (how a pipeline run actually executes)

**Next Milestone:** first live end-to-end pipeline run (MVP)

---

## Phase 0 — Repository Foundation
Status: ✅ Complete

### Objectives
- [x] Repository structure established
- [x] Documentation structure created
- [x] Governance process established
- [x] Review process established
- [x] Architecture decision process established

---

## Phase 1 — Agent Design Review
Status: ✅ Complete (2026-07-17; Portfolio Manager descoped per ADR-005)

### Milestone: Analyst Design Framework
Status: ✅ Complete

- [x] Platform-wide analyst principles documented (evidence-only, Facts/Interpretation separation, no recommendations)
- [x] Analyst/consumer contract established in architecture and workflow
- [x] Canonical analyst design template created (docs/design/analyst-template.md)

### Core Decision Agents
- [x] Earnings Options Strategist
- [x] Strategy Peer Reviewer

### Supporting Analysts
- [x] Market Regime Analyst
- [x] Technical Analyst
- [x] Options Market Analyst

### Decision Layer
- [x] Portfolio Manager — descoped (ADR-005): human-executed for now; passed spec dormant
- [x] Trade Execution Manager

---

## Phase 2 — Documentation Review
Status: ⏭ Deferred post-MVP (owner decision 2026-07-17) — non-blocking; foundation docs remain unreviewed engineering record

- [ ] Architecture
- [ ] Workflow
- [ ] Design Principles
- [ ] Glossary
- [ ] README

---

## Phase 3 — Strategy Framework

Status: ✅ Complete (2026-07-17)

- [x] Strategy definition template (strategies/strategy-template.md)
- [x] First strategy approved: pmcc-calendar-overlay (2026-07-17)
- [x] Owner decision: single-strategy universe for now (4 slots are headroom, not backlog)
- [x] Portfolio policy (policy/) — not required: PM descoped per ADR-005; policy is the owner's manual judgment

---

## Phase 4 — Orchestration

Status: 🚧 In Progress (started 2026-07-17)

- [ ] Orchestration design (run model, data sourcing, artifact hand-offs, reference-artifact delivery registry per OAQ-2)
- [ ] First live end-to-end pipeline run (MVP)

---

## Phase 5 — Implementation

Status: Not Started
