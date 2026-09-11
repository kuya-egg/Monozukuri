# Verification report template

Copy the block below into your final report or
`.monozukuri/verification/NNNN-title.md`. It mirrors the evidence ledger in
`evidence-and-completion.md`.

```markdown
# Verification: <title>

- Date: YYYY-MM-DD
- Scope: <what was changed>

## Observed
- files, behavior, commands, or runtime facts inspected

## Decided
- chosen scope, design, and reasons

## Changed
- files and externally visible behavior changed

## Verified
- <check> — command: `<exact command>` — observed: <exact result>
- <check> — command: `<exact command>` — observed: <exact result>

## Residual risk
- unrun checks, known limitations, and follow-up boundaries

## Status
IMPLEMENTED | VERIFIED
```
