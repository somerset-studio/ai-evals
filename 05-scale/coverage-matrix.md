# Module 5 · Coverage Matrix · Competitor Signal Tracker

## Coverage Row

| Product | Hallucination Rate | Fairness | Latency | UX Trust | Robustness |
|---|---|---|---|---|---|
| Competitor Signal Tracker | ❌ Gap | ❌ Gap | ✅ Covered | ✅ Covered | ❌ Gap |

## What Runs Today

- A timer alerts if the nightly scoring run takes more than 2 hours.
- A style check flags summaries written in a casual or alarmist tone before they reach the sales team.

## Accepted Gap

**Fairness.** Whilst it isn't ideal that the results lack fairness, I would rather the scores vary under scrutiny or be biased to a particularly loud market than the system to hallucinate and make up competitor signal. The scores also are seen by the sales team anyway who can evaluate them as they come in and challenge any bias.

We’ll look again: If the system began to ignore a huge market sector to the detriment of our sales team we would need to expand the targeting.

## Critical Gap

The competitor signal tracker is designed to bring actionable intelligence about competitors to the sales team. Made up or false data makes the signal tracker useless and potentially harmful if the team act on false premises.

1. Define the failure
Write down what counts as a hallucination for this product, with a severity tier for each:

Invented claim, figure, date or product (critical)
Misattributed to the wrong competitor (critical)
Rumour or speculation presented as fact (high)
Old news presented as new (medium)
Score rationale that isn't supported by the post (medium)

2. Build the dataset

Sample 150 real source posts and their outputs from production logs, stratified by source type (news vs social), competitor and market.
Add 30-50 hard cases: thin posts, sarcasm, rumours, conflicting reports, and posts mentioning two competitors.
Have a strategy analyst label every summary claim as supported, unsupported or contradicted. A second person labels 20% so you can measure agreement.
Version it and hold part of it out, so you never tune against everything.

3. Build the graders

Deterministic checks: every number, name and date in a summary must appear in the source.
LLM judge: split each summary into atomic claims and check each against the source.
Calibration: run the judge on your labelled set and check that it catches the unsupported claims (recall) without crying wolf (precision). Don't trust it until it agrees with the humans at roughly 90% or better.

4. Metrics and thresholds
Track the share of claims unsupported and the share of summaries with at least one unsupported claim, severity-weighted. Agree a ship threshold with sales, for example zero critical failures in the golden set.

5. Cadence

Every prompt, model or pipeline change: run the full golden set as a release gate.
Nightly: judge a sample of about 25 live outputs, and alert if the rate crosses the threshold.
Weekly: a human reviews the judge's flagged cases plus 10 random ones, to catch judge drift.

6. Feedback loop
Add a "this is wrong" button for sales. Every report becomes a new test case, and a monthly error review feeds back into the taxonomy and prompts.

## After the Pressure Test

It picked up that the accepting fairness as a gap is ok but the system missing a huge signal area such as an emerging market would be a silent failure which would cause damage to the signal strength of the data.
