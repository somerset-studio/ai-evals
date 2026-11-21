# Module 5 · Coverage Matrix · Ascend Analytics AI Portfolio

## Coverage Row

| Product | Hallucination Rate | Fairness | Latency | UX Trust | Robustness |
|---|---|---|---|---|---|
| Ascend IQ (today, pre-launch) | ⚠️ Partial | ❌ Gap | ⚠️ Partial | ⚠️ Partial | ❌ Gap |
| Ascend IQ (launch target) | ✅ Covered | ⚠️ Partial | ✅ Covered | ⚠️ Partial | ✅ Covered |
| AI Report Summaries | ✅ Covered | ❌ Gap | ⚠️ Partial | ❌ Gap | ❌ Gap |
| Competitor Signal Tracker | ❌ Gap | ❌ Gap | ✅ Covered | ✅ Covered | ❌ Gap |
| AI Voice Agent | ⚠️ Partial | ❌ Gap | ❌ Gap | ⚠️ Partial | ✅ Covered |
| Automated Data Tagging | ⚠️ Partial | ⚠️ Partial | ❌ Gap | ❌ Gap | ⚠️ Partial |

**Key:** ✅ Covered = a check runs on that risk regularly. ⚠️ Partial = someone spot-checks it by hand, or the check only measures part of the risk. ❌ Gap = nothing checks it.

**Coverage does not need production traffic.** A check counts as running regularly when it runs on every release against the golden set, so Ascend IQ can reach ✅ before launch. The two Ascend IQ rows show the difference: today, checks exist and have been tested for three metrics, but none runs on every release yet, and two metrics have no working check at all. The launch target is what the gates in `04-eval-gates/launch-strategy.md` and the budget in `05-scale/eval-budget.md` give us once they are built. Closing that gap is the condition for shipping in `06-culture/ship-hold-memo.md`.

## What Runs Today

### Ascend IQ (today, pre-launch)

Ascend IQ hasn't launched, and no check runs on every release yet. A 20-answer audit, scored by an LLM judge and reviewed by hand, found 7 confirmed failures. A metric is marked ⚠️ where a check has been built and tested on Ascend IQ's answers, and ❌ where a failure was spotted but no working check exists yet.

- **Hallucination Rate ⚠️:** 4 of 20 audited answers had a #HALLUCINATION failure (HIGH frequency, P0 in our failure taxonomy). Three checks for false competitor claims were built and run on the 20 answers. The judge caught 2 of 2, with 2 false alarms. It has run once, and it covers competitor claims only.
- **Fairness ❌:** the audit found 1 fairness failure only because a reviewer overrode the judge. No check for fairness exists yet.
- **Latency ⚠️:** an automated check measured a sample answer at 4.2s against a 2.0s target. It is not yet run on every release, and P95 is not measured yet.
- **UX Trust ⚠️:** an automated tone check flagged slang in a drafted email, and we agreed with it. It runs as an Advisory warning only, and it checks slang, not overconfidence.
- **Robustness ❌:** the audit's one robustness failure was a refusal to summarise a lawsuit (row 13). In Module 4 we concluded the refusal followed the safety guideline, so the relevance check was wrong to fail it and can't be relied on. No reliable robustness check exists yet.

### Ascend IQ (launch target)

- **Hallucination Rate ✅:** the competitor-claim judge runs as a Hard gate on the golden set at every release (100% to pass), a code-based price check runs as a Soft gate at Staging, and the Data Fabrication online eval ($85,000) scores live answers.
- **Fairness ⚠️:** reviewers compare matched US and APAC scenarios each week ($5,500). This is a hand spot-check by design, accepted below.
- **Latency ✅:** a release test fails any answer over 2.0s, and a production P95 alert covers live traffic ($1,250).
- **UX Trust ⚠️:** a weekly reviewer audit scores confidence against evidence ($6,500), and a slang check warns at Pull Request. Both are partial by design.
- **Robustness ✅:** the Context Specificity online eval ($70,000) scores live answers against the client's actual question, and unusual client questions are added to the golden set so robustness is also checked before release (see Critical Gap).

To get from today to the launch target on Hallucination Rate, the condition for shipping in the memo:

1. Put the competitor-claim judge in the pipeline, so any answer it fails is blocked before the user sees it.
2. Build and run the golden set, and pass the Hard gate at 100% on competitor-comparison answers.
3. Build the live price check, so the Soft gate mitigation in the launch strategy can work.
4. Re-check the judge's false-alarm rate on the larger set, since the 20-row result is too small to rely on.

### AI Report Summaries

- An LLM judge compares each summary with its source report before every release.
- Response time is logged, but nobody has set a target.

### Competitor Signal Tracker

- A timer alerts if the nightly scoring run takes more than 2 hours.
- A style check flags summaries written in a casual or alarmist tone before they reach the sales team.

### AI Voice Agent

- An eval gate tests its answers to 300 billing questions before every release.
- A supervisor listens to 10 random calls a week for tone.
- A drift monitor alerts when callers start asking about things the agent wasn't tested on.

Hallucination Rate is ⚠️ rather than ✅ because the eval gate tests billing answers only; support answers are not checked.

### Automated Data Tagging

- A code-based check confirms every label comes from the approved list, but not that it's the right label.
- A data analyst reviews 50 labels by hand each quarter, split by source.
- A check confirms the same kind of text always gets the same label.

## Accepted Gap

### Ascend IQ

**Fairness.** We accept this gap for launch because Fairness is P2 in our budget, and bias shows up as a pattern across many answers, so the weekly matched-pair audit of US and APAC scenarios ($5,500) can detect it without a live check. That takes Fairness from ❌ today to ⚠️ at launch, which is enough at P2. The audit found 1 fairness failure in 20 answers (a competitor integration called "seamless" when it is only via Zapier).

We'll look again: if the weekly audit shows a gap between US and APAC answers in two weeks running, or when non-US questions pass 20% of traffic.

### AI Report Summaries

**Fairness.** We accept this gap because the release judge already checks every summary against its source, so a summary that is weaker for one sector or region would mostly show up as a faithfulness failure. A separate per-sector audit would cost more than that risk right now.

We'll look again: if client complaints cluster in one sector or region, or when we start summarising reports for a new region. Review at the end of next quarter.

### Competitor Signal Tracker

**Fairness.** Whilst it isn't ideal that the results lack fairness, I would rather the scores vary under scrutiny or be biased to a particularly load market than the system to hallucinate and make up competitor signal. The scores also are seen by the sales team anyway who can evaluate them as they come in and challenge any bias.

We'll look again: If the system began to ignore a huge market sector to the detriment of our sales team we would need to expand the targeting.

### AI Voice Agent

**Latency.** We accept this gap for now because a slow agent fails loudly, not silently: callers hear the pause, the supervisor's 10 weekly calls would catch a slowdown, and callers who get stuck can still reach a person. A latency check is cheap, so it is the next gap we close after Fairness.

We'll look again: as soon as the supervisor flags a long pause in a weekly sample, or before we add any new call types. Review at month end.

### Automated Data Tagging

**UX Trust.** We accept this gap because labels have no tone and are read by models, not people, so there is no tone that could undermine anyone's confidence. A tone check would cost more than it saves.

We'll look again: if the product starts writing free-text explanations that people read. Review at the next quarterly label review.

## Critical Gap

### Ascend IQ

We've flagged Robustness as the most critical gap for Ascend IQ because nothing we have today reliably tells us whether it answers the question a client actually asked. The one relevance check we saw failed a refusal we later judged correct, and the $70,000 Context Specificity online eval only starts scoring once real clients are using it. A VP client would notice first, when an answer that looks complete ignores what they asked, and they would take it into a roadmap decision. To close it before launch, we'll add unusual and edge-case client questions to the golden set and score them with the LLM judge at every release. After launch, the Context Specificity online eval scores live answers, with a drift monitor that alerts when clients start asking new kinds of question.

### AI Report Summaries

We've flagged Robustness as the most critical gap for AI Report Summaries because summaries go straight to clients with no person in between, and the release judge only sees reports in its test set. When a new kind of report arrives, such as a long, table-heavy or multi-region one, a summary can quietly drop the main finding. The client executive would notice first, and the cost is their confidence in research they pay for. To close it, we'll add a drift monitor that alerts when incoming reports differ from the judge's test set by length, format or sector, and extend the LLM judge to score a weekly sample of live summaries against their source reports.

### Competitor Signal Tracker

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

### AI Voice Agent

We've flagged Fairness as the most critical gap for AI Voice Agent because if it understands some callers worse than others, for example by accent or line quality, those callers get wrong billing answers or get passed to a person far more often, and nothing we run today would show the pattern. Those callers notice first, through repeat calls and complaints, and the cost is billing errors, complaints and lost trust. To close it, we'll add a new eval gate: re-record a subset of the 300 billing questions across a range of accents and line conditions, and fail the release if any group's pass rate falls more than 5 points below the overall rate. Between releases, the supervisor's weekly 10-call sample will be stratified by caller group.

### Automated Data Tagging

We've flagged Latency as the most critical gap for Automated Data Tagging because if tagging slows down or stalls, the models that use these labels keep running on old data and nothing alerts us. Stale data is already a confirmed failure mode in our portfolio: Ascend IQ quoted a superseded price in our audit. The teams that own those models would notice first, probably only after a client spots an out-of-date answer. To close it, we'll add a drift monitor that alerts when the newest labelled filing is more than 24 hours behind the newest filing received, or when the tagging backlog grows two days in a row.

Also watch: the bigger underlying risk is wrong labels. Hallucination Rate is only ⚠️, because the approved-list check confirms a label is valid, not that it's right, and the hand review covers only 50 labels a quarter.

## After the Pressure Test

**Competitor Signal Tracker:** It picked up that the accepting fairness as a gap is ok but the system missing a huge signal area such as an emerging market would be a silent failure which would cause damage to the signal strength of the data.

**Other rows:** not pressure-tested yet.
