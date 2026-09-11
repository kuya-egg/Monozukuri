# Evidence ledger, Kaizen boundary, and completion standard

Detail for the "Evidence and completion" section of `SKILL.md`.

## Evidence ledger

Maintain a compact ledger in working notes or the final report. Do not create a
repository file unless requested.

```text
Observed
- files, behavior, commands, or runtime facts inspected

Decided
- chosen scope, design, and reasons

Changed
- files and externally visible behavior changed

Verified
- exact checks run and observed results

Residual risk
- unrun checks, known limitations, and follow-up boundaries
```

Verification claims must be traceable to an executed command, observed result,
or authoritative evidence. Never claim a check passed because it normally
passes or because the code looks correct.

## Prove it works

Two status words, and they are not interchangeable:

- **IMPLEMENTED** — the code is written and, as far as you can tell, complete.
  This is a claim about effort, not about behavior.
- **VERIFIED** — evidence proportional to the risk exists: a check was run, its
  result observed, and it demonstrates the intended behavior. Only VERIFIED work
  may be called done.

Do not report VERIFIED when you mean IMPLEMENTED. If the evidence is missing,
say so and name the check that is still owed.

Match the evidence type to what changed:

- **unit** — a function or module behaves correctly in isolation;
- **integration** — components work together across a real boundary;
- **runtime** — the behavior was exercised in a running process and observed;
- **browser** — the user-facing behavior was checked in a real browser;
- **benchmark** — a performance claim is backed by a measured comparison;
- **security** — an authorization, input, or data boundary was probed;
- **migration** — a schema or data change was applied and rolled back on
  representative data;
- **smoke** — the critical path still works end to end after the change.

## Kaizen boundary

An adjacent improvement is permitted only when it is:

1. directly related to the task;
2. low risk;
3. independently verifiable;
4. smaller than the primary change; and
5. unlikely to complicate review or rollback.

Otherwise record it as a concrete follow-up instead. "Leave it better" is not
permission for scope creep.

## Completion standard

Work is complete when:

1. the requested behavior is implemented or the question is answered;
2. the change is as small and clear as reasonably possible;
3. relevant failure modes and security boundaries were considered;
4. meaningful verification was performed and recorded;
5. the final diff contains no accidental or unexplained work;
6. documentation and observability match the new behavior; and
7. limitations, unrun checks, and residual risk are stated plainly.

The standard is not perfection. It is honest, durable craftsmanship.
