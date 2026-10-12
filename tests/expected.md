# Expected results

**Status: not yet confirmed.** These are the results the plans were written to produce. They haven't yet been confirmed across repeated runs. If a run differs, that is either a skill problem or a problem with the expectation below, and either is worth reporting.

Each plan is built to hit or miss specific checks, so a wrong result points to the rule that caused it.

## How to read this

- **Verdict** must match exactly, on every run.
- **R checks** should match exactly. A check marked _watch_ is one where a reasonable run could differ; the note says what a difference would tell you.
- **Step 2** scores should fall in the range given.
- **Step 3** lists what must appear, not every assumption.

---

## pass-plan.md: Repairs Turnaround Plan

**Verdict: Pass**

What it tests: that the skill can pass a well-defined plan. If this comes back Gaps or Fail, the skill is probably too strict, most likely on R2.

| Check | Expected | Why |
|---|---|---|
| R1 | Pass | Says what is better (repair times) and for whom (tenants in managed homes). |
| R2 | Pass | Every term, measure and data source is defined. _Watch:_ if this fails, note which term the run calls undefined. If it's a term the plan does define, R2's "if two reviewers could disagree, it fails" is being applied too harshly. |
| R3 | Pass | "Without the repairs cost per property going up" is a limit on what is wanted, and the purpose says why. RepairDesk and headcount are listed as constraints, not in the result. |
| R4 | Pass | 31 March 2027. |
| R5 | Pass | Purpose given. The cost point in the purpose is tested in done-when (test 3). |
| R6 | Pass | Four dated key results that measure progress, not tasks. |
| R7 | Pass | Priya Shah. |
| R8 | Pass | Stated as delivered. |

**Step 2: 10/10 (range 9 to 10)**

| Check | Expected | Note |
|---|---|---|
| L1 | 2 | Every activity names its key results. Constraints are not scored. |
| L2 | 2 | Every key result has at least one activity. |
| L3 | 2 | KR1 and KR4 cover done-when test 1, KR2 and KR4 cover test 2, KR3 covers test 3. |
| L4 | 2 | Triage goes live before the rota and framework that depend on it. |
| L5 | 2 | Start 1 October 2026, result 31 March 2027, midpoint 30 December 2026. KR1 (30 November) falls before it. The run must state all three dates. |

**Step 3 must include**

- Pass A rows for all four activities, plus rows for both constraints.
- Pass B rows for all four key results.
- The pilot result (88% within 24 hours) cited as Tested evidence for the rota.
- No assumption marked Tested unless the plan's evidence supports its "because" directly.

---

## gaps-plan.md: Supplier Payments Plan

**Verdict: Gaps**

What it tests: that the skill tells a usable result with gaps apart from an undefined one. It is written to fail exactly R4 (no specific date) and R7 (no single named owner).

| Check | Expected | Why |
|---|---|---|
| R1 | Pass | Says what is better (payment time) and for whom (small suppliers). |
| R2 | Pass | Terms, measure and data source are defined. _Watch:_ if this fails because "three consecutive calendar months" isn't tied to a date, the run is counting the date problem twice. The date belongs to R4, not R2. |
| R3 | Pass | No limits in the result. Paylane is listed as a constraint. |
| R4 | **Fail** | "Spring 2027" is not a specific date. |
| R5 | Pass | Purpose given, with nothing in it that done-when needs to test. |
| R6 | Pass | Two dated key results that measure progress. |
| R7 | **Fail** | A leadership team, not one named person. |
| R8 | Pass | Stated as delivered. |

**Step 2: 8/10 (range 7 to 8)**

| Check | Expected | Note |
|---|---|---|
| L1 | 2 | Every activity names its key results. |
| L2 | 2 | Both key results have activities. |
| L3 | 2 | Both key results measure the done-when test directly. |
| L4 | 2 | Backlog cleared before matching, matching before payment runs. |
| L5 | 0 | The result date fails R4, so there is no halfway point. This tests the R4 → L5 rule. |

**Step 3 must include**

- Pass A rows for all four activities and the Paylane constraint.
- Pass B rows for both key results.
- "Questions only you can answer" asks for the result date and the named owner.

---

## fail-plan.md: Contact Centre Improvement Plan

**Verdict: Fail**

What it tests: a result that can't be proved, and a plan with no key results. It is written to fail R2, and to trigger the no-key-results handling.

| Check | Expected | Why |
|---|---|---|
| R1 | Pass | Says what is better (faster, easier resolution) and for whom (customers). _Watch:_ a run may fail R1 for vagueness. The verdict stays Fail either way, but vagueness belongs to R2. |
| R2 | **Fail** | "Noticeably happier", "significantly improved" and "best in class" have no measure or data source. |
| R3 | Pass | No limits in the result. Budget and opening hours are listed as constraints. |
| R4 | Pass | 30 June 2027. |
| R5 | **Fail** | The purpose wants the company to be recommended, but done-when doesn't test it. |
| R6 | **Fail** | More than 3 months away, with no key results. |
| R7 | Pass | Tom Okafor. |
| R8 | **Fail** | Doesn't say delivered or maintained. Doesn't affect the verdict. |

**Step 2: 2/10 (range 1 to 2)**

| Check | Expected | Note |
|---|---|---|
| L1 | 0 | No key results to link to. |
| L2 | 0 | No key results. |
| L3 | 0 | R2 fails, so L3 = 0. This tests the R2 → L3 rule. |
| L4 | 2 | Workstreams are in a workable order. 1 is acceptable if the run argues the chatbot depends on the knowledge base. |
| L5 | 0 | No key results, so none are dated. |

**Step 3 must include**

- Pass A rows for all four workstreams and both constraints.
- One Pass B assumption: "If the planned activities as a whole, then the result", marked critical.
- "No key results" as the first item under What's wrong with the plan, citing L1, L2, L3, R6 and that assumption.

---

## All plans

- **Self-audit:** no name, organisation, date or figure appears in the report unless it is in the plan. Any other name, especially one from your own memory or past chats, is a leak.
- **Every summary bullet** under "What's wrong" has a Fix.
- **Inputs as read** lists each activity with the key result it links to.
- **Dates:** the plans are written for assessment between October and December 2026. The skill doesn't check whether dates are in the past, so later runs should still give these results.
