# Review Plan

The permanent review methodology for this project. All architecture and agent reviews follow this document. Progress is tracked in [review-status.md](review-status.md), sessions are journaled in [review-log.md](review-log.md), and decisions arising from review are recorded in [architecture-decisions.md](architecture-decisions.md).

> **Repository Rule**
>
> **No agent prompt may be considered complete until it has successfully passed the review checklist below. Likewise, no documentation may be treated as complete until it has been reviewed against the architecture.**

## Objectives

1. Ensure every agent behaves correctly within the pipeline — right job, right inputs, right outputs, right authority.
2. Ensure every document accurately reflects the architecture and stays consistent with the others.
3. Catch violations of the [design principles](../design-principles.md) before they become embedded convention.
4. Leave a written, auditable trail of what was reviewed, what was found, and what was decided.

## Review Phases

Every review of an artifact proceeds through three phases, in order:

1. **Correctness** — Does the artifact do the right thing? Is the agent's behavior, authority, and contract correct per the architecture? Findings here must be resolved before anything else matters.
2. **Consistency** — Does it agree with the rest of the repository? Terminology per the [glossary](../glossary.md), hand-offs per the [workflow](../workflow.md), responsibilities per the [architecture](../architecture.md).
3. **Clarity** — Is it well written? Wording, structure, and readability improvements come last, and only after the first two phases pass.

## Review Order

Artifacts are reviewed in pipeline order, upstream before downstream, because downstream contracts depend on upstream outputs:

1. Documentation foundation (README, architecture, workflow, design principles, glossary)
2. Market Regime Analyst
3. Technical Analyst
4. Options Market Analyst
5. Earnings Options Strategist
6. Strategy Peer Reviewer
7. Portfolio Manager
8. Trade Execution Manager

## Review Checklist

Every agent review must evaluate all twelve dimensions:

| # | Dimension | Question |
|---|---|---|
| 1 | **Mission** | Is the agent's purpose stated clearly, and is it the purpose the architecture assigns it? |
| 2 | **Responsibility** | Does it have exactly one job? Is any second job creeping in? |
| 3 | **Scope** | Are the boundaries explicit — what it does, and what it deliberately does not do? |
| 4 | **Inputs** | Are all consumed inputs listed, and are they actually produced by upstream stages? |
| 5 | **Outputs** | Are outputs fully specified, and do they satisfy what downstream consumers need? |
| 6 | **Decision Process** | Is it clear what the agent decides (if anything) and by what criteria? Does its authority match the decision hierarchy? |
| 7 | **Thinking Process** | Does the prompt guide how the agent should reason, not just what to produce? |
| 8 | **Interfaces** | Are hand-offs to neighboring agents explicit and consistent with the workflow document? |
| 9 | **Guardrails** | Are failure modes addressed — overreach, unstated assumptions, opinion beyond evidence? |
| 10 | **Architecture Alignment** | Does it respect the separations of concerns and its tier (analyst / strategist–reviewer / manager)? |
| 11 | **Explainability** | Will its outputs show the reasoning chain from evidence to conclusion? |
| 12 | **Future Extensibility** | Can it be reused or extended without rewriting — no hard-coded assumptions that block growth? |

## Definition of Done — Agent Review

An agent review is done when, and only when:

- [ ] All twelve checklist dimensions have been evaluated and the outcome recorded.
- [ ] Every correctness finding has been resolved (fixed, or explicitly accepted with rationale).
- [ ] Consistency with architecture, workflow, and glossary has been verified.
- [ ] Any architectural decision made during the review is recorded in [architecture-decisions.md](architecture-decisions.md).
- [ ] A session entry has been appended to [review-log.md](review-log.md).
- [ ] The artifact's checkbox in [review-status.md](review-status.md) is updated.

If any item is unchecked, the artifact remains **pending**, regardless of how much work was done.

## Review Rules

1. **Behavioral correctness is always more important than wording improvements.** A review that polishes prose while a behavioral defect stands has failed. Wording changes are never grounds to reopen a passed review; behavioral findings always are.
2. Reviews follow the phase order: correctness, then consistency, then clarity — never the reverse.
3. Findings must cite the specific text and the specific principle, architecture statement, or contract they violate. "I would phrase it differently" is not a finding.
4. A review either passes or it does not. There is no "mostly done."
5. Changing an artifact after it passed review returns it to pending in [review-status.md](review-status.md).
6. Every review session, however small, is logged in [review-log.md](review-log.md).

## Review Philosophy

Review here is adversarial by design, mirroring the platform itself: the reviewer's job is to find reasons the artifact fails, not to confirm that it passes. A review that never finds anything is suspect. At the same time, review serves shipping — findings must be actionable, tied to evidence, and proportionate. The purpose is not perfect prose; it is a pipeline of agents whose behavior is correct, whose contracts hold, and whose decisions can be explained after the fact.
