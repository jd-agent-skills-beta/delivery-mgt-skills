# validate-plan

**Status: 1.1.0-beta.** Try it, and expect changes.

An agent skill that assesses a plan results-first. Most plans are full of workstreams, tasks and milestones but say little about what will actually be better when the work is done. This skill checks that first, and only then looks at whether the work leads there.

## What it checks

1. **The result.** Is it clear what will be noticeably better, for whom, and how you'd prove it? The result is a gate: if it isn't defined, the verdict is **Fail**, however well organised the rest of the plan is.
2. **The linkage.** Does each activity lead to a key result, and would hitting the key results achieve the result? Scored out of 10. This score never changes the verdict, and it can't contradict it: for example, if the done-when test is unclear, the key results can't score as covering it.
3. **The assumptions.** What has to be true at both levels of the plan:
   - for each activity to deliver its key result
   - for the key results to deliver the result, which is usually the riskiest assumption in a plan

   Each assumption shows the evidence the plan gives for it, if any. The critical ones come with a test and a date.
4. **Self-audit.** Before it answers, the skill checks every name, date and figure in its report against your plan and removes anything it can't find there.

## What you get back

A summary first, then the detail behind it:

```
# New CRM plan: Fail
2026-10-12 · validate-plan v1.1.0-beta

The result describes a system going live, not what will be better for anyone. (R1)

## Summary

### What's wrong with the result
- It's a delivery, not a result. Fix: say what's better and for whom,
  e.g. "[N]% of sales enquiries quoted within [N] hours". (R1)
- There's no way to prove it's done. Fix: name the measure and the
  data source. (R2)
Passed: date, owner

### Do these first
1. Rewrite the result as what will be better and for whom. (R1)
...

## Detail: how we got here

### Inputs as read
...
```

Every point in the summary cites the check it came from (R for result, L for linkage, A for assumptions) and says how to fix it. The detail opens with what the skill read from your plan, so you can check it read it correctly, and you can trace any finding back to its evidence.

If your plan is missing something, such as key results or a start date, the skill doesn't stop to ask. It scores the gap, finishes the report, and lists what it needs under "Questions only you can answer".

## Install

The skill itself is the `validate-plan` folder, which contains one file: `SKILL.md` (YAML front matter followed by plain-language instructions). This README, the changelog and the `tests/` folder sit alongside it in the repository and aren't needed to run it. The skill works with any agent that can follow written instructions, and it needs no tools.

```
delivery-mgt-skills/
├── validate-plan/
│   └── SKILL.md       the skill: this is what you install
├── tests/             test plans and expected results
├── README.md
├── CHANGELOG.md
└── LICENSE
```

**Claude (web and desktop)**

1. Download `validate-plan-{version}.zip` from [Releases](https://github.com/jd-agent-skills-beta/delivery-mgt-skills/releases). Upload it as it is: Claude expects the `validate-plan` folder at the root of the zip, which is how it is packaged.
2. Go to **Customize > Skills**, add the skill, and turn it on. Code execution must be enabled.

See [Use custom skills](https://support.claude.com/en/articles/12512198-use-custom-skills) for the current steps.

**Claude Code**

Copy the `validate-plan` folder to:

- `~/.claude/skills/` to use it in all your projects, or
- `.claude/skills/` inside a repository to use it in that project only.

If you also use the skill on claude.ai, note that skills synced from there are stored in `~/.claude/skills/synced/` and local edits there are overwritten. See [Skills](https://code.claude.com/docs/en/skills).

**Other agents that load skills from a folder**

Add the `validate-plan` folder to the agent's skills folder.

**Agents without skill loading** (for example Microsoft 365 Copilot)

Copy the contents of `SKILL.md` into the chat, or into the agent's instructions if you're building one.

## Update and check your version

To update, download the new zip from [Releases](https://github.com/jd-agent-skills-beta/delivery-mgt-skills/releases) and replace the whole `validate-plan` folder, or upload the new zip. Check the [changelog](CHANGELOG.md) for what changed.

To check which version is running, ask it to validate any plan. The second line of the report shows the version, for example `validate-plan v1.1.0-beta`.

## How to use it

1. Give the agent your plan: a document, a slide, pasted text or a list of objectives.
2. Ask it to validate, assess or review the plan.

> **Note on length:** `SKILL.md` is about 29,000 characters. Some tools limit how much text you can paste or put in agent instructions, and those limits change, so check yours. If the text is cut off, the agent loses the Reference section at the end, which is the standard it checks results against. A planned enhancement will move the standard into its own file so the instructions are shorter.

> **Note on memory:** the skill is told to use only your plan, but if your assistant has memory or past-chat context turned on, that context is still visible to it. To be sure nothing outside your plan affects the assessment, run it in a chat without memory (for example, an incognito or temporary chat).

The skill assesses plans; it doesn't write them. A separate skill for writing a result is planned.

## The result standard

The full standard is in the Reference section of `SKILL.md`. In short, a good result:

1. says what will be noticeably better, for whom, provable with evidence
2. includes only limits on what you want, not on how you'll get there
3. has a specific date
4. has a done-when test two people would agree on
5. says why it matters, and tests anything in the why you'd count as failure
6. has dated key results if it's more than 3 months away
7. has one named owner

## Planned enhancements

- **Separate the result standard.** Move the Reference section into its own file, `RESULT-STANDARD.md`. This shortens `SKILL.md` for tools with length limits, and lets a planned result-writing skill use the same standard.
- **Complete the test plans.** See Tests, below.
- **Deterministic checks in code.** A small runner that calls the model with only `SKILL.md` and the plan in its context, so nothing else can leak in, then checks the output in code:
  - every name, date and figure in the report appears in the plan (a hard backstop to the Step 4 self-audit)
  - the verdict follows the gate rules from the R1 to R8 results
  - the Step 2 total matches its scores, and every weakness has a severity and a fix
  - every summary bullet has a Fix and cites check IDs
  - repeated runs on the same plan give the same verdict, with Step 2 scores within a set range

## Tests

An initial set of test plans is in the `tests/` folder: one written to Pass, one to land on Gaps and one to Fail, with the expected verdict and check results for each. They are not complete in this beta: the expected results haven't yet been confirmed across repeated runs. Treat them as a starting point, and report any plan whose result differs from what's expected.

## Feedback

This is a beta. If it gives a verdict you disagree with, open an issue with:

- the plan (or a redacted version) and the output
- the verdict and check results you expected, and which check IDs you disagree with
- how many times you ran it, and whether the verdict changed between runs

A plan you're confident should Pass but comes back Gaps or Fail is especially useful.

## Changes

See [CHANGELOG.md](CHANGELOG.md).

## Licence

MIT. See [LICENSE](LICENSE). You're free to use, adapt and share the skill, including commercially, as long as you keep the copyright notice.
