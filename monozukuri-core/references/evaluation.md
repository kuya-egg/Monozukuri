# Evaluating whether Monozukuri helps

Do not assume the methodology helps; measure it. Craftsmanship claims are
testable. Before trusting that Monozukuri produces better software, compare
assisted work against unassisted work on the same tasks and read the numbers.

## Method

Compare a baseline agent against a Monozukuri-assisted agent on the same fixed
task set:

- Fix a task set in advance: real repositories, real issues, varied in size
  and risk. Freeze it so results stay comparable across runs.
- Baseline arm: the agent works the tasks with no Monozukuri skills or
  references loaded.
- Assisted arm: the same agent, same model, same tasks, with Monozukuri
  active.
- Hold everything else constant: model, tools, time budget, and the person
  reviewing outcomes.
- Score each task against the metrics below. Report per-task results, not just
  aggregates, so regressions in one dimension are visible even when the mean
  improves.

## Metrics

- **Correctness** — does the change do what the task asked, verified against
  an executed check.
- **Regressions introduced** — previously passing behavior or tests broken by
  the change.
- **Diff size** — lines and files touched relative to the smallest change that
  solves the task.
- **Unnecessary changes** — edits in the diff not explained by the task.
- **Test quality** — do added tests exercise real behavior and fail when the
  behavior breaks.
- **Human interventions** — how often a person had to correct course, unblock,
  or redirect the agent.
- **Maintainability** — is the system as easy to change afterward, judged by
  interface clarity and coupling.
- **Completion rate** — fraction of tasks finished to the completion standard
  without being abandoned.
- **Token/cost efficiency** — tokens and wall-clock spent per task completed.

## Note

This is a documented procedure only. No benchmark runner ships in this repo. A
future optional implementation layer may add one; until then, teams run the
comparison by hand using their own task sets.

These metrics also gate methodology changes: see `evolution.md`, where a
proposed rule is only adopted if it improves them.
