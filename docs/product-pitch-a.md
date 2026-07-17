# Product Pitch A — Segments and Go-to-Market Plan

Working plan for who this platform serves and what each segment needs from it. This is a product/GTM document, not an architecture document — it does not gate agent reviews and is not part of the Repository Rule.

## Context

The platform (five reviewed AI agents + a governed strategy/policy layer) currently ships as a self-contained repository run through Claude Code. This document identifies the distinct user segments discovered while pitching it, what each already gets from the current build, and what's missing for each.

## Segments

Each segment is documented in the same four fields: **Wants** (what they need from the platform), **Served today** (what current state gives them), **Gap** (what's missing), **Notes** (scope boundary, open questions, or supporting detail).

### 1. Operators / Distributors

Power users who run the pipeline themselves (via Claude Code) and either trade on the output personally or forward the reviewed recommendation to a downstream audience who never touches the tool.

- **Wants:** run the pipeline, get a peer-reviewed recommendation, share or act on it.
- **Served today:** fully. README quick start, user guide, both trigger forms (natural language + `/run-pipeline` skill), orchestration design, `pmcc-calendar-overlay` as the default strategy.
- **Gap:** none. This segment is the platform's current shape.
- **Notes:** how Segment 1 shares or publishes recommendations to their own downstream audience is assumed to be their own independent publishing model (WhatsApp, newsletter, website, etc.) — **explicitly out of scope for this platform.** The platform's responsibility ends at producing the reviewed recommendation artifact; distribution to Segment 1's own audience is theirs to build and operate.

### 2. Consumers

People who want an actionable trade idea with zero interest in setup, prompts, repos, or Claude Code.

- **Wants:** the trade idea, delivered, with no tooling of their own.
- **Served today:** exclusively through Segment-1 operators' own independent publishing (see Segment 1, Notes).
- **Gap:** none, by decision. **The platform will not build a direct delivery channel to Segment 2.** This is a deliberate scope boundary, not a backlog item — Segment 2 remains served solely as a byproduct of Segment 1's independent distribution.
- **Notes:** if this decision is ever revisited, it would require new platform-owned scope (scheduling, a distribution surface) separate from the current Phase 4 orchestration, which serves only Segment 1's on-demand runs.

### 3. Strategy Authors

Traders with their own edge — a structure they trust or have backtested — who want the pipeline's discipline (evidence gathering, independent peer review, audit trail) applied to *their* strategy instead of the shipped default.

- **Wants:** plug in a custom strategy definition and get the same rigor `pmcc-calendar-overlay` received.
- **Served today:** substantially, structurally. `strategies/strategy-template.md`, the governance rule that a strategy should be reviewed before being trusted, and the five-strategy cap all exist. Because `strategies/` is a versioned, swappable content layer, no code or agent change is required — a new strategy file is immediately evaluated by the existing Strategist and Peer Reviewer with the same contracts.
- **Gap:** none, by decision. **Segment 3 is served by the author (platform owner) as an additional, out-of-platform service** — not by self-serve tooling in the repository. Effort and cost are scoped per strategy: the author takes a user's drafted strategy through the same review discipline applied to `pmcc-calendar-overlay` (defined-risk verification, structured-evidence-only entry criteria, snapshot-testable exit plan, deterministic ADJUST rulebook, disqualifiers) and delivers an approved, review-passed strategy definition.
- **Notes:** this is a service the author performs, not a repository capability — no self-serve reviewer skill or checklist is being built for this segment. Scope and pricing (if any) are per strategy, decided outside this document.

**Private-by-default rule:** every new strategy created through this service is **private by default** — it lives in the requesting user's own copy/fork and is never merged into the shared public repository automatically. Whether a strategy is ever made public is a separate decision between the author and the requester, made either at onboarding (when the strategy is first created) or at any later point — never assumed, never automatic. Making a strategy public means merging it into the shared `strategies/` directory, subject to the platform-wide five-strategy cap (decision-layer.md §3), which may require retiring an existing strategy to make room.

## Segment Summary Table

| Segment | Wants | Status | Gap |
|---|---|---|---|
| 1. Operators / Distributors | Run + share default output | ✅ Fully served | None |
| 2. Consumers | Actionable trades, no setup | ✅ Served exclusively via Segment 1's independent publishing | None — out of platform scope by decision |
| 3. Strategy Authors | Plug in their own edge | ✅ Served — as an author-delivered service, scoped per strategy | None — served outside the repository, not by self-serve tooling |

## Candidate Next Actions (not yet committed)

1. Segment 3: no repository action — served as an author-delivered service, scoped and priced per strategy, outside this document.
2. Segment 2: no action — decided as out of platform scope; served exclusively via Segment 1's independent publishing.

## Status

Draft — first pass, capturing the segmentation as discussed. Not yet actioned.
