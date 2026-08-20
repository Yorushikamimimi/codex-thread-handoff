---
name: codex-thread-handoff
description: "Migrate a context-heavy Codex task using the lightest reliable path: restore authoritative project or knowledge-base sources first, ask the old task only for missing deltas, or create a standalone handoff document when no durable sources exist. Use when replacing a long conversation with a clean task, preserving working context, validating a successor, or archiving the old task after explicit acceptance."
---

# Codex Thread Handoff

Prefer source-backed recovery over replaying the full old conversation. Increase validation cost only when the evidence or risk requires it.

## Establish scope

Identify the old task, saved project or workspace, governing instructions, authorized current sources, action boundaries, and the records that may be updated during finalization. Read [source-of-truth-adapter.md](references/source-of-truth-adapter.md) when the source map or precedence is unclear.

Do not discover context by searching unrelated tasks, private files, or external systems. Reading an old task never expands file, Private, Git, automation, task-management, or external-action authority.

## Choose the lightest reliable path

### A. Source-backed fast path — default

Use when maintained files or systems already contain most current state, decisions, permissions, progress, and next steps.

1. Verify the governing and current sources are readable and not obviously stale.
2. If the old task already has normal write authority, finish required worklog or source-of-truth persistence before freezing it. Otherwise do not infer permission.
3. Ask the old task only for information missing from durable sources: exact discussion position, recent reasoning, relevant rejected options, open loops, unfinished commitments, undocumented calibration or boundaries, and the exact next entry.
4. Give the successor the authoritative source map plus this short delta.
5. Require a concise recovery report listing sources read, current state, uncertainty, boundaries, and next entry.

The old task ID is a fallback source, not a mandatory full-history read. Inspect only the specific tail or turns needed to resolve a disclosed gap.

### B. Conversation-backed handoff document

Use when no maintained source of truth exists or most useful state still lives only in the conversation.

1. Have the old task produce one standalone handoff document using [handoff-packet.md](references/handoff-packet.md).
2. Include established facts and evidence, decisions and rejected options, completed results, current position, open loops, boundaries, and the next entry.
3. Mark unsupported claims `unverified`; an old task's memory is not external truth.
4. Persist the document only when writes are already authorized or the user asks for a file. Otherwise keep it in the task exchange.
5. Let the successor read the document and explicitly named supporting files. Use targeted old-task inspection only when a gap remains and history tools are available.

Do not create a second archive when a maintained worklog or current-status file already exists.

### C. Strict validation — exception path

Use a full shadow process when any of these applies:

- the user requests maximum-fidelity or machine-scored validation;
- sensitive authorization boundaries or high-impact actions are involved;
- current sources conflict, appear stale, or omit important decisions;
- a previous handoff hallucinated state or missed open loops;
- the candidate cannot recover confidently from the sources plus bridge.

Strict mode requires a clean shadow task, direct read-only coverage of the named old task, [validation-rubric.md](references/validation-rubric.md), and user calibration. If required history is unavailable, strict validation is blocked. This does not invalidate a source-backed fast handoff whose limits are disclosed.

## Keep operations separately authorized

- **Prepare only**: Produce the delta or handoff document; do not create, rename, or archive tasks.
- **Candidate recovery**: Create a clean non-forked task only after explicit authorization to replace or test. Keep recovery read-only.
- **Finalize**: Rename and archive only after the user explicitly accepts the successor.

Do not infer a later operation from an earlier one. Commit and push always require separate authorization.

## Candidate creation and recovery

When task tools are available:

1. Create the successor in the same saved project or workspace. Do not Fork a context-heavy task.
2. Preserve the user's model and reasoning defaults unless they request an override.
3. Use a temporary title such as `<original title> · handoff validation`.
4. Keep the old task readable and unarchived.
5. Instruct the successor to read only the chosen sources and bridge, distinguish fact from inference and uncertainty, and make no file edits, Git mutations, automations, third-party contact, rename, archive, or permission expansion during recovery.

For fast paths, a concise recovery report plus ordinary user use is sufficient. Do not force a 10-point score or exactly three questions unless strict validation was selected.

If task creation is unavailable, return the bridge and startup instructions. Never pretend that a task operation succeeded.

## Finalize safely

After explicit acceptance:

1. Rename the old task to `<original title> · old · YYYY-MM-DD`.
2. Rename the successor to the original title.
3. Archive, but never delete, the old task.
4. Update only authorized current-status or history records when the migration is worth recording.
5. Report old and new task IDs, completed operations, and remaining uncertainty.

## Threshold

Preparing around 60% to 70% observed context use is a heuristic. Do not claim Codex can measure the exact percentage unless the current product exposes it.
