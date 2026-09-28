# Phase plan template

Copy the block below into `.monozukuri/blueprint/04-phase-plan.md` and fill it in.

`playbook` and `risk` are read by `definition-of-done.md` when the phase starts;
phase 1 must be a vertical slice through real boundaries; later phases run
smallest first within prereq order (see `checkpoints.md`).

```markdown
# Phase plan — <project>

## Phase 1: <name>
- id: 1
- intent: <one line — the outcome this phase delivers>
- affected: <areas / files>
- prereqs: <phase ids + external dependencies, or "none">
- playbook: <feature|refactor|migration|release>
- risk: <low|medium|high|critical>

## Phase 2: <name>
- id: 2
- intent: <one line>
- affected: <areas / files>
- prereqs: <phase ids + external>
- playbook: <feature|refactor|migration|release>
- risk: <low|medium|high|critical>

## Phase order rationale
<why this sequence: what each phase unblocks, why small phases come before large ones, where a small probe retires a big unknown early>
```
