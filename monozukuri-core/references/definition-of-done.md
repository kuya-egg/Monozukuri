# Definition of Done

The Definition of Done is generated per task, not a universal list. Each
playbook carries a `dod-template`; the concrete checklist for a task is
derived from it:

```text
DoD = playbook checklist
    + items whose min_risk <= task risk
    + modifier additions
```

A low-risk task keeps only the `min_risk: low` items. A `critical` task
keeps every item at or below `critical`, plus whatever the active
modifiers inject.

## How to render a checklist

1. Take the `dod-template` block for the task's playbook.
2. Keep every item whose `min_risk` is at or below the task's risk tier
   (`low` < `medium` < `high` < `critical`).
3. Append the additions from every active modifier (see below).
4. Group the resulting items by `gate`: `discover`, then `change`, then
   `prove`.
5. The task is done only when every rendered item has traceable evidence
   in the evidence ledger.

## Playbook templates

```yaml
schema: monozukuri/v1
type: dod-template
playbook: feature
checklist:
  - { id: intent,     text: "Purpose, constraints, consumers understood",     gate: discover, min_risk: low }
  - { id: reuse,      text: "Reuse-first search done before new code",         gate: discover, min_risk: medium }
  - { id: design,     text: "Design documented (ADR if architectural)",        gate: discover, min_risk: high }
  - { id: tests,      text: "Behavior and failure-case tests written, passing", gate: prove,   min_risk: low }
  - { id: regression, text: "Existing test suite passes",                      gate: prove,    min_risk: low }
  - { id: evidence,   text: "Evidence ledger complete",                        gate: prove,    min_risk: low }
  - { id: budget,     text: "Actual diff within declared change budget",       gate: prove,    min_risk: medium }
  - { id: docs,       text: "Docs and operational notes updated",              gate: prove,    min_risk: medium }
```

```yaml
schema: monozukuri/v1
type: dod-template
playbook: bugfix
checklist:
  - { id: repro,      text: "Defect reproduced against real code",             gate: discover, min_risk: low }
  - { id: cause,      text: "Root cause identified, not just symptom",          gate: discover, min_risk: low }
  - { id: failing,    text: "Regression test fails before the fix",             gate: prove,    min_risk: low }
  - { id: minimal,    text: "Fix is minimal and targets the cause",             gate: change,   min_risk: low }
  - { id: regression, text: "Regression suite passes after the fix",            gate: prove,    min_risk: low }
  - { id: evidence,   text: "Evidence ledger complete",                        gate: prove,    min_risk: low }
```

```yaml
schema: monozukuri/v1
type: dod-template
playbook: refactor
checklist:
  - { id: tests,      text: "Behavior tests exist before any change",           gate: discover, min_risk: low }
  - { id: no-change,  text: "No behavior change intended or introduced",        gate: change,   min_risk: low }
  - { id: suite,      text: "Full test suite passes unchanged",                 gate: prove,    min_risk: low }
  - { id: complexity, text: "Complexity measurably reduced or held flat",       gate: prove,    min_risk: medium }
  - { id: evidence,   text: "Evidence ledger complete",                        gate: prove,    min_risk: low }
```

```yaml
schema: monozukuri/v1
type: dod-template
playbook: incident
checklist:
  - { id: mitigate,   text: "Mitigation applied and impact stopped",            gate: change,   min_risk: low }
  - { id: timeline,   text: "Impact and timeline recorded",                     gate: discover, min_risk: medium }
  - { id: cause,      text: "Root cause identified",                            gate: prove,    min_risk: high }
  - { id: followup,   text: "Prevention follow-up filed",                       gate: prove,    min_risk: medium }
  - { id: writeup,    text: "Incident writeup completed from template",         gate: prove,    min_risk: high }
```

```yaml
schema: monozukuri/v1
type: dod-template
playbook: migration
checklist:
  - { id: compat,     text: "Backward and forward compatibility assessed",      gate: discover, min_risk: high }
  - { id: rollback,   text: "Rollback path verified to work",                   gate: prove,    min_risk: high }
  - { id: dryrun,     text: "Dry run performed on representative data",         gate: prove,    min_risk: high }
  - { id: integrity,  text: "Data integrity checked after migration",           gate: prove,    min_risk: critical }
  - { id: evidence,   text: "Evidence ledger complete",                        gate: prove,    min_risk: low }
```

```yaml
schema: monozukuri/v1
type: dod-template
playbook: release
checklist:
  - { id: changelog,  text: "Changelog and version updated",                    gate: change,   min_risk: low }
  - { id: prechecks,  text: "Pre-release checks pass",                          gate: prove,    min_risk: low }
  - { id: smoke,      text: "Smoke test passes after deploy",                   gate: prove,    min_risk: medium }
  - { id: rollback,   text: "Rollback rehearsed and ready",                     gate: prove,    min_risk: high }
  - { id: metrics,    text: "Post-deploy metrics confirmed healthy",            gate: prove,    min_risk: medium }
```

```yaml
schema: monozukuri/v1
type: dod-template
playbook: investigation
checklist:
  - { id: question,   text: "Question under investigation stated plainly",      gate: discover, min_risk: low }
  - { id: evidence,   text: "Evidence gathered from the real system",           gate: discover, min_risk: low }
  - { id: findings,   text: "Findings and recommendation written up",           gate: prove,    min_risk: low }
  - { id: throwaway,  text: "Anything built is labeled throwaway",              gate: prove,    min_risk: low }
```

```yaml
schema: monozukuri/v1
type: dod-template
playbook: docs
checklist:
  - { id: verified,   text: "Claims verified against the real system",          gate: discover, min_risk: low }
  - { id: examples,   text: "Examples run and produce the shown output",        gate: prove,    min_risk: low }
  - { id: links,      text: "Links resolve to the right targets",               gate: prove,    min_risk: low }
```

```yaml
schema: monozukuri/v1
type: dod-template
playbook: greenfield
checklist:
  - { id: intent,     text: "nemawashi output exists: purpose, users, constraints, success criteria", gate: discover, min_risk: low }
  - { id: logic,      text: "Business-logic artifact approved by the user",                    gate: discover, min_risk: low }
  - { id: stack,      text: "Tech-stack artifact approved; significant choices recorded as ADRs", gate: discover, min_risk: medium }
  - { id: mapping,    text: "Logic-to-stack map approved with no unmapped elements and no orphans", gate: discover, min_risk: medium }
  - { id: phases,     text: "Phase plan approved; every phase names a playbook and a risk tier", gate: discover, min_risk: low }
  - { id: slice,      text: "Phase 1 is a vertical slice through real boundaries",             gate: discover, min_risk: medium }
  - { id: per_phase,  text: "Each completed phase passed its own playbook's Definition of Done", gate: prove,    min_risk: low }
  - { id: evidence,   text: "Evidence ledger spans the phases built in this effort",           gate: prove,    min_risk: low }
```

## Modifier additions

When a modifier is active, its items are appended to the rendered
checklist and grouped by `gate` like any other item. They apply only
when the task's risk tier is at or above each item's `min_risk`.

- `security` adds:
  - threat model recorded for the change surface (discover, min_risk: high)
  - security tests written and passing (prove, min_risk: medium)
  - security review completed (prove, min_risk: medium)
- `performance` adds:
  - baseline captured before the change (discover, min_risk: medium)
  - benchmark shows the intended improvement (prove, min_risk: medium)
