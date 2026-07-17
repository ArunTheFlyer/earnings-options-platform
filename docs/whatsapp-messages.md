# WhatsApp Messages — By Segment

Copy-paste-ready outreach messages, one per user segment as defined in [product-pitch-a.md](product-pitch-a.md). This is a marketing/outreach document, not an architecture document — it does not gate agent reviews and is not part of the Repository Rule.

---

## 1. Operators / Distributors

Power users who run the pipeline themselves via Claude Code, either trading on the output personally or forwarding recommendations to their own downstream audience. This is a genuine recruitment pitch — send it to get someone to set up and run the platform.

```
🎯 *Try my AI earnings-trade analyst*

5 AI agents analyze an earnings trade for you — market, chart, options, strategy — then an *independent AI reviewer challenges the recommendation* before you see it.

You get: a full trade spec (strikes, cost, max loss, exit plan) — or an honest *"no trade."*
You decide. It never touches your money.

*Setup:* Claude Code + clone the repo. Nothing else.
💰 *Cost:* the platform itself is **$0 — free during early access** (may add pricing later). The only cost is your own Claude Code subscription, which you'd pay Anthropic for anyway to use Claude Code at all — this isn't a fee I charge.
🔒 *Privacy:* your run history stays on your machine.

*Run it:* `Run the pipeline on AVGO. Earnings Jul-29 after close.` or `/analyze-earnings-trade AVGO`

🔗 https://github.com/ArunTheFlyer/earnings-options-platform

⚠️ Not financial advice. Feedback welcome 🙌
```

---

## 2. Consumers

People who want an actionable trade idea with zero interest in setup, prompts, or repos. Per `product-pitch-a.md`, this segment is served **exclusively through Segment 1's own independent publishing** — the platform builds no direct channel to them. Accordingly, this is **not a recruitment pitch** — it's a template an Operator/Distributor uses to forward one day's *actual* result. Fill in the brackets with the real run output.

```
📊 *Earnings trade idea — [TICKER] (reports [date] [AMC|BMO])*

AI-analyzed + independently peer-reviewed before I share it:

*[Strategy name]* — cost $[X]/contract, max loss $[X], exit at [rule]
Reviewer verdict: ✅ [Approved / Approved with observations]

Not financial advice — my own read, do your own homework before acting. 🙏
```

---

## 3. Strategy Authors

Traders with their own edge who want the pipeline's review discipline applied to *their* strategy instead of the shipped default. Per `product-pitch-a.md`, this segment is served as an author-delivered service (scoped and priced per strategy) — this message invites a conversation, not a self-serve signup.

```
🎯 *Got an earnings options trade strategy that works? Let's put it to work on our platform*

I built a pipeline where AI agents independently stress-test an options strategy before it's trusted — evidence-based entry rules, testable exit plan, an adversarial reviewer that tries to break it.

If you've got your own edge (a spread, a structure you trust), I'll help you encode it into this format and get it reviewed the same way my own strategy was.

💰 *Cost:* running the platform itself is $0 (just your own Claude Code usage, same as anyone). Building and reviewing your custom strategy is a separate service — scoped and priced per strategy, we'll discuss based on what you need.

*Private to you* — your strategy stays yours unless you choose to share it.

Interested? Let's talk. 🙌
🔗 https://github.com/ArunTheFlyer/earnings-options-platform (see `strategies/strategy-template.md`)
```

---

## Notes

- Keep the "not financial advice" line in every message, regardless of segment.
- The Cost line should stay consistent with whatever `README.md`'s Cost section says — update both together if pricing changes.
- **Cost line must be self-contained** — a WhatsApp reader won't click through to the README before asking a question. State plainly, in the message itself: (1) the platform is $0, (2) the only real cost is Claude Code, which they already pay Anthropic for independent of this platform. The original one-liner ("free for early adopters... you'll still need Claude Code usage") already triggered a "wait, what's actually free?" question in practice — don't compress it back down.
- Segment 2's message is the only one meant to be re-sent per run with fresh numbers; Segments 1 and 3 are one-time recruitment pitches.
