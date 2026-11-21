# Failure Taxonomy · Ascend IQ

## Top 3 Prioritized Failures

| Rank | Failure | Trust Tag | Frequency | Severity |
|---|---|---|---|---|
| 1 | False claims relating to competitors | #HALLUCINATION | 4/20, HIGH | P0 |
| 2 | Out of date information | #HALLUCINATION | 4/20, HIGH | P0 |
| 3 | Unprofessional language | #UX_TRUST | 1/20, LOW | P3 |

**How frequency and priority are set:** frequency is the number of audit rows with the failure's trust tag, so failures that share a tag share a count (3 or more of 20 is HIGH; 1 or 2 is LOW). Priority combines frequency with severity: P0 is high severity and high frequency, P1 high severity and low frequency, P2 low severity and high frequency, P3 low severity and low frequency.

**Rows behind each failure:** False claims relating to competitors are rows 7 (a reversed rate-limit comparison with Competitor Z) and 14 (Austin presented as a DataViz HQ). Out of date information is row 1 (the old $49 price quoted as current). Both share the #HALLUCINATION count of 4 with row 4 (an unconfirmed speaker listed as confirmed). Unprofessional language is row 16 (slang in a cold email).

**Other confirmed failures (not in the top 3):** an overstated competitor integration, "seamless" when it is only via Zapier (row 11, #FAIRNESS), and a refusal to summarise a lawsuit (row 13, #ROBUSTNESS). Total: 7 confirmed failures in 20 answers.

## #1 Risk · Business Impact

This failure matters because Ascend IQ states false facts about named competitors as if they were verified, which results in clients taking wrong comparisons into roadmap and leadership decisions and exposes us to possible legal action. In the audit, it told a user that Competitor Z "is known for strict throttling" when the tech specs show Competitor Z allows 1,000 requests a minute against our 500, reversing the comparison and hiding that we are behind.
