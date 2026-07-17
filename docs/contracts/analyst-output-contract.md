# Analyst Output Contract

The canonical output interface for every analyst agent, established by [ADR-002](../review/adr-002-analyst-output-contract.md). Every analyst produces **two complementary deliverables** per assessment — a Structured Output Contract and a Narrative Explanation. They are complementary artifacts, not alternative formats: the structured artifact is the interface; the narrative supports it.

All platform-wide analyst rules (evidence only, no recommendations, Facts separated from Interpretation, assumptions labeled, confidence as evidence strength) apply to both deliverables.

---

## A. Structured Output Contract

**Purpose:** the machine-consumable interface for downstream agents.

**Characteristics:**

- Stable field names.
- Deterministic structure — same fields, same order, every run.
- No prose paragraphs — fields carry values, enumerations, and short factual statements.
- No recommendations.
- No ambiguity — a downstream agent must never need to interpret phrasing to extract a field.
- Designed for orchestration and downstream consumption.

**Required common fields** (every analyst, always present):

| Field | Content |
|---|---|
| **Facts** | Objective observations only, stated without interpretation. |
| **Analysis** | The analyst's conclusions drawn from the facts. A domain-specific name is permitted (e.g., the Technical Analyst's *Trend Assessment*, the Market Regime Analyst's *Evidence Summary*). |
| **Assumptions** | Anything the assessment depends on that is not supported by available evidence, explicitly labeled. |
| **Confidence** | High / Medium / Low; qualitative only; represents the strength of the supporting evidence. |
| **Executive Summary** | A concise synthesis for downstream readers. |

Each analyst may extend this structure with domain-specific fields (classifications, indicators, levels, risks, etc.) while preserving these common elements. The analyst's design specification fixes the full field set and ordering; once fixed, the structure never varies between runs.

---

## B. Narrative Explanation

**Purpose:** the human-readable explanation supporting the structured contract.

**Characteristics:**

- Explains the reasoning behind the structured artifact's contents.
- References the supporting evidence.
- May contain prose.
- Must remain consistent with the Structured Output Contract at all times.
- Never introduces conclusions absent from the structured artifact.
- Never contains recommendations.

The narrative exists primarily for human inspection, peer review, and debugging. Downstream agents consume the structured artifact; they never depend on the narrative.
