# Orchestration

Monozukuri defines the roles and the guidance for coordinating work across
them. The host provides the execution mechanism — how agents are spawned,
isolated, and joined. No agents ship in this suite; a host maps these roles
onto whatever concurrency it offers, or onto a single operator working the
roles in sequence.

## Roles

- **architect** — owns scope, decomposition, and the shape of the change;
  approves the approach before implementation starts.
- **implementer** — makes the smallest correct change that satisfies the
  approved approach.
- **tester** — designs and runs the checks that prove behavior, independent of
  the implementer's reasoning.
- **reviewer** — inspects the finished change against requirements, code,
  tests, architecture, and evidence.
- **security** — threat-models the change surface and checks trust
  boundaries, secrets, inputs, and dependencies.

## When to parallelize

- Independent work streams that touch disjoint parts of the system.
- Changes with high blast radius, where a second stream de-risks the primary.
- Adversarial review of high or critical changes, run alongside the work.

Do not use multiple agents to change one line.

## Worktree isolation

Give each parallel stream its own worktree so edits never collide. Then
integrate: merge the streams one at a time, resolve conflicts, run full
verification on the combined result, and only then merge to the mainline.

## Independent review

For high or critical risk, a reviewer inspects the requirements, the code, the
tests, the architecture, and the evidence directly. The reviewer confirms each
against the artifact rather than accepting the implementer's reasoning that it
holds.
