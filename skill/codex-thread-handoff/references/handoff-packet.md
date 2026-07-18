# Incremental handoff packet

Use this reference during Prepare only. Keep the packet short: it supplements durable sources and direct old-task inspection; it replaces neither.

## Contents

- Preparation prompt for the old task
- Packet template and checks
- Shadow startup prompt

## Preparation prompt for the old task

Send this through task tools when a separate controller is orchestrating the handoff. Resolve every placeholder before sending it.

```text
Use $codex-thread-handoff in Prepare only mode.

Old task ID: <exact-old-task-id>
Old task title: <current-title>
Saved project or workspace: <project-or-workspace>
Authorized governing context: <ordered-list>
Authorized source-of-truth sources: <ordered-list-with-purpose>
History cutoff: <timestamp-or-last-turn>

Compare the authorized durable sources with this task's recent context and return a short incremental handoff packet. Mark state-bearing items as durable-source, current-task-reasoning, or unverified. Include decisions, rejected options and reasons, open loops, unfinished commitments, collaboration calibration, hard boundaries, and the exact next entry only when they are not already recoverable from the durable sources.

Do not include a raw transcript, credentials, unrelated private material, or hidden validation answers. Do not write files, mutate Git, create or rename tasks, archive anything, create automations, or contact third parties. After returning the packet, stop normal work in this task and remain available only as a frozen read-only source.
```

## Packet template

Remove empty sections.

```markdown
# <task title> incremental handoff

- Prepared: <ISO-8601 timestamp with timezone>
- Old task ID: <exact ID>
- History cutoff: <timestamp or last included turn>
- Saved project or workspace: <name or authorized path>
- Purpose: <one sentence>

## Authorized durable sources

- <source>: <what it governs or proves>

## Role and scope

- [durable-source] <stable responsibility or boundary>
- [current-task-reasoning] <recent scope refinement not yet durable>
- [unverified] <claim that still requires evidence>

## Current position

- [durable-source] <current state already recorded>
- [current-task-reasoning] <where the discussion actually stopped>

## Recent decisions and rejected options

- [current-task-reasoning] <decision and rationale>
- [current-task-reasoning] <rejected option and reason>

## Open loops and unfinished commitments

- [unverified] <question, blocker, promised action, or missing evidence>

## Collaboration calibration

- <how to answer, challenge assumptions, and communicate uncertainty>

## Hard boundaries

- <authorized read scope>
- <file-write, Git, private-data, automation, and external-action limits>

## Exact next entry

- <the next question, artifact, or decision to continue from>
```

## Packet checks

- Use the exact old task ID; a title is not a stable substitute.
- Point to authorized sources rather than copying them into the packet.
- Preserve rejected options and reasons when revisiting them would waste work or change the next decision.
- Flag conflicts between the current task and durable sources instead of silently choosing one.
- State evidence gaps plainly. Do not upgrade `current-task-reasoning` or `unverified` into fact.
- Omit hidden expected answers. The evaluator creates them only after the candidate's first recovery response.

## Shadow startup prompt

Use this only after explicit Shadow validation authorization and after creating a clean, non-forked task.

```text
Use $codex-thread-handoff in Shadow validation mode. This is read-only recovery.

Read, in order:
1. Authorized governing context: <ordered-list>
2. Authorized current source-of-truth sources: <ordered-list>
3. The incremental packet below
4. Old task ID <exact-old-task-id>, through the complete available turn index or compact summaries, followed by detailed inspection of the current tail and all decision-, rejection-, open-loop-, commitment-, calibration-, and next-entry turns

History cutoff: <timestamp-or-last-turn>

Report every source actually read, full index/summary/page coverage, detailed turn coverage, and anything unread or unavailable. Distinguish facts, inferences, and uncertainties. Recover the task's role, current state, decisions, rejected options, open loops, boundaries, collaboration calibration, and exact next entry.

If the old task or its complete available summary/index coverage cannot be read, report Shadow validation blocked. Do not claim continuity. Do not edit files, mutate Git, create automations, contact third parties, rename or archive tasks, or broaden private-data access.

<incremental-packet>
```
