# The greenfield blueprint

Four gated artifacts turn a business idea into an ordered, executable
build plan: business logic, then tech stack, then a logic-to-stack map,
then a phase plan. Each stage is presented and approved before the next
begins. The `monozukuri-blueprint` skill runs the workflow; this file
explains what a strong artifact at each stage looks like and how to
judge one.

## The gate protocol

After producing each artifact:

1. Present it in full.
2. Name what the next stage will do with it.
3. Wait for explicit approval before starting that stage.

If a later stage exposes a gap in an earlier artifact — a missing
entity, an undriven component, an unstated rule — stop, return to the
earlier stage, fix it, and re-present the changed artifact for approval
before resuming. Never start a stage that has not been approved.

## Stage 1 — business logic

Template: `templates/blueprint/business-logic.md`. Fill it into
`.monozukuri/blueprint/01-business-logic.md`.

A strong artifact has:

- named business rules and invariants, each stated without reference to
  any mechanism;
- concrete edge cases with their expected handling;
- entities, actors, flows, and state transitions in the users' language;
- no technology — no framework, datastore, protocol, or library.

Vague vs concrete:

```text
vague:    "Orders are validated before they are accepted."
concrete: "An order with zero line items is rejected. An order whose
           total exceeds the customer's credit limit is held for review."
```

Refuse to name a framework or datastore at this stage. If asked "what
database", the answer is "that is stage 2 — first we finish the rules".

## Stage 2 — tech stack

Template: `templates/blueprint/tech-stack.md`. Fill it into
`.monozukuri/blueprint/02-tech-stack.md`.

Every component is derived from an actual stage-1 need, not from habit
or resume-building. `kanso` applies: a component with no stage-1 driver
is cut, not justified.

Every choice records "why this — over what": the alternative that was
seriously considered and the reason it lost.

A choice is **significant** when reversing it later would touch many
modules or the data model. Significant choices get an ADR via
`templates/adr.md`, stored under `.monozukuri/decisions/NNNN-title.md`
(see `keep-why.md`). A choice that could be swapped inside one module
with a local edit is not significant — record it in the artifact and
move on.

## Stage 3 — logic to stack map

Template: `templates/blueprint/logic-to-stack-map.md`. Fill it into
`.monozukuri/blueprint/03-logic-to-stack-map.md`.

One row per stage-1 element (actor, domain, entity, rule, flow, state),
each mapped to exactly one home in the stack.

Read the completed map two ways:

- **Orphan** — a stage-1 element with no home. The stack is incomplete;
  return to stage 2 and add the missing piece.
- **Unused component** — a stack piece no element uses. It is waste; cut
  it, or justify it explicitly in the artifact.

If building the map reveals a rule or entity that stage 1 never named,
go back to stage 1, add it, and re-approve.

## Stage 4 — phase plan

Template: `templates/blueprint/phase-plan.md`. Fill it into
`.monozukuri/blueprint/04-phase-plan.md`.

A **phase** is a shippable increment — something that delivers a usable
outcome through real boundaries. A **task** is work inside a phase and
is **not** listed in the plan; the task list for a phase is written when
that phase starts.

Per phase, record:

- `intent` — one line, the outcome the phase delivers;
- `affected` — the areas or files it touches;
- `prereqs` — earlier phase ids and named external dependencies, or
  "none";
- `playbook` — `feature | bugfix | refactor | migration | release |
  greenfield-subsystem`;
- `risk` — `low | medium | high | critical`.

Validate that every `prereqs` entry is either an earlier phase id or a
named external dependency — never a forward reference, never vague.

Phase 1 is a vertical slice: it cuts through every layer of the stack on
one thin path, proving the boundaries work before breadth is added.

## Executing the plan

Phases run one at a time. When a phase starts, its `playbook` and `risk`
render its Definition of Done from `definition-of-done.md` (playbook
checklist plus every item at or below the phase's risk tier plus active
modifier additions).

The detailed task list for that phase is produced then, not now, by
`spec-driven-implementation` / `writing-plans`. The blueprint sets
direction and order; the per-phase plan sets the steps.

## Project memory

The blueprint reads and updates `.monozukuri/` memory:

- `.monozukuri/project.md` — what the project is, its actors, and its
  current phase;
- `.monozukuri/architecture.md` — the stack and the logic-to-stack map,
  kept current as phases land;
- `.monozukuri/decisions/` — ADRs for every significant stage-2 choice.

Each stage updates the relevant memory file as part of its approval, so
later work reads a current picture rather than reconstructing it.
