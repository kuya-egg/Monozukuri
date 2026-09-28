# Checkpoint planning

When work splits into more than one unit — tasks inside a change, or phases in
a blueprint — order the units **smallest first** and end each one at a
checkpoint. Small units land fast, expose wrong assumptions while they are
still cheap to fix, and leave the large units a smaller, better-understood
surface to change.

## What a checkpoint is

A checkpoint is a verified stopping point: the unit's behavior works, its
checks pass, its evidence is recorded, and the work could be paused, resumed,
reviewed, or rolled back from here. A unit is not done until it reaches one.

## Ordering rule

1. **Dependencies first.** A unit never starts before the units it depends on.
   This is a hard constraint; everything below orders units within it.
2. **Smallest first.** Among the units that are ready, take the one that is
   smallest and easiest to change: fewest files, narrowest blast radius, most
   reversible, lowest risk tier.
3. **Largest last.** The biggest, riskiest, or hardest-to-reverse unit goes
   after the small ones, when the codebase and your understanding have both
   moved in its favor.
4. **Tie-break:** prefer the unit that retires the most uncertainty for the
   least effort.

A large *unknown* is not a reason to build the large unit first. Retire it
early with a small probe — a spike, a prototype call, a failing test — and
keep the full unit in its smallest-first slot.

## Sizing

Size each unit before ordering, using the change budget
(`change-budget.md`):

| Size | Rough shape |
| --- | --- |
| S | one concern, one to three files, trivially reversible |
| M | one module or boundary, a handful of files, reversible with care |
| L | several modules, a data model, a public contract, or a migration |

An L unit is a signal to split it. Order the resulting pieces by the same rule.

## Procedure

1. List the units.
2. Mark dependencies between them.
3. Size each one S / M / L; split any L that can be split.
4. Order: dependency layers first, smallest-first within each layer.
5. Give each unit an exit criterion — the check that proves its checkpoint.
6. Work one unit at a time. At each checkpoint: verify, record evidence, commit.
7. Re-plan after each checkpoint. What you learned may shrink, grow, merge, or
   re-order the remaining units.

## Output

Emit the plan as a short ordered list, one line per unit:

```text
Checkpoints (smallest first):
  1. [S] <unit> — exit: <check>
  2. [S] <unit> — exit: <check>            depends on: 1
  3. [M] <unit> — exit: <check>
  4. [L] <unit> — exit: <check>            depends on: 2, 3
```

## Stop conditions

Stop and re-plan when a unit grows to roughly twice its size estimate, when a
checkpoint cannot be reached without touching a later unit's scope, or when a
checkpoint's checks fail for a reason the unit did not introduce.

## Relationship to other references

- `change-budget.md` — supplies the size estimate and the overrun signal.
- `task-state.md` — each checkpoint is a resume point; record it there for
  multi-session work.
- `orchestration.md` — delegated units follow the same order, and a dependent
  unit never starts on an unreviewed predecessor.
- `blueprint.md` — stage 4 orders phases by this rule, after the phase-1
  vertical slice.
