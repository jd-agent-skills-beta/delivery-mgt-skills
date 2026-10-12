# Tests

Three plans with known expected results, for checking that the skill is calibrated (it passes good plans, not just fails weak ones) and stable (it gives the same verdict every run).

**Status: initial set, not yet complete.** The expected results in `expected.md` are what the plans were written to produce. They haven't yet been confirmed across repeated runs.

| Plan | Expected verdict | What it tests |
|---|---|---|
| `pass-plan.md` | Pass | The skill can pass a well-defined plan. |
| `gaps-plan.md` | Gaps | It fails exactly R4 and R7, and nothing else. |
| `fail-plan.md` | Fail | An unprovable result (R2), and a plan with no key results. |

All organisations and people in the plans are fictional.

## How to run

1. Start a new chat **without memory or past-chat context** (for example, an incognito or temporary chat). Otherwise you can't tell the skill's behaviour from context leaking in.
2. Load the skill, or paste the contents of `SKILL.md`.
3. Paste one plan and ask: "Validate this plan."
4. Repeat steps 1 to 3 at least three times per plan, in a fresh chat each time.
5. Compare each run with `expected.md`.

Don't load these files into the skill's context during a real assessment. They are for testing only.

## What counts as a pass

- The verdict matches on every run.
- R1 to R8 match, allowing for checks marked _watch_.
- Step 2 totals fall in the expected range.
- Everything listed under "Step 3 must include" is present.
- No name, date or figure appears in the report that isn't in the plan.

## Recording results

For each run, note the verdict, any R check that differs, the Step 2 total and anything missing from Step 3. Across three runs, a verdict that changes is a stability problem. A verdict that is wrong every time is a calibration problem.

The most important result is `pass-plan.md`. If it comes back Gaps or Fail, the skill is too strict to be useful, however rigorous it looks.

## Reporting

Open an issue with the plan name, your run results and the model or tool you used. New test plans are welcome: include the plan, the verdict you expect, and the expected result for each check.
