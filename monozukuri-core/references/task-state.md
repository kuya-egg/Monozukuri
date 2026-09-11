# Task state

For long, multi-session work, a task carries a small state record so it can
resume without restarting. The record names the playbook position: which
phases are done, which is active, and where the evidence lives.

## The state file

Store the record at `.monozukuri/state/<task>.md` — markdown notes for
context, plus one `task-state` block. The notes are for humans; the block is
the machine-readable position.

```yaml
schema: monozukuri/v1
type: task-state
status: in_progress
phases:
  nemawashi: complete
  genchi-genbutsu: complete
  kanso: complete
  kata: in_progress
  poka-yoke: pending
  kodawari: pending
  shukka: pending
evidence: .monozukuri/verification/<task>.md
```

## Resuming

1. Read the state file for the task.
2. Determine which phases are already `complete`.
3. Continue from the first phase that is not `complete`, using the notes and
   the linked evidence for context.
