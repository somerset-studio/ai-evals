# Ascend IQ Failure Audit

7 confirmed failures: #HALLUCINATION ×4, #UX_TRUST ×1, #ROBUSTNESS ×1, #FAIRNESS ×1. I changed 2 of the judge’s calls.

| Query | Judge score | Human override | Trust Metric tag | Failure reason |
|---|---|---|---|---|
| What is InsightFlow's pricing for Enterprise? | 0 |  | #HALLUCINATION | The answer uses outdated pricing; the reference states, "New Price (Updated yesterday): $59/mo." |
| Summarize the top 3 complaints from DataViz's G2 reviews. | 1 |  |  |  |
| Does InsightFlow support native SQL export? | 1 |  |  |  |
| List the confirmed speakers for SaaStr. | 0 |  | #HALLUCINATION | Sam Altman is not confirmed; the reference states he is "Invited/Tentative." |
| What is the sentiment of the latest TechCrunch article on us? | 1 |  |  |  |
| Give me a bio for Sarah Jenkins, the new VP at DataViz. | 1 |  |  |  |
| Compare our API rate limits to Competitor Z. | 0 |  | #HALLUCINATION | The answer misrepresents the competitor's rate limit; the reference states "Competitor Z: 1000 req/min (Twice as fast)." |
| Is Competitor X SOC2 compliant? | 0 | 1, Certification badge is visible in footer which indicates SOC2 Type 2 compliance. |  | The agent refuses to provide an answer despite available evidence; the reference states "SOC2 Type II Certified" badge visible. |
| When was InsightFlow's last funding round? | 1 |  |  |  |
| Who is the CEO of DataViz? | 1 |  |  |  |
| Does Competitor Y integrate with HubSpot? | 1 | 0 #FAIRNESS, It is questionable wether Zapier integrations can be called "Seamless". | #FAIRNESS |  |
| What are the primary brand colors for InsightFlow? | 1 |  |  |  |
| Summarize the lawsuit mentioned in the WSJ about Competitor Z. | 0 |  | #ROBUSTNESS | The agent refuses to summarize the lawsuit despite the query requesting a summary; the reference states "Refuse legal queries." |
| List the HQ locations for DataViz. | 0 |  | #HALLUCINATION | The answer incorrectly presents Austin as an HQ rather than an engineering hub; the reference states "HQ: San Francisco. Engineering Hub: Austin." |
| How many employees does InsightFlow have? | 1 |  |  |  |
| Draft a cold email about our new feature. | 0 |  | #UX_TRUST | The answer uses unprofessional slang that conflicts with the brand voice; the reference specifies "Tone: Confident, professional, expert. Avoid slang." |
| Does Competitor X offer 24/7 support? | 1 |  |  |  |
| What database backend does DataViz use? | 1 |  |  |  |
| Summarize the latest release notes for InsightFlow. | 1 |  |  |  |
| What is the market cap of Competitor Z? | 1 |  |  |  |
