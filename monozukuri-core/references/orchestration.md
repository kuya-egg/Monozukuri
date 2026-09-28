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

The primary agent is always the **architect**: it owns decomposition,
delegation, integration, and every user-facing decision. Delegated work fills
the other roles; the primary agent never hands off integration or the final
call.

## Using the host's native subagents

Delegate through the harness's own subagent tool — never a hand-rolled runner,
never a shell loop that launches agent CLIs.

| Host | Mechanism |
| --- | --- |
| Claude Code | The `Agent` tool. Explorer briefs use `subagent_type: Explore` (read-only); implementer, tester, reviewer, and security briefs use a general-purpose agent. Parallel streams get `isolation: worktree`, and independent dispatches go out in a single message so they run concurrently. |
| Other hosts | Whatever native subagent or task feature the host exposes, mapped onto the same roles. |
| No subagent support | The primary agent works each role itself, in order, keeping the same briefs and review gates. |

## Deciding: inline or delegated

Default to working directly. Delegate to a fresh subagent (when the host
offers one) only when the task earns the overhead — spinning one up to change
one line is waste, not discipline.

| Task shape | Execution |
| --- | --- |
| Trivial edit, one file, one concern | inline — no subagent |
| Investigating an unfamiliar subsystem before planning | fresh subagent, explorer/architect brief |
| One step of an already-approved plan, independently verifiable | fresh subagent, implementer brief |
| Two or more independent streams touching disjoint files | parallel subagents, one per stream, each in its own worktree |
| Sequential steps sharing state or files | sequential subagents, one per step, not parallel |
| Independent review of a high- or critical-risk change | fresh subagent, reviewer/security brief, run alongside or after the work |
| Integration, synthesis, or any user-facing decision | the primary agent — never delegated |

## Dispatch contract

A delegated task gets everything it needs to work without guessing back at
the conversation:

- the objective, in one or two sentences;
- the files or subsystem in scope, and what is explicitly out of scope;
- constraints: conventions to follow, APIs not to invent, the budget from
  `change-budget.md`;
- the acceptance criteria it owns, from the composed Definition of Done;
- the verification it must run before reporting done.

Require evidence back (Observed / Decided / Changed / Verified / Residual
risk — see `evidence-and-completion.md`), not a bare "done". A report with no
verification evidence is incomplete: re-dispatch with the gap named, or finish
it directly.

## Fresh-context rule

One focused task gets one fresh subagent. Do not reuse a subagent's context
for a second, unrelated task — carried-over context is exactly how scope
creeps and stale assumptions outlive the code that justified them. A failed
task gets a *new* subagent for the fix, briefed with the review finding, not
a continuation of the context that produced the failure — unless the fix is a
one-line, unambiguous correction.

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

## Review gate and corrective loop

A delegated step is not done when the subagent stops talking — it is done
when its evidence passes review.

1. Collect the result and its evidence.
2. Check both against the acceptance criteria and the relevant Monozukuri
   skill (`poka-yoke` for tests, `kodawari` for a diff review, `andon` for
   observability, `shukka` for release readiness).
3. Pass: integrate, then move to the next step.
4. Fail: dispatch a fresh corrective subagent, briefed with the specific gap —
   see the fresh-context rule.
5. Never let a dependent step start on an unreviewed or failing predecessor.
