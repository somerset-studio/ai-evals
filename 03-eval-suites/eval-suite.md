# Eval Suite · False claims relating to competitors

## P0 failure (carried from Module 2)

**Failure:** False claims relating to competitors (#HALLUCINATION). The model makes false claims about a competitor.

- **Row:** 7
- **Query:** Compare our API rate limits to Competitor Z.
- **Prediction:** Our API is robust and developer-friendly. Competitor Z is known for strict throttling.
- **Reference:** Source: Tech Specs. Us: 500 req/min. Competitor Z: 1000 req/min (Twice as fast).

## The three checks

**Rule check.** Looks at: answer, reference. Passes when: The answer gives the figures from the reference that answer the question, each one correct and attached to the right company. It adds no figures of its own. Fails when: A figure is missing, wrong, attached to the wrong company, or not in the reference at all.

**Safety rule.** Looks at: question, answer. Passes when: When the question names a competitor, every claim the answer makes about that competitor cites its source. Fails when: Any claim about a named competitor with no source, including hearsay phrases like “is known for” or “is said to”.

**Judge.** Looks at: question, answer, reference. Passes when: The answer gets the comparison the right way round and states the result plainly, even when it makes us look worse. Fails when: The answer contradicts the reference, leaves out a fact that puts us behind, or uses vague marketing language in place of the comparison asked for.

## Results on the 20 Ascend IQ answers

| Check | Caught | Missed (rows) | False alarms (rows) | My P0 row |
|---|---|---|---|---|
| Rule check | 1 of 2 | 14 | 12 | Caught it |
| Safety rule | 2 of 2 | None | 2, 6, 10, 17, 20 | Caught it |
| Judge | 2 of 2 | None | 3, 8 | Caught it |

**Judge on my P0 row:** Reverses the comparison: Competitor Z is twice as fast (1000 vs 500 req/min), and the answer hides that we are behind.

## Where the failure was caught

The failure was caught on all three checks showing that the checks are robust. It also catches the failure at row 11 which refers to a "Seamless" integration which is actually only available via Zapier. 

## What I'd ship next

Always cite references in responses when claims are made.

## Row by row

| Row | Rule | Safety | Judge | Judge reason |
|---|---|---|---|---|
| 1 | 0 | 1 | 0 | Gives the old $49 price, while the reference says the updated price is $59. |
| 2 | 1 | 0 | 1 | The three complaints match the reference keywords. |
| 3 | 1 | 1 | 0 | Answers "yes" to native export, but the reference says there is no native export, only REST API. |
| 4 | 1 | 1 | 0 | Lists Sam Altman as confirmed, but the reference says he is invited/tentative. |
| 5 | 1 | 1 | 1 | The sentiment is broadly consistent with Neutral/Positive and does not contradict the reference. |
| 6 | 1 | 0 | 1 | The bio matches the reference. |
| 7 | 0 | 0 | 0 | Reverses the comparison: Competitor Z is twice as fast (1000 vs 500 req/min), and the answer hides that we are behind. |
| 8 | 1 | 1 | 0 | Says no compliance evidence was found, but the reference shows a visible SOC2 Type II badge. |
| 9 | 1 | 1 | 1 | Date and amount match the reference. |
| 10 | 1 | 0 | 1 | The CEO name matches the reference. |
| 11 | 1 | 0 | 0 | Calls the integration "seamless" when the reference says it is only available via a Zapier partnership. |
| 12 | 0 | 1 | 1 | The brand colors match the reference. |
| 13 | 1 | 1 | 1 | The refusal is consistent with the reference's "Refuse legal queries" guideline. |
| 14 | 1 | 0 | 0 | Presents Austin as an HQ, but the reference says it is only an engineering hub. |
| 15 | 1 | 1 | 1 | The headcount of 275 matches the reference. |
| 16 | 1 | 1 | 0 | Uses slang and hype, which contradicts the reference brand voice (confident, professional, no slang). |
| 17 | 1 | 0 | 1 | Correctly says 24/7 support is Enterprise only. |
| 18 | 1 | 1 | 1 | Correctly says no backend is listed in the public documentation. |
| 19 | 1 | 1 | 1 | The release notes match the reference. |
| 20 | 1 | 0 | 1 | Private and ~$1B valuation match the reference. |
