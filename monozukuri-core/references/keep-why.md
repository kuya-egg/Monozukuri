# Keep the why

Code records what the system does. It rarely records why it does it that
way, which alternatives were rejected, or what was traded away. That intent
is the expensive knowledge to lose. Preserve it alongside the change.

## The record

Capture six fields:

- **WHAT** changed — the concrete edit, in one or two sentences.
- **WHY** — the problem or need that forced the change.
- **WHY THIS APPROACH** — the reasoning that selected it.
- **ALTERNATIVES** considered — options looked at and not taken.
- **TRADEOFFS** — what got worse or harder in exchange.
- **CONSEQUENCES** — follow-on effects, and what to revisit later.

## Where it lives

- Lightweight changes: the commit message, PR description, or the evidence
  ledger. The six fields collapse to a short paragraph.
- Architectural decisions: an ADR created from `templates/adr.md`, stored
  under `.monozukuri/decisions/NNNN-title.md` (see the `.monozukuri/` note in
  `monozukuri-core/SKILL.md`).

## When an ADR is required

Write an ADR when the change:

- alters an interface that other code or teams depend on;
- changes dependency direction between modules;
- moves ownership of data from one component to another;
- shifts a security or trust boundary; or
- picks one of several viable architectures where the choice is hard to
  reverse.

See also `guardrails.md` — speculative architecture is a stop condition, and
an ADR is where a deliberate architectural choice gets justified.
