# Change budget

AI agents tend to over-change: a small requirement turns into a broad
rewrite, and unrelated files drift into the diff. A change budget makes the
expected scope explicit before editing and creates a concrete stop point to
compare against afterward.

## Before CHANGE

Declare, tied to the plan:

- the expected file-count range (e.g. 3 to 6 files);
- the directories or modules expected to be touched.

Anything outside this declaration is a signal to re-assess, not a licence to
proceed.

## After CHANGE

Run `git diff --stat` yourself (there is no shipped script) and compare the
actual files and count against the declaration.

## Stop conditions

Stop, explain, and re-assess if the actual file count exceeds roughly twice
the declared range, or if the diff touches a subsystem that was not declared.
Re-assessment may re-route the work to a higher risk tier or split it into
smaller changes.

## Relationship to other references

- `guardrails.md` — general stop conditions; "the request requires an
  unrelated subsystem to change" applies directly here.
- `definition-of-done.md` — the `budget` checklist item ("Actual diff within
  declared change budget") in the feature DoD template gates on this.
