# Evolving the methodology

Monozukuri applies Hansei and Kaizen to the software it helps build. The same
loop applies to the methodology itself: when a kind of failure keeps recurring
across tasks, that is a defect in the method, and it earns a proposed rule.

## Loop

```text
task → execute → verify → hansei → identify recurring failure → propose rule → evaluate → update skill/reference → kaizen
```

The `evaluate` step is where `evaluation.md` feeds in: a proposed rule is only
worth adopting if it improves the metrics without regressing the others.

## Proposed improvements are not auto-applied

A recurring failure produces a proposal, not an edit. The proposal is written
down and left for a human to accept or reject:

```text
PROPOSED IMPROVEMENT
Problem:  <recurring failure observed across tasks>
Rule:     <the rule that would prevent it>
Affects:  <which skill or reference file>
Status:   awaiting approval
```

No skill or reference file changes on the strength of a proposal alone.

## Where proposals live

A proposal is stored under `.monozukuri/decisions/` as a proposal — the same
directory that holds accepted decisions and ADRs (see `keep-why.md`). It is
promoted into an actual skill or reference edit only after a human accepts it.
Until then it stays a proposal on the record, visible but not in force.
