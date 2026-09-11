# Monozukuri playbooks

A playbook is a *default* composition of the eleven focused skills for a task
type. The router adapts it to the task in front of it; it is not a rigid
pipeline. A skill can be skipped, repeated, or reordered when the work calls
for it.
Modifiers layer extra steps and definition-of-done items onto any playbook.

## How to read a playbook

`sequence` lists the skills in the order they normally engage, from framing the
work through to shipping and reflection. Earlier entries set up context that
later entries depend on, so moving a skill earlier usually means doing less of
it. `kaizen` appears last where it appears at all, and runs only within the
adjacent-improvement boundary defined in `evidence-and-completion.md` — anything
larger is recorded as a follow-up, not done inline.

## Playbooks

### feature

Use when building new behavior into an existing system, greenfield or not. The
router trims the front of the sequence when alignment already exists and leans
harder on `kata` and `poka-yoke` as risk rises. Stop if the change surface is
not understood well enough to name its failure modes.

```yaml
schema: monozukuri/v1
type: playbook
name: feature
sequence: [nemawashi, genchi-genbutsu, kanso, kata, poka-yoke, kodawari, andon, shukka, hansei, kaizen]
```

### bugfix

Use when a defect has a known or reproducible symptom. `genchi-genbutsu` and
`jidoka` dominate: reproduce against real code, then lock the behavior with a
failing test before touching the fix. The rest of the sequence compresses for
low-risk fixes.

```yaml
schema: monozukuri/v1
type: playbook
name: bugfix
sequence: [genchi-genbutsu, jidoka, poka-yoke, kata, kodawari, shukka, hansei]
```

### refactor

Use when changing structure without changing observable behavior. The router
weights `kanso` and `kata` heavily and keeps each step small and reversible.
Stop if behavior tests do not exist yet — establish them before changing
structure.

```yaml
schema: monozukuri/v1
type: playbook
name: refactor
sequence: [genchi-genbutsu, poka-yoke, kanso, kata, kodawari, hansei, kaizen]
```

### incident

Use when a live system is degraded or down. `andon` leads: make the state
visible, then mitigate. Mitigate first; root-cause analysis comes after the
system is stable.

```yaml
schema: monozukuri/v1
type: playbook
name: incident
sequence: [andon, genchi-genbutsu, jidoka, poka-yoke, shukka, hansei, kaizen]
```

### migration

Use when moving data, schemas, or infrastructure from one state to another. The
router front-loads `nemawashi` and `genchi-genbutsu` and expands `poka-yoke`
into staged cutover and verification. Stop if no rollback path is identified
before CHANGE.

```yaml
schema: monozukuri/v1
type: playbook
name: migration
sequence: [nemawashi, genchi-genbutsu, kanso, poka-yoke, kata, kodawari, andon, shukka, hansei]
```

### release

Use when packaging and shipping already-built work. `genchi-genbutsu` confirms
what is actually going out, `poka-yoke` and `andon` guard the rollout, and
`shukka` runs the release itself. Stop if the build under release does not match
verified source.

```yaml
schema: monozukuri/v1
type: playbook
name: release
sequence: [genchi-genbutsu, poka-yoke, kodawari, andon, shukka, hansei]
```

### investigation

Use when the task is to answer a question, not change the system. `nemawashi`
frames what is being asked, `genchi-genbutsu` and `jidoka` gather real evidence,
and `hansei` records the finding. Stop and reframe if evidence keeps
contradicting the question as posed.

```yaml
schema: monozukuri/v1
type: playbook
name: investigation
sequence: [nemawashi, genchi-genbutsu, jidoka, hansei]
```

### docs

Use when the deliverable is documentation. `genchi-genbutsu` grounds the text in
real behavior, `kanso` keeps it minimal, and `kodawari` polishes for the
reader. Stop if the documented behavior cannot be verified against the code.

```yaml
schema: monozukuri/v1
type: playbook
name: docs
sequence: [genchi-genbutsu, kanso, kodawari]
```

## Modifiers

### security

Layers threat modeling and security verification onto the base playbook.

```yaml
schema: monozukuri/v1
type: modifier
name: security
inject:
  after_kanso: [threat-model]
  poka-yoke: [security test cases]
  kodawari: [security review]
  andon: [security observability]
  shukka: [security verification]
```

### performance

Layers measurement and regression guarding onto the base playbook.

```yaml
schema: monozukuri/v1
type: modifier
name: performance
inject:
  genchi-genbutsu: [profiling, baseline measurement]
  kanso: [performance-aware design]
  poka-yoke: [benchmark regression test]
  kodawari: [performance review]
```
