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
- **Cheaper check:** An automated release test runs a fixed set of long multi-step questions and fails the release if any exceed 10 seconds. A production alert on p95 latency, built on existing telemetry, covers live traffic between releases.
- **Why it’s enough:** Latency is directly measurable by rule, so it needs no judgement or LLM scoring. The release gate catches regressions, and the p95 alert catches load spikes and slow paths in production. At P3, this is proportionate.

