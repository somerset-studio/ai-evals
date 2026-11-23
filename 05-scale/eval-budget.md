# Module 5 · Eval Budget · Ascend IQ

**Quarterly budget cap:** $200,000
**Online eval spend:** $155,000 (2 of at most 3 online evals)

## Budget

| Failure | Trust metric | Risk | Level | Cost |
| --- | --- | :---: | :---: | ---: |
| Data Fabrication | Hallucination Rate | P0 | Online eval | $85,000 |
| Context Specificity | Robustness | P1 | Online eval | $70,000 |
| Overconfident Tone | UX Trust | P1 | Periodic audit | $6,500 |
| Data Bias | Fairness | P2 | Periodic audit | $5,500 |
| Slow Answers | Latency | P3 | Code-based check | $1,250 |

## Downgrades

### Periodic audit · Overconfident Tone (UX Trust · P1)
- **Cheaper check:** A reviewer samples outputs weekly, oversampling high-stakes ones (financial or operational recommendations), and scores each for confidence versus evidence: unsupported precision, recommendations made on thin evidence, and certainty that outruns the sources. Hedge wording alone doesn't count as a pass.
- **Why it’s enough:** Overconfidence is visible to a human reader and drifts gradually, so a risk-weighted weekly sample catches a trend before it spreads. Fabrication and context misses fail silently and need live scoring, but tone doesn't. Flagged cases feed back into prompt fixes, and the sample size can grow using the unspent budget.

### Periodic audit · Data Bias (Fairness · P2)
- **Cheaper check:** Each week, reviewers compare matched US and APAC scenarios and score accuracy, omissions, unsupported assumptions and recommendation quality, not just summary length or detail. Samples are stratified by client type and data characteristics.
- **Why it’s enough:** Bias shows up as a pattern across many outputs, so a stratified matched-pair sample detects it without scoring every response live. At P2, a weekly review is proportionate, and it can move to a higher frequency if a skew appears.

### Code-based check · Slow Answers (Latency · P3)
- **Cheaper check:** An automated release test runs a fixed set of long multi-step questions and fails the release if any exceed 2.0 seconds. A production alert on p95 latency, built on existing telemetry, covers live traffic between releases.
- **Why it’s enough:** Latency is directly measurable by rule, so it needs no judgement or LLM scoring. The release gate catches regressions, and the p95 alert catches load spikes and slow paths in production. At P3, this is proportionate.

## Reasons for changes
We funded the two failures that fail silently and carry the highest cost of being wrong as online evals: Data Fabrication (P0, $85,000), because an invented competitor figure can drive a bad decision before anyone notices, and Context Specificity (P1, $70,000), because an answer that looks complete but ignores what the client asked can only be caught by scoring live traffic against the actual question. That brings online spend to $155,000 of the $200,000 cap with two of the three permitted evals used. A third online eval, Overconfident Tone at $65,000, would have taken us to $220,000 and broken the cap, so it became our deliberate P1 downgrade to a weekly periodic audit ($6,500). That downgrade is defensible because overconfidence is visible to a human reader and drifts gradually, so a risk-weighted sample that oversamples high-stakes outputs and scores confidence against evidence, not just hedge wording, can catch it in time. Data Bias (P2) stays on a $5,500 audit, redesigned to compare matched US and APAC scenarios on accuracy and omissions rather than summary length, and Slow Answers (P3) gets a $1,250 code-based check, a release test for long multi-step questions plus a production p95 alert, because latency is directly measurable by rule and needs no judgement. Together this puts the money where failures are hardest to see and least tolerable, and uses cheaper, well-designed checks where problems are visible, slow-moving or easy to measure. We hold the remaining $31,750 as a reserve, released if a hotfix needs re-evaluation or if audit findings (for example, a rising tone or bias flag rate) justify a larger sample. It is not allocated to a third online eval, which at $65,000 would exceed the cap.

## Considerations for further budgeting
Overconfident tone could be done by a code-based check

Context: A code-based check can catch the surface of overconfidence, but not the real failure, which is certainty outrunning evidence. So it works as a cheap first filter, not a full replacement for the audit.

**What code can check (cheap, runs on every release or every output)
Certainty language without support: flag absolute phrases ("will", "definitely", "guaranteed", "clearly", "always") in answers where the retrieval step returned thin or no sources.
Unsupported precision: flag specific figures (percentages, prices, counts) that don't appear in the retrieved source data. This overlaps with the fabrication check, but it's a good tone signal too, because false precision reads as confidence.
Missing uncertainty markers on weak evidence: if retrieved sources fall below a threshold (fewer than N, or a low retrieval score), require some qualifier or source caveat in the answer, and flag it if absent.
Recommendation without basis: flag directive language ("you should", "we recommend") when no supporting source is cited.
Regression test: run a fixed set of deliberately thin-evidence questions on every release and fail if the answers come back unhedged.

**What code can't check
Whether the confidence is actually justified. A confident answer backed by strong sources is fine, and a hedged answer can still mislead. That judgement about confidence versus evidence needs a human or an LLM judge.
Subtle certainty, such as framing or selective emphasis, with no flaggable keyword.



