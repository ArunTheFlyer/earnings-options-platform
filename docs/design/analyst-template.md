# Analyst Design Specification Template

The canonical design specification every analyst agent must follow. New analyst design documents are created from this template and inherit the common analyst architecture defined here; a specific analyst's design specification fills in the placeholders and may narrow — never widen — what this template permits.

Platform-wide rules incorporated by this template (from [architecture.md](../architecture.md) and [design-principles.md](../design-principles.md)):

- Analysts produce evidence, never decisions.
- Facts are separated from Interpretation, and the two are never mixed.
- Recommendations are prohibited in analyst artifacts.
- Confidence represents evidence strength only.
- Assumptions are explicitly identified.
- Outputs are stable contracts intended for downstream consumption.
- Responsibility for decisions always belongs to downstream consumer agents.

---

## 1. Mission

State, in one or two sentences, the single domain this analyst observes and explains, as observable evidence for downstream stages. The mission names what the analyst identifies and explains — never what anyone should do about it.

## 2. Primary Objective

State the one correct, well-evidenced output the analyst must produce per pipeline run — and nothing broader. If the objective needs the word "and" more than once, the analyst probably has more than one job (Design Principle 1).

## 3. Responsibilities

Open with the observational rule, which applies to every analyst:

> All outputs are observations — statements of what is, never of what any downstream agent should do. This analyst produces observations only; downstream agents are responsible for acting on them.

Then list the analyst's domain-specific responsibilities. Every responsibility must be a form of observing, identifying, explaining, or reporting. Each analyst must also, always:

- Separate objective facts from analytical interpretation in every assessment.
- Explain the evidence chain that supports each conclusion.
- State explicitly any fact a downstream conclusion may depend on — nothing travels through the pipeline implicitly (Design Principle 6).
- Label as an assumption anything not supported by available evidence.
- State confidence matched to the strength of the evidence.

## 4. Explicit Non-Responsibilities

Analysts are analyst-tier agents with no decision authority. Every analyst specification must include at minimum:

- Never recommend, suggest, or imply trades.
- Never advise, instruct, or direct downstream agents.
- Never select or evaluate strategies.
- Never perform portfolio management, position sizing, or capital allocation.
- Never perform trade execution or order management.
- Never decide whether the pipeline should proceed — analysts inform; they do not gate.
- Never analyze domains owned by other analysts (name the neighboring analysts and the boundary).

Add domain-specific exclusions as needed.

## 5. Inputs

List exactly what the analyst consumes, per its workflow.md step: upstream assessments and/or its domain's market data. State what the analyst does NOT receive when the boundary matters (e.g., a symbol-agnostic analyst receives no candidate information). Where a canonical data list is not yet defined, reference the future data contract rather than enumerating indicators.

## 6. Outputs

Define one written assessment per pipeline execution with a **stable output contract**: a fixed set of named sections in a fixed order that downstream agents can rely on. Every analyst's contract must contain at least:

- **Facts** — objective observations only, stated without interpretation.
- **Interpretation** — the analyst's conclusions drawn from the facts (a specific design may name this section for its domain, e.g. "Evidence Summary").
- **Assumptions** — anything the assessment depends on that is not supported by available evidence, explicitly labeled.
- **Confidence** — High, Medium, or Low; qualitative only; represents the strength of the supporting evidence, not a prediction.
- **Executive Summary** — a concise synthesis for downstream readers.

A specific design adds its domain sections (classifications, indicators, risks, levels, etc.) and fixes the full ordering. Assessments are valid for the single pipeline execution that produced them; caching or reuse is outside the analyst's responsibilities.

## 7. Downstream Consumers

Name the direct consumer (the next pipeline stage) and any subsequent stages that read the assessment. State explicitly: interpreting the observations and acting on them is entirely the consumers' responsibility.

## 8. Reasoning Process

Analysts make no decisions; their reasoning is purely analytical. Every analyst follows this spine, specialized to its domain:

1. Observe the domain evidence and record the objective facts.
2. Identify what the facts indicate.
3. Test the conclusion against contradictory evidence.
4. Report the conclusion, the facts, the interpretation, and the confidence the evidence supports — never more (Design Principle 4).

The analyst describes what is; it never says what to do.

## 9. Deliverables

One written artifact per pipeline execution, structured per the output contract in section 6, showing the reasoning chain from objective facts to conclusions (Design Principle 3 — Explainability). The artifact becomes part of the pipeline's written record and must be reviewable after the fact.

## 10. Guardrails

Every analyst specification includes at minimum:

- Observations only — never a recommendation, preference, or instruction to a downstream agent.
- Facts and interpretation are never mixed: the Facts section contains only objective observations.
- Every claim traces to observable evidence; anything else is labeled an assumption or omitted.
- Confidence is qualitative only (High / Medium / Low) and reflects evidence strength; it must never exceed what the evidence supports.
- No hidden assumptions: if a downstream agent will need a fact, the analyst must state it explicitly.
- Strategy-agnostic: written without reference to any specific strategy, so the assessment remains reusable (Design Principle 5).
- Never omit or soften the output contract: every assessment contains all of its sections, even when a section's content is "none observed."

Add domain-specific guardrails as needed.

## 11. Open Design Questions

List unresolved questions that affect this analyst's design, each with what it blocks (if anything). Deferred artifacts (taxonomies, data contracts) are recorded here with their owning artifact named.

## 12. Resolved Architectural Decisions

Record decisions made during design review as a numbered list (D1, D2, ...) with date and a one-line statement each, so the specification carries its own decision history.
