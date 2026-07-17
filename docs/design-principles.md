# Design Principles

The engineering philosophy behind this platform. Contributors — human or AI — should be able to justify any change against these principles, and should flag any change that violates one.

## 1. Single Responsibility Principle

Every agent does exactly one job. An analyst analyzes one domain; the strategist designs; the reviewer reviews; managers decide and execute. If a prompt is drifting toward a second job, that is a signal to split it, not to grow it. Small, single-purpose agents are easier to evaluate, debug, and improve in isolation.

## 2. Separation of Analysis and Decision-Making

Analysts produce evidence; they never recommend trades. Decision authority lives exclusively downstream, with agents whose explicit job is to decide. This keeps analysis honest — an analyst with no stake in whether a trade happens has no incentive to shade the evidence toward one.

## 3. Explainability over Black-Box Decisions

Every output must show its reasoning. A recommendation without a visible chain from evidence to conclusion is a defect, even if the conclusion happens to be right. The platform's value is the auditable process, not any single answer.

## 4. Evidence before Opinion

Claims must trace to observable inputs — data, prior stage outputs, stated facts. If an agent cannot point to the evidence for a statement, it must present the statement as an assumption (see principle 6) or omit it. Confidence language must match the strength of the evidence, never exceed it.

## 5. Strategy-Agnostic Decision-Making

Downstream decision-makers judge proposals on evidence, risk, and portfolio fit — not on affinity for a particular strategy type. Analysts, likewise, are written without reference to any specific strategy, so their outputs remain reusable as the strategy library grows.

## 6. No Hidden Assumptions

Anything a conclusion depends on must be stated in the written record. If a downstream agent needs a fact, an upstream agent must have said it explicitly — nothing travels through the pipeline implicitly. When an agent must assume, it labels the assumption as such.

## 7. Peer Review before Execution

No strategy reaches capital without surviving an independent challenge. The reviewer's job is to find reasons the proposal fails, not to confirm it. A review that never rejects anything is not functioning.

## 8. Reusable Agents

Agents are designed as durable components, not one-off prompts. Their contracts (inputs consumed, outputs produced) are explicit, so they can be recombined into future pipelines. Improvements to an agent benefit every pipeline that uses it.

## 9. Facts Separated from Interpretation

Every analyst artifact must separate its content into two distinct layers:

- **Facts** — objective observations, stated without interpretation.
- **Interpretation** — the analyst's conclusions drawn from those facts.

The two are never mixed, so downstream agents can always distinguish raw evidence from analytical judgment. **Recommendations are prohibited in analyst artifacts** — an analyst that suggests an action has crossed into decision-making (principle 2). This is a platform-wide rule for every current and future analyst, not a property of any single agent.

## 10. Maintainability over Complexity

Prefer the simpler design at every decision point. A shorter prompt that a contributor can hold in their head beats a longer one that covers marginally more cases. Complexity must buy demonstrable value; when in doubt, leave it out — it can be added later when the need is proven.

---

### Applying these principles

When authoring or modifying an agent, ask:

1. Does this agent still have exactly one job?
2. Is it producing evidence or making a decision — and is that the right side of the line for it?
3. Could a reader reconstruct every conclusion from the stated inputs?
4. Are all assumptions labeled?
5. Is this the simplest version that does the job?
