# monozukuri/v1 — machine-readable schema

## Purpose

Markdown is canonical. The prose in each skill and reference file is the source
of truth. `monozukuri/v1` YAML blocks are consumable metadata embedded directly
in that markdown as ` ```yaml ` fences, so a router or checker can read the
suite without parsing prose. The blocks are a projection of the prose and must
not drift from it: when the text and a block disagree, the text wins and the
block is a bug. Every block begins with the two keys `schema: monozukuri/v1`
and `type: <block-type>`.

## Block types

Five block types are defined in v1. Each subsection lists required fields and a
minimal example. Examples below show only the payload; a real block also carries
the `schema` and `type` keys.

### risk-tiers

Names the four risk tiers and the process weight and human gate each one
requires. Exactly one `risk-tiers` block exists in the suite; it lives in
`constitution.md`.

| Field   | Type | Notes |
|---------|------|-------|
| `tiers` | map  | keys `low`, `medium`, `high`, `critical`; each value is a map with `process` and `human_gate` |

```yaml
tiers:
  low:      { process: fast-path, human_gate: none }
  critical: { process: controlled, human_gate: explicit-approval }
```

### playbook

An ordered route through the suite for a class of work.

| Field      | Type            | Notes |
|------------|-----------------|-------|
| `name`     | string          | unique across all playbook blocks |
| `sequence` | list of strings | each entry is one of the 12 skill names |

```yaml
name: feature
sequence: [nemawashi, genchi-genbutsu, kanso, kata, jidoka, kodawari, shukka]
```

### modifier

A named overlay that injects extra steps into a playbook without redefining it.

| Field    | Type | Notes |
|----------|------|-------|
| `name`   | string | unique across all modifier blocks |
| `inject` | map    | keys are skill names or `after_<skill>`; values are lists of injected step labels |

```yaml
name: security-sensitive
inject:
  poka-yoke: ["threat-model the change surface"]
  after_jidoka: ["run dependency and secret scans"]
```

### dod-template

A definition-of-done checklist bound to one playbook.

| Field       | Type            | Notes |
|-------------|-----------------|-------|
| `playbook`  | string          | must match a `playbook.name` |
| `checklist` | list of maps    | each item has `id`, `text`, `gate`, `min_risk` |

`gate` is one of `discover`, `change`, `prove`. `min_risk` is one of `low`,
`medium`, `high`, `critical`; the item applies only at that tier or higher.

```yaml
playbook: feature
checklist:
  - { id: repro,   text: "Behavior reproduced against real code", gate: discover, min_risk: low }
  - { id: arch-ok, text: "Architecture approach approved",        gate: change,   min_risk: high }
```

### task-state

The current position of one task moving through a playbook.

| Field      | Type   | Notes |
|------------|--------|-------|
| `status`   | string | one of `not_started`, `in_progress`, `blocked`, `complete` |
| `phases`   | map    | skill name → one of the same four status values |
| `evidence` | path   | optional; path to the evidence ledger or report |

```yaml
status: in_progress
phases:
  genchi-genbutsu: complete
  kanso: in_progress
evidence: .superpowers/notes/task-3-evidence.md
```

## Versioning and public API

> Existing skill names and paths are public API. `monozukuri/v1` blocks remain
> backward-compatible within v1; a breaking change becomes `monozukuri/v2`.

## Not in v1

- No separate `.yaml` or `.json` files; blocks live inside the markdown they
  describe.
- No block types beyond the five above.
- Add a type only when a real consumer needs it, not in anticipation.
