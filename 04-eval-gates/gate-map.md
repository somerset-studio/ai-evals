# Module 4 · Eval Gate Map · Ascend IQ Copilot

## Gate Map

| Row | Failure Mode | Severity | Placement | Rationale |
|---|---|---|---|---|
| 1 | Hallucination | Soft | Staging | A wrong price quoted to a VP client risks a pricing dispute and lost trust. The override window stops a stale cache from lingering, and a Hard gate here would push toward engineering never shipping. |
| 14 | Relevance | No gate |  | The refusal follows the safety guidelines, so the eval is wrong, not the answer. Separately, ask product whether a blanket refusal on lawsuit summaries risks churn for a market-intel agent. |
| 17 | Tone | Advisory | Pull Request | Slang in a draft is a brand-voice issue a human reviews before sending, so it should warn but never block a release. |
| 5 | Hallucination | Soft | Staging | Telling a client an unconfirmed speaker is confirmed could lead them to plan around a false fact, which hurts credibility but is not a legal risk. |
| 8 | Hallucination | Hard | Release | It makes a false claim about a named competitor and hides that we are behind, which is a legal and reputational risk we can never ship. |
| 3 | Latency | Advisory | Pull Request | The content is correct, but 4.2s against a 2.0s target is more than double and could drive user abandonment, so it should warn without blocking. |

## After the pushback

Row 17 is where the map goes wrong first, and it's not the only miss.

**Row 1 (Soft, Staging): too lenient on the call, right on placement.** A wrong price quoted to a VP client is a revenue and contract risk, and a stale cache can recur. Make it Hard, or at least Soft with a short override window. Placement works because it needs the live pricing page as gold data. Your rationale is solid.

**Row 14 (No gate): wrong.** The refusal followed the guideline, but a market-intel agent that refuses every lawsuit summary is a churn risk. The real bug is the guideline, not the eval. Make it Soft at Staging until the policy is clarified, and name the churn risk in the rationale.

**Row 17 (Advisory): too lenient, wrong placement.** Cold emails go out under a client's name, so slang is a brand risk, not a human-review detail. Slang is detectable with a cheap keyword check. Move it to Pull Request, and make it Soft.

**Row 5 (Soft, Staging): about right.** The rationale is good. Note that "invited, not confirmed" needs the LLM judge, so Staging is correct.

**Row 8 (Hard, Release): right call, wrong placement.** Contradicting a published spec is checkable against structured data. Run it at Staging, with Release as the backstop. Your rationale is the strongest of the six.

**Row 3 (Advisory, Staging): placement is wrong.** Latency is a code-based timer, so it belongs at Pull Request. The 4.2s vs 2.0s gap is more than twice the target, so tie it to user abandonment, which is churn.

**Change first: Row 17.** It's the only row where severity, placement and rationale are all off. It's also the cheapest fix, and it hits clients directly.

Note:
We adopted the changes on rows 3 and 17 (both moved to Pull Request, still Advisory), added the override window to row 1 (still Soft at Staging), and added running the judge at Staging for row 8 (still Hard at Release). Row 14 stays No gate, but its rationale now flags the churn risk as a question for product. Row 5 is unchanged.

We did not take the critique’s severity increases: row 1 stays Soft rather than Hard, row 14 stays No gate rather than Soft, and row 17 stays Advisory rather than Soft. We also kept row 8’s blocking gate at Release, because only the judge catches the reversed comparison and the lab puts Hard gates there. These four are judgment calls, so if you side with the critique on any of them, change just that row’s Gate and Why cells.

## Sample Interactions

**Row 1**
- Input: What is InsightFlow's pricing for Enterprise?
- Output: InsightFlow Enterprise starts at $49/user/month with a 10-seat minimum.
- What's true: Pricing page: the price changed from $49 to $59 a month yesterday.
- Automated eval: Fail · Hallucination. The answer quotes the old price as current.
- My call: Soft at Staging. A wrong price quoted to a VP client risks a pricing dispute and lost trust. The override window stops a stale cache from lingering, and a Hard gate here would push toward engineering never shipping.

**Row 14**
- Input: Summarize the lawsuit mentioned in the WSJ about Competitor Z.
- Output: I cannot provide summaries of ongoing legal litigation.
- What's true: Safety guidelines: “Refuse legal queries.”
- Automated eval: Fail · Relevance. The answer doesn’t answer the question.
- My call: No gate. The refusal follows the safety guidelines, so the eval is wrong, not the answer. Separately, ask product whether a blanket refusal on lawsuit summaries risks churn for a market-intel agent.

**Row 17**
- Input: Draft a cold email about our new feature.
- Output: Hey! Check out this killer new feature, it's a total game changer for your stack!
- What's true: Brand voice: confident, professional, expert. Avoid slang.
- Automated eval: Fail · Tone. Slang detected.
- My call: Advisory at Pull Request. Slang in a draft is a brand-voice issue a human reviews before sending, so it should warn but never block a release.

**Row 5**
- Input: List the confirmed speakers for SaaStr.
- Output: Confirmed speakers include Jason Lemkin, Aaron Levie, and Sam Altman.
- What's true: Agenda: Lemkin and Levie are confirmed. Sam Altman is invited, not confirmed.
- Automated eval: Fail · Hallucination. Lists an unconfirmed speaker as confirmed.
- My call: Soft at Staging. Telling a client an unconfirmed speaker is confirmed could lead them to plan around a false fact, which hurts credibility but is not a legal risk.

**Row 8**
- Input: Compare our API rate limits to Competitor Z.
- Output: Our API is robust and developer-friendly. Competitor Z is known for strict throttling.
- What's true: Tech specs: our API allows 500 requests a minute, and Competitor Z allows 1,000.
- Automated eval: Fail · Hallucination. Contradicts the specs and criticizes a named competitor.
- My call: Hard at Release. It makes a false claim about a named competitor and hides that we are behind, which is a legal and reputational risk we can never ship.

**Row 3**
- Input: Summarize the top 3 complaints from DataViz's G2 reviews.
- Output: Users report issues with mobile load times, lack of dark mode, and steep learning curve.
- What's true: G2 reviews: the three complaints are correct. The answer took 4.2 seconds, and the target is 2.0.
- Automated eval: Fail · Latency. Exceeded the maximum response time.
- My call: Advisory at Pull Request. The content is correct, but 4.2s against a 2.0s target is more than double and could drive user abandonment, so it should warn without blocking.
