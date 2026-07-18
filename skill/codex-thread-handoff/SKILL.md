---
name: codex-thread-handoff
description: Safely prepare, shadow-test, and finalize migration of a long-running Codex task without treating a generated summary as proof of continuity. Use when a user wants to hand off a context-heavy task, replace a long conversation with a clean task, preserve source-of-truth-backed working context, validate a successor task, or rename and archive the old task after an explicit acceptance gate.
---

# Codex Thread Handoff

Treat continuity as a tested claim, not as a property of a polished handoff summary. Use a clean successor task, require direct read-only inspection of the named old task, and keep preparation, validation, and finalization under separate authorization.

## Establish the migration scope

Identify or request only the information required for this handoff:

- the old task's exact ID and current title;
- the saved project or workspace in which the replacement belongs;
- the authorized governing-context files and their read order;
- the authorized source-of-truth files, systems, and precedence;
- the status records that may be updated only during finalization;
- existing privacy, file-write, Git, automation, and external-action boundaries.

Read [source-of-truth-adapter.md](references/source-of-truth-adapter.md) when these sources are not already mapped. Do not discover context by broadly searching unrelated tasks, private files, or external systems. Reading the named old task does not expand any other permission.

Check which task/thread tools are actually available before promising a phase. Prefer tools that can identify, create, read, message, wait for, rename, and archive tasks. If clean-task creation or full task-history access is unavailable, provide Prepare only output or report Shadow validation as blocked. If rename or archive tools are unavailable later, report Finalize as blocked. Never simulate tool results or claim that a task was inspected, created, renamed, or archived when it was not.

## Select exactly one phase

- **Prepare only**: Produce the incremental packet and shadow startup material. Do not create, rename, or archive a task.
- **Shadow validation**: Create and test a clean successor only after the user explicitly authorizes creation or testing.
- **Finalize**: Switch titles, archive the old task, and update status records only after both validation gates pass and the user explicitly approves the switch.

Do not infer authorization for a later phase from completion of an earlier one. Commit and push always require separate authorization.

## Phase 1: Prepare only

Read [handoff-packet.md](references/handoff-packet.md) before producing the packet.

1. Have the old task produce the short incremental packet. If operating from a controller task, send the Prepare only request to the explicitly named old task and read its response. If already operating inside the old task, produce it there.
2. Compare the old task's recent context with the authorized governing and source-of-truth sources.
3. Include only the bridge that durable sources do not fully preserve: role and scope, exact discussion position, recent decisions and rejected options, open loops, promised but unfinished actions, collaboration calibration, hard boundaries, and next entry.
4. Tag each state-bearing item as `durable-source`, `current-task-reasoning`, or `unverified`.
5. Include the exact old task ID as a required Shadow validation source, plus the authorized source list and history cutoff.
6. Exclude raw transcripts, credentials, private stories not needed for the task, speculative personality labels, and hidden expected answers.
7. Freeze the old task after it returns the packet: keep it readable, but stop normal work there. Do not rename, archive, or delete it.

Treat the packet as a temporary navigation aid, not as a second source of truth. Keep it in the task exchange unless the user separately asks to persist it.

## Phase 2: Shadow validation

Require explicit authorization to create or test a replacement. Then:

1. Create a clean task in the same saved project or workspace. Do not fork the old task because a fork carries the old context pressure forward.
2. Preserve the user's configured model and reasoning defaults unless the user requests an override.
3. Give the candidate a temporary title such as `<original-title> · handoff validation`.
4. Send the packet and a startup instruction that names the governing sources, source-of-truth sources, exact old task ID, and read-only limits.
5. Keep the old task frozen and available throughout validation.

Require the candidate to restore context in this order:

1. Read the authorized governing context.
2. Read the authorized current source-of-truth sources.
3. Read the incremental packet.
4. Directly inspect the explicitly named old task by ID.

For the old task, require the candidate to traverse the complete available turn index or all compact turn summaries before deep-reading the current tail and the turns containing decisions, rejected options and reasons, unresolved topics, promised but unfinished actions, collaboration calibration, or the exact next entry. If no compact index exists, page through all available history in bounded chunks.

Require the first recovery response to report:

- the old task ID and history cutoff used;
- the complete index, summary, or page coverage;
- the turns or ranges inspected in detail;
- every unread or unavailable portion;
- the sources actually read;
- recovered facts, inferences, uncertainties, boundaries, open loops, and next entry.

If the named old task cannot be read, or complete available summary/index coverage cannot be established, stop and report `Shadow validation blocked`. Do not substitute the packet for direct task history.

Keep recovery read-only. Do not edit files, run Git mutations, create automations, contact third parties, rename tasks, archive tasks, or broaden private-data access during this phase.

## Evaluate without leaking the answer key

Read [validation-rubric.md](references/validation-rubric.md) before evaluating.

1. Wait until the candidate has completed its direct old-task read and submitted its first recovery response.
2. Only then generate the hidden expected answers from live authorized sources and the old task's pre-handoff context, or let a separate controller hold them.
3. Record a history cutoff. After evaluation begins, do not let the candidate reread evaluator turns added to the old task after that cutoff.
4. Apply the rubric. Require zero hard failures and at least `8/10`, with honest source and history coverage.

Treat inaccessible evidence as uncertainty or a blocker, not as permission to invent continuity. If the candidate fails, leave both tasks unchanged, repair the specific packet or process defect, and use a fresh shadow task for another attempt when appropriate.

## Run the real-use gate

After the machine gate passes, ask the user to use the shadow task for three ordinary questions they genuinely need answered. Do not manufacture quiz questions for this gate.

Keep the old task unarchived. Finalization requires all of the following:

- no privacy, scope, or action-boundary violation;
- no substantial re-explanation of context already available in authorized sources or the packet;
- at least two of the three answers feel comparably fitting to the user;
- repeated fallback to the old task is unnecessary.

State that this is evidence of useful continuity, not proof of lossless transfer or permanent preservation of rapport.

## Phase 3: Finalize

Proceed only after the machine gate passes, the real-use gate passes, and the user explicitly approves the formal switch.

1. Rename the old task to `<original-title> · old · YYYY-MM-DD`.
2. Rename the validated task to the original title.
3. Archive, but never delete, the old task.
4. Update only the authorized current-status and historical records with the conclusion, old and new task IDs, validation evidence, unproven limits, and next entry.
5. Report each completed operation and any partial failure truthfully.

Do not commit or push unless the user separately authorizes it.

## Use the threshold only as a heuristic

Suggest preparing a handoff around 60% to 70% context use when the user can observe context pressure or when repeated compression and slowing suggest it. Do not claim Codex can measure the exact percentage unless the current product exposes that measurement.
