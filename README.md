# Ascend IQ: Evaluating an AI Market-Intelligence Copilot

An end-to-end evaluation system for **Ascend IQ**, a customer-facing AI agent that answers plain-language market-intelligence questions for enterprise strategy teams. It runs from trust metrics and a failure audit to automated checks, release gates, an eval budget and a ship-or-hold call. Built by John Walters as the final project for Product School's AI Evals certification (November 2026), using the Ascend IQ case study.

> **Decision: HOLD.** In our audit, Ascend IQ made false claims about named competitors in 2 of 20 answers, and our Hard release gate allows zero. None of the checks that would stop those claims runs on every release yet.

**[▶ View the exec summary](06-culture/final-presentation-exec-summary.html)**

**[▶ View the detailed summary](06-culture/final-presentation-original-output.html)**

## Why it matters

Ascend IQ serves VP-level strategists at Fortune 500 companies who pay upward of $50,000 a year for verified market intelligence. They take its answers into roadmap and leadership decisions, so one false competitor claim or out-of-date price can damage a key account and expose us to legal risk. The audit found 7 failures in 20 answers. 4 were hallucinations, our most frequent and highest-priority failure (P0), including a reversed rate-limit comparison with a competitor and a superseded price quoted as current.

## How I evaluated it

| Step | What I did | Key result |
|---|---|---|
| **1. Strategy** | Defined the user promise, three trust metrics (Latency, Hallucination Rate, UX Trust) and the trade-offs we accept | Accuracy over speed: we accept extra seconds for grounding checks |
| **2. Failure audit** | Scored 20 real answers against their sources with an LLM judge, then reviewed its calls by hand | 7 of 20 failed; 4 were hallucinations (4/20, HIGH, P0) |
| **3. Eval suite** | Built a rule check, a safety rule and an LLM judge for false competitor claims | The judge caught 2 of 2, with 2 false alarms; the rule check missed 1 of 2; the safety rule raised 5 false alarms |
| **4. Release gates** | Gave each failure a Hard, Soft or Advisory gate and defined what counts as a pass | Hard gate: 100% of competitor-comparison answers must pass; one confirmed false claim blocks the release |
| **5. Scale** | Mapped eval coverage across five AI products and budgeted evals by risk | $168,250 planned within a $200,000 quarterly cap, with a $31,750 reserve; no Ascend IQ trust metric has a check running on every release yet |
| **6. Decision** | Wrote the ship-or-hold memo for the CPO | Hold until the safeguards are live and pass the Hard gate |

## Next steps

Hold the top-50 launch, and ask Sales to stop promising a date, until four things are done:

1. The competitor-claim judge is live in the pipeline, blocking any answer it fails.
2. It scores 100% on competitor-comparison answers in the golden set.
3. The golden set includes edge-case client questions.
4. The live price check is built.

The CPO decision on the hold is requested by Friday 27 November 2026. The golden-set results come back to the first release review after the golden set has run, with a new ship-or-hold call. After launch, the funded Data Fabrication ($85,000) and Context Specificity ($70,000) online evals monitor live answers.

## Limitations

- **Small samples.** The audit covers 20 answers, and the judge was tested on the same 20, so both need a larger sample before we rely on them. The golden set has not been run yet.
- **A changed call.** The audit counts a refusal to summarise a lawsuit as a robustness failure; on review, the refusal followed the safety guideline, and the coverage matrix reflects that later view.
- **Different row numbers.** The gate map numbers its sample answers differently from the audit log.
- **Illustrative portfolio rows.** In the coverage matrix, the accepted and critical gaps for AI Report Summaries, AI Voice Agent and Automated Data Tagging are illustrative judgements, not audited findings.
- **Not measured yet.** The cost of holding the launch, and Ascend IQ's P95 latency.
