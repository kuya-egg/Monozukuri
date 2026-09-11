# Business logic template

Copy the block below into `.monozukuri/blueprint/01-business-logic.md` and fill it in.

```markdown
# Business logic — <project>

## Overview
<what the system is for, in the language of the people who use it>

## Actors and roles
- <actor> — <what they want, what they may do>

## Domains
- <domain> — <the slice of the problem it owns>

## Entities and relationships
- <entity> — <what it represents; key attributes>
- <entity> <relates to> <entity> (<one-to-many / owns / references>)

## Business rules
- <rule that must always hold, stated without reference to any mechanism>

## Primary flows
1. <flow name>: <actor> does <step> -> <step> -> <outcome>

## State transitions
- <entity>: <state> --<event>--> <state>

## Invariants
- <condition that is true before and after every operation>

## Edge cases
- <unusual input or situation> -> <expected handling>

## Open questions
- <unresolved question about the problem, not the solution>
```
