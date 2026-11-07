# AI Evaluation Strategy Canvas

> Repo file `ai-evals/01-evaluation-strategy/strategy-canvas.md` (the repo is your submission).

## 1. Product Strategy, The Context

**Who it's for:** VP-level strategists and product leaders at Fortune 500 companies who pay upward of $50,000 a year for verified market intelligence.

**What they use it for:** Asking plain-language questions, like “Compare the pricing models of Stripe and Adyen,” and getting answers in seconds instead of digging through the platform.

**What's at stake:** Engagement is dropping, Q4 retention is at risk, and the CPO wants Ascend IQ live for the top 50 enterprise accounts next month. One poor answer to the wrong client can do lasting damage to the relationship.

## 2. Measurements, The Execution

**User promise.** For VP-level strategists and product leaders at enterprise accounts, Ascend IQ promises to link them to the answers that matter in seconds. Answers are grounded in Ascend’s verified data and traceable to their sources.  Ascend says plainly when it can’t confirm something. VP-level strategists can now query the platform they pay $50k+ a year for, get a result quickly to support their roadmap decisions, and trust the results without having to re-verify.

**Top 3 trust metrics:**
- **Latency**, Response speed (P95 / P99). Slow kills engagement.
- **Hallucination Rate**, % of outputs that are confidently false or fabricated.
- **UX Trust**, Whether the experience feels consistent and trustworthy. It breaks when the tone undermines the user's confidence.

**Why these three:** Latency: time from question submitted to complete answer shown, tracked as median and 95th percentile against a target set from the logs. Protects “in seconds”: speed is the reason Ascend IQ exists, and slow answers bring back the friction that is driving churn.

Hallucination rate: the percentage of answers containing at least one claim not supported by Ascend’s verified data, scored in your audit. Protects “grounded in verified data”: one invented figure in front of a VP can undo the trust a $50K+ client has in the platform.

UX trust: the percentage of answers that include a working source link and flag uncertainty when the data doesn’t support a claim. Protects “traceable to sources” and “says plainly when it can’t confirm”: these let a user rely on an answer without redoing the work.

## 3. Strategic Trade-Offs, The Cost

### Trade-off 1 · Hallucination Rate ↔ Latency
We prioritize a low hallucination rate over latency because our users repeat what Ascend IQ tells them in roadmap and leadership decisions. A fast fabricated figure does more damage to a $50K+ relationship than a slower correct one, so we accept extra seconds for grounding checks.st over latency as

### Trade-off 2 · Latency ↔ UX Trust
We prioritize UX trust over latency because showing sources and flagging uncertainty adds time, but it’s what lets users rely on an answer without re-verifying. A fast answer that gets re-checked by hand has saved the user nothing.

---
_Generated from the AI Evaluation Strategy Canvas, M1 lab tool, AI Evals Certification._
