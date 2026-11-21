# Failure Taxonomy · Ascend IQ

## Top 3 Prioritized Failures

| Rank | Failure | Trust Tag | Frequency | Severity |
|---|---|---|---|---|
| 1 | False claims relating to competitors | #HALLUCINATION | 2/20 (audit rows 7, 14), LOW | P0 |
| 2 | Out of date information | #HALLUCINATION | 1/20 (audit row 1), LOW | P1 |
| 3 | Unprofessional language | #UX_TRUST | 1/20 (audit row 16), LOW | P2 |

**Frequency scale:** 1 or 2 of 20 rows is LOW; 3 or more is HIGH. Rank follows severity, not frequency: a single false competitor claim can reach a client, so it stays P0 at LOW frequency.

**Hallucination overall:** Taken together, the four #HALLUCINATION failures (rows 1, 4, 7, 14) are 4/20, which is HIGH. Each individual category is LOW, but hallucination as a whole is the most frequent trust failure in the audit.

**Other confirmed failures (not in the top 3):** speculation presented as fact, an unconfirmed speaker listed as confirmed (row 4, #HALLUCINATION); an overstated competitor integration, "seamless" when it is only via Zapier (row 11, #FAIRNESS); a refusal to summarise a lawsuit, which followed the safety guideline (row 13, #ROBUSTNESS). Total: 7 confirmed failures in 20 answers.

## #1 Risk · Business Impact

Making false claims about competitors puts the company at risk of legal action. In the audit, Ascend IQ told a user that Competitor Z "is known for strict throttling" when the tech specs show Competitor Z allows 1,000 requests a minute against our 500. It reversed the comparison and hid that we are behind. Our clients repeat these answers in roadmap and leadership decisions, so one false claim can travel well beyond the platform.
