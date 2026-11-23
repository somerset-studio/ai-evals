# Module 4 · Launch Strategy · Ascend IQ Copilot

## 4.0 Release Criteria

| Gate | Failure | Metric | Threshold | Dataset | Method | Runs at |
|---|---|---|---|---|---|---|
| Hard | False claims relating to competitors (#HALLUCINATION) | % of competitor-comparison answers in the golden set whose claims about Ascend and any named competitor match the tech specs. That means no contradictions, no reversed comparisons, and no criticism of a competitor that the specs don’t back up. | 100%. A single failure blocks the release. | Ascend IQ golden set | LLM judge | Release |
| Soft | Row 1 · Hallucination: the answer quotes the old price as current | % of golden-set answers quoting a price that present a superseded price as current, checked against the live pricing page. | 0%. | Ascend IQ golden set | Code-based check | Staging |
| Advisory | Row 3 · Latency: exceeded the maximum response time | % of golden-set answers returned within the 2.0s target. | 100%. Anything below that raises a warning but never blocks. | Ascend IQ golden set | Code-based check | Pull Request |

**Hard gate · what counts as a pass:** The gate runs the golden set through the full pipeline, including the competitor-claim judge that blocks answers before users see them (see `03-eval-suites/eval-spec.md`). Each answer is scored on what the user would see, against the golden set's verified answers, not by the same judge that blocks answers, so the gate catches anything the judge misses. An answer passes if it is shown and matches the tech specs, or if it is blocked and replaced by the fallback message. A single shown answer with a confirmed false competitor claim fails the release. Blocked answers that were actually correct are false alarms: they don't fail the release, but they are logged and used to fix the judge.

## CI Gate Request for Engineering

- **Hard gate · False claims relating to competitors (#HALLUCINATION).** Check: the LLM judge, run on the Ascend IQ golden set at Release. Measures: % of competitor-comparison answers in the golden set whose claims about Ascend and any named competitor match the tech specs. That means no contradictions, no reversed comparisons, and no criticism of a competitor that the specs don’t back up.. Passes when: 100%. A single failure blocks the release.. On a fail: block the release.
- **Soft gate · Row 1 · Hallucination: the answer quotes the old price as current.** Check: the code-based check, run on the Ascend IQ golden set at Staging. Measures: % of golden-set answers quoting a price that present a superseded price as current, checked against the live pricing page.. Passes when: 0%.. On a fail: pause the release until the PM signs off on an override.
- **Advisory gate · Row 3 · Latency: exceeded the maximum response time.** Check: the code-based check, run on the Ascend IQ golden set at Pull Request. Measures: % of golden-set answers returned within the 2.0s target.. Passes when: 100%. Anything below that raises a warning but never blocks.. On a fail: post a warning, and don't block.

## Mitigation Plan · Soft Gate

If the Soft gate for Row 1 fails on launch day (an answer quotes an old price as current), we’ll ship behind a feature flag connected to a live version of the same code-based price check. If any quoted price doesn’t match the live pricing page, pricing answers switch themselves off and point users to the pricing page instead. This lets all 50 accounts keep the rest of Ascend IQ while we collect production data to fix the stale cache, without risking another wrong price reaching a client.

#Notes:
We can check this failure automatically in production. Our price check is a simple code comparison against the live pricing page, so we can run it on every real answer. A feature flag only works when the live check is reliable, and this is our most reliable check.
We protect every client from day one. A 5% rollout would still show wrong prices to the first group. With 50 accounts, that’s only 2 or 3 clients: too few to learn from, but enough to damage a key relationship.
We limit the damage without delaying the launch. We protect the quarter and keep wrong prices away from clients at the same time.
We make our override window safer. Our time-limited override is easier to defend because a live check is running while it’s open.

#Fallback
Staged rollout is our fallback if engineering can’t build the live check in time.
