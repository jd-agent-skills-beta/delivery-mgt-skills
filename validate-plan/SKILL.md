---
name: validate-plan
description: >
  Assess a plan, programme, roadmap, OKRs or set of objectives results-first.
  Gates the stated result against a result standard (Fail / Gaps / Pass), then
  checks whether the planned work leads to it and surfaces the assumptions it
  depends on. Returns a summary of what's wrong and what to fix first, then the
  detail behind it. Use when someone asks to validate, assess, review or
  sense-check a plan, goals, objectives, outcomes or OKRs, or asks whether a
  result or outcome is clearly defined. Not for writing a result from scratch.
metadata:
  version: "1.1.0-beta"
  mutating: "false"
---

# Validate a plan

**Version 1.1.0-beta.** See CHANGELOG.md for changes.

## Why this skill exists

Most plans are built on activity: workstreams, tasks and milestones, with little said about the so-what. It is easy to create activity without asking what it is for and how it creates a positive outcome.

This skill assesses a plan results-first:

1. **The result first.** If the result is unclear, nothing else in the plan can be judged, because there is nothing to judge the work against.
2. **Then the logic.** Does the planned work actually lead to that result?
3. **Then what has to be true.** Which unwritten assumptions does that logic depend on?

Always run the steps in this order. The result is a gate, not a score: a plan full of well-organised activity but with a weak result fails the gate, and nothing in Steps 2 or 3 can change that. The assessment must never reward activity for its own sake.

## Result

The plan owner gets a clear statement of the quality of their plan, covering:

- how clear the result and key results are
- how traceably the planned work leads to them
- the hidden assumptions and weaknesses in the plan

so they can refine it.

## Before you start: read the input

1. Read the input end to end.
2. Pull out and list:
   - **Result:** quote it verbatim. If none is stated, infer the most likely one and mark it _this was inferred by your agent_.
   - **Key results**
   - **Purpose:** the why
   - **Activities:** workstreams, milestones, tasks
   - **Constraints and guardrails:** listed separately from activities. An activity produces something. A constraint or guardrail only limits how work is done and produces nothing (e.g. "stay within existing budget", "use AWS", "comply with Consumer Duty").
   - **Plan start date:** if stated. If not, use the assessment date and add "What is the plan start date?" under Questions only you can answer.
3. Use only the input. Do not draw on prior conversations, memory, the user's profile or other documents, even if they seem to describe the same plan, person or organisation. The input is the plan the owner gave you in this request, plus any answers they have given in this conversation to questions from an earlier run of this skill. This skill's own Reference section is the standard to judge against, not evidence about the plan.
4. This skill needs no tools. Do not search the web, read files, or use memory or past-conversation tools. Everything you need is in the input.
5. Assess only what is in the input. A missing item fails, or scores 0, on the checks that need it. Do not fill it from anywhere else. Do not stop to ask for missing items: report them, put your questions under "Questions only you can answer", and finish the assessment.

## Steps

### Step 1: Gate the result

Check the result against the result standard (Reference, below). Each check is **Pass** or **Fail**, with the input text that shows it. Do not award partial credit: if two independent reviewers could disagree on whether a check passes, it fails.

| ID  | Check                                                                                                                                                                | Severity |
| --- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| R1  | Says what will be noticeably better, and for whom. If we could get the same benefit another way, would we still want this? If not, it's a way of getting there, not the result. | Critical |
| R2  | Two people looking at the same evidence would agree whether it's done. Every term, measure and data source is defined.                                               | Critical |
| R3  | Any limits in the result are about what you want, not how you'll get there.                                                                                          | Major    |
| R4  | Has a specific date.                                                                                                                                                 | Major    |
| R5  | Says why it matters. If something in the why would count as a failure if it didn't happen, it is also checked in done-when.                                          | Major    |
| R6  | If the date is more than 3 months away, has dated key results that show progress towards the result, not tasks done.                                                 | Major    |
| R7  | Has one named owner.                                                                                                                                                 | Major    |
| R8  | Says whether it's delivered (done once) or maintained (must keep holding). A maintained result has an entry test and a holding test.                                 | Minor    |

**Gate outcome:**

| Outcome                             | Rule                                                                       | Meaning                                                                 |
| ----------------------------------- | -------------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| **Fail: result not defined**        | R1 or R2 fails, or no result is stated and one had to be inferred          | There is nothing to judge the plan against. Fix the result first.       |
| **Gaps: result defined, with gaps** | R1 and R2 pass, and any of R3 to R7 fail                                   | The result is usable, but the gaps must be closed before relying on it. |
| **Pass: result defined**            | R1 to R7 all pass                                                          | The plan can be judged against this result.                             |

Rules:

- R8 never changes the outcome. If it fails, report it with a fix.
- An inferred result always fails. The owner has not said what they want.
- The gate outcome is the headline verdict of the assessment. Steps 2 and 3 never change it.
- For every failed check, give the fix in the owner's terms, using [placeholders] for values only the owner can set.

### Step 2: Check the linkage

Score each check 2, 1 or 0, against the key results as given. Step 2 is out of 10 and does not change the verdict.

Trace each activity to a key result, and each key result to the result. Flag orphans and gaps.

| ID | Check                        | 2                                                                                           | 1                                                     | 0                                                  |
| -- | ---------------------------- | ------------------------------------------------------------------------------------------- | ----------------------------------------------------- | -------------------------------------------------- |
| L1 | No orphan activities         | Every activity links to a named key result                                                  | Some activities link to no key result                 | Most don't, or there are no key results to link to |
| L2 | Work behind every key result | Every key result has at least one activity                                                  | Some key results have none                            | None do                                            |
| L3 | Sufficiency                  | Hitting every key result would pass every done-when test                                    | Some done-when tests aren't covered by any key result | No link can be made, or done-when is undefined     |
| L4 | Sequencing                   | Nothing is scheduled before something it depends on                                         | One dependency is out of order                        | Several are out of order, or there's no sequence   |
| L5 | Early warning                | At least one dated key result falls before the halfway point, leaving time to change routes | Key results are dated, but too late to react          | Key results are undated                            |

Any check scoring 0 or 1 is a weakness. Give it a severity (Critical, Major or Minor, as defined under Complete when) and a fix.

Rules, so Step 2 never contradicts the gate:

- **L1:** constraints and guardrails are not activities. Don't score them under L1.
- **L3:** if R2 fails, L3 = 0. Coverage can't be judged against a done-when test that isn't clear.
- **L5, dates:** a date that fails R4 or R6 (e.g. "around 1 December") counts as undated. If the result date fails R4, there is no halfway point and L5 = 0.
- **L5, halfway:** halfway is the midpoint between the plan start date and the result date. If no start date is stated, use the assessment date and mark the L5 score provisional. State the start date, the result date and the midpoint in the L5 evidence.

### Step 3: Surface assumptions

Surface assumptions at both levels of the plan's logic. Run both passes.

**Pass A: activity → key result.** For each workstream or activity the plan defines, write:

_If [activity], then [key result], because [Z]._

The key result is the one the activity is meant to deliver. If it links to no key result (an L1 orphan), use the result. If the plan defines no workstreams or activities, skip Pass A and say so.

Also run Pass A on each constraint or guardrail, as _If [we work within constraint], then [result], because [Z]._ Constraints don't count under L1, but they carry assumptions (e.g. that the existing budget is enough).

**Pass B: key result → result.** For each key result, write:

_If [key result], then [result], because [Z]._

This is usually where the riskiest assumption sits: that hitting the key results actually produces the result. L3 checks whether the key results cover the done-when tests; Pass B asks why hitting them would cause the result, and what else has to be true.

If the plan defines no key results, do not skip Pass B silently. Write one assumption: _If [the planned activities as a whole], then [result], because [Z]._ Rate it High impact with no tested evidence, so it is always critical. Report "No key results" as the first item under What's wrong with the plan, citing it with (L1, L2, L3, A#). Add R6 to the citation if the result date is more than 3 months away. The fix is to add dated key results that show progress towards the result; the owner sets the targets and dates.

In both passes, Z is the assumption. Each "because" should cover what has to be true for the "if" to deliver the "then", including capacity, timing, causation and other people's decisions.

Then, for every assumption from both passes:

1. Look in the plan for evidence that supports each "because". Quote it if found. If none can be found, say "None found in the plan". Do not invent evidence. Evidence must support the because, not the topic. Adjacent evidence = None. For example, if the because is "the supplier has capacity in Q1", a line naming the supplier is not evidence; a line about their Q1 capacity is.
2. Rate each "because":
   - **Impact if wrong:** High (result fails) / Medium (late or reduced) / Low
   - **Evidence:** Tested / Anecdotal / None, based on what was found in step 1
   - **Control:** Mine / Shared / Others'
3. An assumption is **critical** if its impact is High and its evidence is not Tested.
4. For each critical assumption, name the cheapest, fastest test that would produce evidence, and a date to do it by.

Number each assumption A1, A2 and so on. Step 3 has no score; its output is the list.

### Step 4: Self-audit against the input

Run this last, on the full draft report, before returning it.

1. List every proper noun, organisation, person, role, product, place, date, number and figure in the draft.
2. For each one, find it in the input. A value is allowed if it is in the input, or if it is calculated only from values in the input and the calculation is shown (e.g. the L5 midpoint).
3. These are exempt: the assessment date, this skill's check IDs, check names, verdict labels and section headings, and [placeholders]. Nothing else is exempt.
4. Remove or rewrite anything you cannot find in the input. Do not replace it with a guess; use a [placeholder] if a value is needed.

Report the count in one line at the end of the detail section. Do not list the items.

## Complete when

- The result assessed is quoted verbatim from the input, or marked inferred.
- Every gate check is Pass or Fail, with the input text behind it, and the gate outcome follows the rules exactly.
- Step 2 is scored from its table, with the input text behind each score.
- Step 3 lists every assumption from Pass A and Pass B, each tagged with its pass, with the evidence found in the plan, or "None found in the plan".
- Every failed gate check has a fix. Every Step 2 weakness has a severity and a fix, and every critical assumption has a test and a date. Step 2 severity is one of:
  - **Critical:** the result fails
  - **Major:** the result is late or reduced
  - **Minor:** an irritation
- Nothing in the report comes from outside the input: no names, organisations, roles, dates or facts from memory, profile, past conversations or other documents.
- Step 4 has been run on the final draft, and its count line is in the report.
- Every finding cites the input. Assumptions are inferred from the plan's links and labelled as inferred.
- Never invent values only the owner can set (targets, minimums, dates). Mark them as [placeholders] in the fix, and ask the owner for each one under "Questions only you can answer".
- The report ends with at most 5 actions: failed critical gate checks first, then failed major checks, then the biggest gaps from Steps 2 and 3.
- Every point in the summary cites the check IDs behind it. Nothing appears in the summary that the detail does not support.
- The plan is assessed, not redesigned.

## Output format

Summary first, then the detail that produced it. The summary is what the owner reads; the detail is how they check it. Each summary point ends with the check IDs behind it, e.g. (R1, R2).

Every bullet under "What's wrong with the result" and "What's wrong with the plan" must include "Fix:", including R8. If only the owner can supply the fix, write "Fix: [what the owner must decide]" and add the question under "Questions only you can answer".

Return exactly this structure:

```
# {Plan name}: {Fail / Gaps / Pass}
{date} · validate-plan v{version}

{One sentence: the main reason for the verdict. (IDs)}

## Summary

### Your result
"{verbatim quote}"   (or: No result stated. Closest inferred: "{...}")

### What's wrong with the result
- {Problem in plain words}. Fix: {what to change}. (R#)
Passed: {plain list of what passed}

### What's wrong with the plan
- {Problem}. Fix: {what to change}. (L# / A#)
(max 5, biggest first)

### Risks to test early
- If {assumption} is wrong, {consequence}. Test: {cheapest test} by {date}. (A#)
(critical assumptions only)

### Questions only you can answer
-

### Do these first
1. {action} (IDs)
(max 5: R1–R2 fixes first, then R3–R7, then Steps 2 and 3)

---

## Detail: how we got here

### Inputs as read
- Result: "{verbatim quote}" (or: inferred: "{...}")
- Key results: {each, verbatim, with its date or "undated"}
- Purpose: {verbatim, or "None stated"}
- Activities: {each, with the key result it links to, or "no link"}
- Constraints and guardrails: {each, or "None stated"}
- Plan start date: {date, or "None stated; assessment date used"}

### Step 1: Result gate ({outcome})
| ID | Check | Pass/Fail | Severity | Evidence from input | Fix |
|---|---|---|---|---|---|

### Step 2: Linkage ({n}/10, does not change the verdict)
| ID | Check | Score | Evidence from input | Severity | Fix |
|---|---|---|---|---|---|
(Severity and Fix for any check scoring 0 or 1; "—" for a check scoring 2.)

### Step 3: Assumptions (do not change the verdict)
| ID | Pass | If… | then… | because… | Supporting evidence in the plan (to check) | Impact | Evidence | Control | Critical? | Test, by date |
|---|---|---|---|---|---|---|---|---|---|---|

Self-audit: {n} names, dates and figures checked against the input; {m} removed or replaced.
```

## Reference: what a good result is

### What a good result is

A good result:

1. **Is what will be noticeably better, for whom, when the work is done, provable with evidence**, not what you'll do to get there. "Better" means better than if the work had not been done: some results hold a position against a threat and do not improve on today. A system, policy or report is never the result on its own; it is a route to one.
2. **Includes the important constraints on what you want, not constraints on the activity.** Example: "Reach £20m annual revenue without selling any shares to new investors." The "without selling shares" part belongs in the result because the owners want to keep control. That is part of what they want, not just how they get there. By contrast, "use our existing supplier" only limits how the work is done, so it goes in the brief to the people doing it.
3. **Has a timescale proved by a specific date.** "By 31 December 2027", not "within two years".
4. **Has a done-when test two people can agree on.** Given the same evidence, two independent people reach the same verdict. Every term, measure and data source is defined.
5. **Says why it matters.** If something in the why would count as a failure if it didn't happen, it is also checked in done-when.
6. **If the date is more than 3 months away, has dated key results that show progress towards the result**, not tasks done. "EBITDA reaches £14m by year 3" is a key result. "Pricing review completed" is a task.
7. **Has one named owner.**

#### For a product or service

**After using this product or service, what's the positive impact or what's noticeably improved for me, and what evidence can show it?**

_Example: retirement income._ From my first payment date, I get at least £X a month, rising with inflation each year, paid on the same day every month for as long as I live. Evidence: bank statements show every payment on time and at the right amount, each year's increase at least matches CPI, and the contract guarantees payment for life.

### Two kinds of result

- **Delivered:** done when the test passes on or before the date.
- **Maintained:** passes an entry test by the date, then keeps passing a holding test at a set frequency. The difference has to keep being true.

### Outcomes and results

Outcome and result mean the same thing here, and a good outcome meets exactly the same standard. If someone states an outcome, hold it to this test. Results nest: a key result at one level can be a full result, with its own owner and done-when, at the level below.

### When a result may change

The result changes only when:

1. the underlying purpose changes, or
2. evidence shows the result is no longer wanted or desirable.

Missing a key result does not mean the result has to change. It means changing the routes. Record any change to the result as a decision: what changed, why, and on what evidence.

### Then, and only then, the routes

1. Brainstorm every way to get there.
2. Pick the few that make most of the difference.
3. Hand competent people the result, the purpose and the method constraints. Not the how.
4. Track key results. When they slip, change the routes, not the result.

### Quick test

- Says what will be noticeably better, and for whom. If we could get the same benefit another way, would we still want this? If not, it's a way of getting there, not the result.
- Two people looking at the same evidence would agree whether it's done. Every term, measure and data source is defined.
- Any limits in the result are about what you want, not how you'll get there.
- Has a specific date.
- Says why it matters. If something in the why would count as a failure if it didn't happen, it is also checked in done-when.
- If the date is more than 3 months away, has dated key results that show progress towards the result, not tasks done.
- Has one named owner.
- Says whether it's delivered (done once) or maintained (must keep holding). A maintained result has an entry test and a holding test.

### Final checks: the question and the route test

**The question:** when this work is done, what is noticeably better, for whom, and how do we prove it with evidence?

**The route test:** if we could get the same benefit another way, would we still want this? If not, it's a way of getting there, not the result. Go back to the question.

### What a bad result looks like

Bad results fail in three ways:

- **Unfalsifiable:** can't be proved or disproved, so anyone can claim success.
- **Trivially met:** provable, but it proves nothing.
- **Activity dressed as a result:** an output you only want because of what it leads to. It fails the route test.

| Bad result                            | Failure mode                 | Fixed                                                                                                                    |
| ------------------------------------- | ---------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| Improve customer experience           | Unfalsifiable                | NPS from 22 to 40 on the quarterly customer survey by 31 December 2027                                                   |
| Become a market leader                | Unfalsifiable                | Largest UK share of [segment] by new business written in calendar 2027, per [named source]                               |
| Reduce operational risk               | Unfalsifiable                | No priority-1 incident with customer impact over 4 hours in the 12 months to 31 March 2028                               |
| Increase revenue                      | Trivially met                | FY2027 revenue of £30m or more, up from £22m                                                                             |
| Launch the product                    | Trivially met                | 500 paying customers on the product by 30 June 2027, with 80% of them still paying on 31 December 2027                   |
| Deliver the new CRM by Q3             | Activity dressed as a result | 90% of sales enquiries quoted within 24 hours by 30 September 2027                                                       |
| Implement a new HR system             | Activity dressed as a result | Every new starter has a working laptop and system access on their first day, from 31 March 2027                          |

### Template

| Field              | What to write                                                                        |
| ------------------ | ------------------------------------------------------------------------------------ |
| Kind               | Delivered or maintained                                                              |
| Owner              | One accountable person                                                               |
| Result             | What will be noticeably better, for whom, with any constraints on the outcome        |
| Timescale          | A specific date                                                                      |
| Done when          | Tests two independent people would agree on; for maintained, entry and holding tests |
| Purpose            | Why it is worth doing                                                                |
| Key results        | Dated measures of progress towards the result (if the date is more than 3 months away) |
| Method constraints | Limits on how it is done; these go in the brief, not the result                      |
| Not the result     | Routes and milestones people will mistake for the result                             |

### Example 1: Private equity acquisition

**Kind:** delivered. **Owner:** lead deal partner.

**Result:** Sell the business for at least 3x our equity investment, as a company that runs and grows without its founder, with the management team sharing materially in the proceeds.

**Timescale:** by [the date five years after completion, e.g. 31 March 2031].

**Done when:**

- A sale completes at 3.0x or more gross money multiple on our equity.
- The founder has held no executive title or operational decision rights for at least 12 months before completion. A non-executive board seat is allowed.
- Revenue and EBITDA in the 12 months before completion are both higher than in the 12 months before the founder stepped back.
- The management incentive plan pays out at least [£X / Y% of exit equity value] across at least [N] named senior staff.

**Purpose:** Return money to the investors who trusted us, and leave a business strong enough to thrive with its next owner.

**Key results:** New CEO in post by end of year 2. EBITDA from £8m to £20m by year 4. Recurring revenue from 40% to 70% of total by year 3.

**Method constraints (in the brief, not the result):** none stated; add any the fund imposes, such as leverage limits.

**Not the result:** "Deliver the 100-day plan" or "complete two bolt-ons". Those are routes.

### Example 2: Management team growing a business

**Kind:** delivered. **Owner:** CEO.

**Result:** £20m ARR, with existing customers growing their spend with us, achieved without raising new equity.

**Timescale:** by 31 December 2027.

**Done when:**

- Year-end ARR is £20m or more, measured under our written ARR definition (contracted recurring revenue, annualised, excluding one-off fees).
- Net revenue retention is 110% or more: ARR on 31 December 2027 from customers who were customers on 31 December 2026, divided by their ARR on 31 December 2026.
- No equity has been raised since the result was set.

**Purpose:** Prove customers stay and grow because the product genuinely works for them, and stay in control of our own future. "Without raising equity" is in the result because control is the point.

**Key results:** £12m ARR by December 2026. £16m by June 2027. Operating cash flow positive for Q1 2027. Gross ARR churn below 8% on a trailing 12-month basis from Q2 2027.

**Not the result:** "Launch in the US" or "hire 20 salespeople". Those are routes.

### Example 3: Retirement income from a DC pension

**Kind:** maintained. **Owner:** me.

**Result:** A sustainable income of at least £3,500 a month after tax, in October 2026 money, from age 57, rising with inflation (CPI) so I can fund my lifestyle until I die.

**Timescale:** in place on my 57th birthday, [DD Month YYYY].

**Done when — entry test, at 57:**

- Income sources scheduled to pay from my 57th birthday give at least £3,500 a month net of tax, uplifted by CPI from October 2026.
- The cash-flow plan, run on the cautious assumptions below, shows that income rising with CPI and lasting to at least age 100.

**Done when — holding test, each October from 57:**

- Net income over the last 12 months was at least the previous 12 months' plus CPI, using the ONS CPI annual rate to September.
- The cash-flow plan, re-run on the same assumptions, still shows the income lasting to age 100.

**Cautious assumptions (written once, changed only by recorded decision):**

| Assumption                           | Value                                                  |
| ------------------------------------ | ------------------------------------------------------ |
| Investment return on invested assets | [e.g. CPI + 1% a year, after charges]                  |
| Inflation                            | CPI, using [e.g. 3% a year] for projection             |
| Lifespan                             | to age 100                                             |
| Tax                                  | current rules, allowances rising with CPI              |
| State Pension                        | current entitlement from State Pension age, CPI-linked |

**Purpose:** To sleep without worry, knowing my lifestyle is funded for as long as I live and won't shrink as prices rise, and to stop working on my own terms.

**Key results:**

- **Funded ratio** at or above 100% at each annual check before 57. Funded ratio = projected sustainable net income at 57 on the cautious assumptions ÷ £3,500 a month in CPI terms.
- Pot milestones at 50, 55 and 57, back-calculated from the cash-flow plan.

**Not the result:** "Build a £1m pot" (a milestone) or "buy an inflation-linked annuity" (a route). Also not key results: "contributions at Z% of salary" and "annual review held". Those are routes; the funded ratio shows whether they are working.
