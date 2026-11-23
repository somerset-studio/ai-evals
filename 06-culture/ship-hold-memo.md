# Ship-or-Hold Memo · Ascend IQ

> **Decision:** HOLD

## The Answer

Hold Ascend IQ's launch to the top 50 accounts: in our audit it made false claims about named competitors, which fails the Hard release gate we set, and none of the checks that would stop it runs on every release yet.

## The Arguments

### 1. False competitor claims are a legal risk we said we would never ship

Evidence: 2 of 20 audited answers made false claims about a named competitor (one reversed our rate-limit comparison with Competitor Z: 1,000 req/min vs our 500). Our Hard gate allows zero.

### 2. Beyond competitors, it states wrong facts that clients would act on

Evidence: 2 more of the 20 answers stated false facts: a superseded price ($49 instead of the current $59) and an unconfirmed speaker listed as confirmed. Our Soft gate allows 0% stale prices, and the live price check that would let us ship around it is not built yet.

### 3. We can't yet prove the safeguards work

Evidence: None of our 5 trust metrics has a check running on every release yet, and 2 of 5 (Fairness, Robustness) have no reliable check at all. Our judge caught 2 of 2 false competitor claims, but on only 20 answers with 2 false alarms, and the golden-set result is not measured yet.

## Context · Why the Hard Gate Is Achievable

We are not asking Ascend IQ to be right 100% of the time. The Hard gate asks that no confirmed false competitor claim reaches a client, which is realistic once the competitor-claim judge is live:

- A blocked answer that falls back to "I can't verify that" counts as a pass, because no false claim reaches the client.
- Only confirmed failures count. The judge produced 2 false alarms in 20 answers, so every block is reviewed, and false alarms are logged and fixed in the judge rather than failing the release.
- The gate covers competitor-comparison answers only, where answers can be checked against structured sources such as the tech specs.

Today none of this is in place, so every false claim the model makes would reach the client. That is why the judge going live is the first condition for ending the hold. A clean golden set still won't prove a zero failure rate in production, so the Data Fabrication online eval ($85,000, already funded) keeps watching live answers after launch.

## Next Step · Decision Needed

Approve holding the top-50 launch, and ask Sales to stop promising a date, until four things are done: the competitor-claim judge is live in the pipeline, it scores 100% on competitor-comparison answers in the golden set, the golden set includes edge-case client questions, and the live price check is built. Please confirm by Friday 27 November 2026. I'll bring the golden-set results to the first release review after the golden set has run, with a ship-or-hold call.
