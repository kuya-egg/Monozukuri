# Phase plan template

Copy the block below into `.monozukuri/blueprint/04-phase-plan.md` and fill it in.

`playbook` and `risk` are read by `definition-of-done.md` when the phase starts;
phase 1 must be a vertical slice through real boundaries.

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
<why this sequence: what each phase unblocks, where risk is front-loaded>
```
