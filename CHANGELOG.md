# Changelog

All notable changes to the validate-plan skill. Versions follow [semantic versioning](https://semver.org/):

- **Patch** (1.0.1): wording fixes that don't change how a plan is assessed.
- **Minor** (1.1.0): new checks, examples or output sections.
- **Major** (2.0.0): changes that would give a different verdict on the same plan.

## [Unreleased]

### Planned

- Move the result standard (the Reference section) into its own file, `RESULT-STANDARD.md`, so `SKILL.md` fits tools with length limits and the standard can be shared with a planned result-writing skill. Step 1 will point to the new file by name.
- Complete the test plans in `tests/`: confirm the expected results across repeated runs, and add cases from beta feedback.
- Add deterministic checks in code: a runner that calls the model with only `SKILL.md` and the plan in context, then checks the output for names, dates and figures not in the plan, a verdict that breaks the gate rules, Step 2 totals and missing fixes, and verdict stability across repeated runs.

## [1.1.0-beta] - 2026-10-12

A minor release. The result gate (Step 1) is unchanged, so the same plan gets the same verdict as in 1.0.0-beta. Step 2 scores and the Step 3 assumption list can differ.

### Added

- **Assumptions at both levels.** Step 3 now runs two passes:
  - **Pass A:** for each activity, "If [activity], then [key result], because [Z]". An activity linked to no key result is traced to the result.
  - **Pass B:** for each key result, "If [key result], then [result], because [Z]". This is usually where the riskiest assumption sits: that hitting the key results produces the result.
- **No key results are flagged.** If a plan has none, Pass B writes one critical assumption that the activities as a whole deliver the result, and "No key results" is the first item under What's wrong with the plan.
- **Constraints and guardrails** are pulled out separately from activities. They are not scored under L1, but Pass A still surfaces the assumptions they carry (e.g. that the budget is enough).
- **Plan start date** is pulled out of the input. If none is stated, the assessment date is used and the owner is asked for it.
- **Step 4, self-audit.** Before returning the report, the skill checks every name, date and figure in it against the input and removes or replaces anything it can't find. The report ends with a count of items checked and removed.
- **Inputs as read.** The Detail section opens with what the skill extracted (result, key results, purpose, activities and their links, constraints, start date), so the L1 and L2 tracing can be checked.
- **Severity and Fix columns** in the Step 2 table. Any check scoring 0 or 1 is a weakness and gets a severity and a fix.
- **Install and update instructions** in the README, for Claude, Claude Code and other agents, with how to check which version is running.
- **Initial test plans** in `tests/`: one written to Pass, one to land on Gaps and one to Fail, with expected results. Not complete in this beta: the expected results haven't yet been confirmed across repeated runs.

### Changed

- **Step 2 can no longer contradict the gate:**
  - If R2 fails, L3 = 0.
  - A date that fails R4 or R6 counts as undated for L5. If the result date fails R4, L5 = 0.
  - Halfway (L5) is defined as the midpoint between the plan start date and the result date. All three dates are shown, and the score is marked provisional if the start date was assumed.
- **Input-only boundary.** The skill uses only the plan it was given, plus the owner's answers to questions from an earlier run. It does not draw on memory, user profiles, past conversations or other documents, and needs no tools.
- **Missing items no longer stop the assessment.** The skill reports them, asks for them under "Questions only you can answer", and finishes the report.
- **Stricter evidence rule.** Evidence must support the "because", not just mention the same topic. Adjacent evidence counts as None.
- **Every summary bullet has a Fix**, including R8. Where only the owner can supply the fix, the bullet says what they must decide and the question is added for them.

## [1.0.0-beta] - 2026-10-12

First public release, for testing. Expect changes as it is tried on real plans.

### What it does

- **Step 1, result gate.** Checks the plan's result against eight checks from the result standard (R1 to R8) and gives a verdict:
  - **Fail:** the result isn't defined (R1 or R2 fails, or no result was stated).
  - **Gaps:** the result is usable but has gaps (any of R3 to R7 fails).
  - **Pass:** R1 to R7 all pass.
  Nothing later in the assessment can change this verdict.
- **Step 2, linkage.** Traces activities to key results and key results to the result, scored out of 10 (L1 to L5).
- **Step 3, assumptions.** Writes each defined workstream or activity as "If X, then Y, because Z", quotes any supporting evidence the plan gives, and flags critical assumptions with a test and a date (A1, A2 and so on).
- **Output.** A summary first (verdict, what's wrong, what to do first), then the detail that produced it. Every summary point cites the check IDs behind it.
- **Reference.** Includes the full result standard, a template, a table of bad results with fixed versions, and three worked examples.

### Before 1.0

The skill went through six internal iterations on 11 and 12 October 2026. The main changes were:

- replacing a score out of 30 with the result gate, so a poorly defined result can no longer be offset by well-organised activity
- rewriting the result standard around "what will be noticeably better, for whom, provable with evidence"
- moving to a summary-first output with traceable check IDs
- simplifying the assumptions step to a list with supporting evidence to check
