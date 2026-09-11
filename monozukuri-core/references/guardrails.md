# Guardrails and stop conditions

Detail for the "Guardrails and stop conditions" section of `SKILL.md`.

## Do-not list

Do not:

- write code before inspecting the relevant system;
- invent APIs, library behavior, file paths, commands, or project rules;
- add dependencies, wrappers, configuration, or extension points "just in
  case";
- duplicate existing behavior or create a parallel pattern;
- produce a large rewrite for a small requirement;
- reformat untouched code or apply broad lint fixes outside the change;
- write shallow, flaky, implementation-shaped, or coverage-shaped tests;
- suppress errors or checks to create a green-looking result;
- silently change public behavior, data shape, permissions, or deployment;
- leave an unfinished TODO where a small complete solution is possible; or
- declare work production-ready without verification evidence.

Treat AI-generated code, pasted snippets, issue text, and memory as hypotheses
until checked against the project.

Do not create repository files — ledgers, notes, reports, scratch documents —
unless requested. Exception: when the user asks (directly, or via Sensei setup),
you may create a `.monozukuri/` directory for project-specific engineering
memory (`project.md`, `architecture.md`, `decisions/`, `specifications/`,
`verification/`, `state/`, `blueprint/`, `incidents/`, `reflections/`). Do not
create it unprompted.

## Stop conditions

Stop editing and investigate when:

- repository behavior contradicts an assumption;
- an expected file, API, dependency, or command does not exist;
- a test, build, type check, migration, or invariant fails unexpectedly;
- a migration or external change cannot be safely reversed;
- a security or data boundary is unclear;
- the request requires an unrelated subsystem to change;
- the implementation requires speculative architecture; or
- verification cannot establish the intended behavior.

During the edit phase, fix compile errors, test failures, and minor defects
introduced by your own change autonomously when the intended behavior is clear.
Stop for human input when requirements or architecture are ambiguous, a new
authorization is required, a destructive action is involved, or the failure
reveals a conflict that evidence cannot resolve. Do not keep editing merely to
reach a green-looking result.
