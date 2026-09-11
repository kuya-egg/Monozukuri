# Logic-to-stack map template

Copy the block below into `.monozukuri/blueprint/03-logic-to-stack-map.md` and fill it in.

```markdown
# Logic → stack map — <project>

Every stage-1 business element maps to exactly one home in the stack.

| Business element | Type | Home in stack | Notes |
| --- | --- | --- | --- |
| <name> | <actor/domain/entity/rule/flow/state> | <module / table / service> | <caveat> |

## Orphans
Stage-1 elements with no home in the stack yet.

- <element> — <why unplaced / what is needed>

## Unused components
Stack pieces no business element uses.

- <component> — <keep and justify, or drop>
```
