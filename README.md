<p align="center">
  <img src="assets/monozukuri-banner.png" alt="A Japanese workshop table with engineering plans and subtle circuit traces, representing software craftsmanship" width="100%" />
</p>

# monozukuri

`monozukuri` is a complete family of Agent Skills for building and maintaining
software as a craft rather than disposable output.

![Installing the monozukuri skill suite](assets/demo.svg)

It combines Japanese-inspired quality principles with practical software
engineering: YAGNI, KISS, clear design, defect prevention, testing, security,
observability, documentation, and evidence-based review.

It is designed for both greenfield construction and existing codebases. The
target is maintainable software—not theoretically perfect software: code that
future maintainers can understand, verify, operate, and change safely.

The skill is deliberately **Japanese-inspired, not Japanese-prescriptive**.
It does not claim that developers in one country share a single engineering
style. The useful ideas are treated as metaphors for disciplined software work.

## The craftsmanship system

The umbrella skill applies this loop:

```text
Observe → Understand → Plan → Craft → Verify → Reflect → Kaizen
```

The repository also contains focused, independently installable skills:

| Skill | Focus |
| --- | --- |
| `monozukuri-router` | Classify the task, assess risk, compose the skill sequence and Definition of Done |
| `monozukuri-blueprint` | Author a greenfield build blueprint in four gated stages |
| `nemawashi` | Clarify intent and prepare the design |
| `genchi-genbutsu` | Inspect the real repository and environment |
| `kanso` | Choose simple, durable designs |
| `kata` | Implement small, integrated vertical slices |
| `jidoka` | Debug failures and find root causes |
| `poka-yoke` | Prevent mistakes and design meaningful tests |
| `kodawari` | Review details that carry quality |
| `andon` | Make health and failure visible |
| `kaizen` | Improve code incrementally |
| `hansei` | Learn from failures and completed work |
| `shukka` | Release, migrate, and hand off responsibly |

The suite is `monozukuri-core` (the umbrella), `monozukuri-router`,
`monozukuri-blueprint`, and the 11 focused skills above—14 skills in total.

### Router and interaction modes

`monozukuri-router` is step 0 of any consequential task. Before engineering
work starts, it classifies the task, assesses its risk tier, chooses the
interaction mode, and emits a plan header naming the Monozukuri sequence and
the Definition of Done. It does no engineering work itself; it hands off to
the composed sequence. Skip it for trivial one-line edits, pure questions, and
throwaway scripts.

Two interaction modes:

- `execute` (default) — apply the methodology and do the work. Incident or
  "production is down" language forces this mode.
- `sensei` — chosen when the user asks to be **guided**, **taught**,
  **reviewed** (a review of their own approach), or **set up** (Monozukuri
  configured for a project). The methodology is unchanged; only how the agent
  talks to the user changes.

### Greenfield blueprint

`monozukuri-blueprint` turns a framed idea into a build plan before any
implementation starts. It runs the `greenfield` playbook and produces four
artifacts in order, each gated on explicit user approval before the next
stage begins:

1. **Business logic** — domains, actors, entities, rules, flows, and
   invariants, in plain language, with no technology choices.
2. **Tech stack** — language, framework, datastores, infra, and key
   libraries, each justified by a stage-1 need.
3. **Logic-to-stack map** — every stage-1 element mapped to exactly one home
   in the stack, with orphans called out explicitly.
4. **Phase plan** — ordered phases, each naming its `playbook`, `risk` tier,
   affected areas, and prerequisites; phase 1 is a vertical slice.

The artifacts live under `.monozukuri/blueprint/`. Once the phase plan is
approved, each phase is executed under the playbook and risk tier it already
names—the router's classify step is already answered, so implementation goes
straight to that playbook's Definition of Done.

### Reference material

The methodology is documented under `monozukuri-core/references/`:

| File | Contents |
| --- | --- |
| `constitution.md` | Non-negotiable principles the skills implement |
| `playbooks.md` | Default skill compositions per task type, plus modifiers |
| `definition-of-done.md` | Per-task DoD templates derived from each playbook |
| `change-budget.md` | Declaring expected file scope before editing |
| `keep-why.md` | Preserving intent, rejected alternatives, and trade-offs |
| `sensei.md` | The sensei interaction mode: guide, teach, review, set up |
| `orchestration.md` | Roles and coordination guidance for multi-agent hosts |
| `task-state.md` | Small resumable state record for long, multi-session work |
| `evaluation.md` | Measuring whether the methodology actually helps |
| `evolution.md` | Applying Hansei and Kaizen to the methodology itself |
| `schema.md` | The `monozukuri/v1` machine-readable YAML block format |
| `blueprint.md` | The four-stage greenfield blueprint gate protocol |
| `templates/` | Fill-in templates: `adr`, `specification`, `verification`, `incident`, `reflection`, and `blueprint/` |

### Compatibility

Existing skill names and paths are a stable public API. Monozukuri 2.0 is
additive: one new skill (`monozukuri-router`) and new reference material only.

## What it changes

When active, an agent should:

- clarify the problem before consequential work;
- inspect the real repository, environment, and constraints before changing it;
- make the smallest complete change;
- prevent invalid states and make failures visible;
- verify behavior instead of trusting generated code;
- avoid speculative abstractions, fake tests, and unrelated rewrites; and
- leave clear evidence, documentation, and residual-risk notes;
- build from scratch through the same craftsmanship loop as maintenance work.

## Install

### With the `skills` CLI

Umbrella skill only (`monozukuri-core`):

```bash
npx skills add kuya-egg/Monozukuri --skill monozukuri-core
```

Everything (umbrella plus all focused skills):

```bash
npx skills add kuya-egg/Monozukuri --skill '*'
```

One focused skill when you need a narrower workflow:

```bash
npx skills add kuya-egg/Monozukuri --skill nemawashi
npx skills add kuya-egg/Monozukuri --skill kodawari
```

The task router on its own:

```bash
npx skills add kuya-egg/Monozukuri --skill monozukuri-router
```

The greenfield blueprint skill on its own:

```bash
npx skills add kuya-egg/Monozukuri --skill monozukuri-blueprint
```

The focused skills can also be used as a sequence. A typical greenfield path
is `nemawashi` → `genchi-genbutsu` → `kanso` → `kata` → `poka-yoke` →
`kodawari` → `andon` → `shukka`, followed by `hansei` and `kaizen` as the
system learns.

Each skill is a top-level `<name>/SKILL.md` directory, in the Agent Skills
format, discoverable by the `skills` CLI and compatible with any agent that
supports the format.

### As a Claude Code plugin

The repository is also a self-contained plugin marketplace. From Claude Code:

```text
/plugin marketplace add kuya-egg/Monozukuri
/plugin install monozukuri@monozukuri
```

This installs all 14 skills at once, namespaced under the `monozukuri` plugin.
`.claude-plugin/plugin.json` and `.claude-plugin/marketplace.json` define it;
`claude plugin validate .` passes.

## Use explicitly

```text
Use $monozukuri-core to review this change for correctness, maintainability,
failure handling, and AI-generated complexity.
```

## Name

The suite is `monozukuri`—the Japanese idea of craftsmanship and the art of
making things. Its umbrella skill is `monozukuri-core` and its task router is
`monozukuri-router`; the eleven focused skills carry their own vocabulary
names (`kaizen`, `jidoka`, `poka-yoke`, …).

## Sources and inspiration

The principles were synthesized from software-craft writing about kaizen,
jidoka, poka-yoke, nemawashi, documentation, incremental delivery, and
maintainability, including:

- [Japanese Quality Principles applied to Software](https://frappe.io/blog/engineering/japanese-quality-principles-applied-to-software)
- [From Kaizen to Wabi-Sabi](https://dev.to/dev_tips/from-kaizen-to-wabi-sabi-what-japanese-developers-taught-me-about-writing-code-that-lasts-mjl)
- [The Japanese Way of Coding](https://kenrickvaz.com/2025/07/30/the-japanese-way-of-coding/)
- [The Samurai Engineer](https://balaaagi.in/posts/the-samurai-engineer/)
- [The 56 Laws of Software Engineering: A Japanese Translation](https://zenn.dev/inspector/articles/laws-of-software-engineering-56-ja?locale=en)

These sources are inspiration, not authority. The skill favors practices that
can be explained, tested, and adapted to the actual project.
