# Handoff bridge templates

Use the smallest template that can restore the task. Remove empty sections.

## Source-backed delta

Use when authoritative sources already hold most durable context.

```markdown
# <task title> handoff delta

- Prepared: <ISO-8601 timestamp>
- Old task ID: <id, if available>
- Workspace: <path or saved project>

## Authoritative sources

- <source>: <what it owns>

## Missing delta

- [current-task-only] <discussion position or reasoning not yet durable>
- [unverified] <claim still requiring evidence>

## Open loops

- <unfinished question or promised action>

## Boundaries and calibration

- <only what the authoritative sources do not already state>

## Next entry

- <exact point from which to continue>
```

If there is no meaningful delta, pass only the source list plus next entry. Do not duplicate source contents.

## Conversation-backed handoff document

Use when no maintained source of truth exists or most state remains conversation-only.

```markdown
# <task title> handoff

- Prepared: <ISO-8601 timestamp>
- Old task ID: <id, if available>
- Workspace: <path or saved project>
- Purpose: <one sentence>

## Established facts and evidence

- <fact>: <file, output, user confirmation, or other evidence>

## Decisions and rejected options

- <decision and reason>
- <rejected option and reason>

## Completed results

- <artifact or verified outcome>

## Current position

- <where work or discussion stopped>

## Open loops

- [unverified] <question, blocker, missing evidence, or unfinished action>

## Boundaries and collaboration

- <privacy, write, Git, external-action, and communication limits>

## Next entry

- <first concrete step for the successor>
```

## Common checks

- Prefer authoritative source paths over copied prose.
- Preserve rejected options only when revisiting them would waste work or change the next decision.
- Do not include raw transcripts, credentials, unrelated private stories, hidden answer keys, or speculative labels.
- Flag source conflicts rather than silently choosing one.
- A bridge is not proof that live state is current.
