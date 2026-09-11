# Tech stack template

Copy the block below into `.monozukuri/blueprint/02-tech-stack.md` and fill it in.

```markdown
# Tech stack — <project>

## Summary

| Component | Choice | Why — over what |
| --- | --- | --- |
| <e.g. runtime> | <choice> | <reason — beaten alternative> |

## Runtime
<language + version, execution model, why it fits the workload>

## Framework
<web / app framework and version, or "none — plain <runtime>", and why>

## Data
<datastore(s), schema/migration approach, caching, why>

## Infra and deploy
<where it runs, how it ships, CI, environments>

## Key libraries
- <library> — <the job it does, why not hand-rolled>

## Rejected alternatives
- <option> — <why it lost>

## ADRs raised
- `.monozukuri/decisions/NNNN-<slug>.md` — <decision in one line>
```
