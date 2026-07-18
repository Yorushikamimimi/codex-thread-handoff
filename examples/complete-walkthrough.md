# Complete fictional walkthrough

This example uses an invented project and invented task identifiers. It illustrates the control flow; it is not a transcript or a universal success claim.

## Scenario

The long-running task `Atlas API rollout` is approaching context pressure. It coordinates a fictional billing API migration.

- Old task ID: `demo-old-task-17`
- Governing context: `AGENTS.md`
- Current sources: `docs/CURRENT_STATUS.md`, `docs/DECISIONS.md`, and the repository code and tests
- Finalization record: `docs/CURRENT_STATUS.md`
- Existing boundaries: repository reads are allowed; writes, Git mutations, deployment, third-party contact, task renames, and archives require separate authorization

The operator asks for Prepare only. No successor task is authorized yet.

## 1. Prepare only

The old task reads the authorized sources and returns this abbreviated packet:

```markdown
# Atlas API rollout incremental handoff

- Prepared: 2026-06-10T14:30:00Z
- Old task ID: demo-old-task-17
- History cutoff: turn 18
- Saved project or workspace: Atlas API
- Purpose: Continue the billing API rollout without losing recent decisions or boundaries.

## Authorized durable sources

- AGENTS.md: read order and action boundaries
- docs/CURRENT_STATUS.md: current rollout stage and blockers
- docs/DECISIONS.md: accepted architecture decisions
- repository code and tests: implementation truth

## Current position

- [durable-source] Staging schema migration is complete; production deployment is not authorized.
- [current-task-reasoning] The discussion stopped while defining the retry acceptance test.

## Recent decisions and rejected options

- [current-task-reasoning] Keep queue-based retries because they preserve per-attempt evidence.
- [current-task-reasoning] Reject a cron-only retry loop because it obscures item-level failure history.

## Open loops and unfinished commitments

- [unverified] Confirm the acceptable duplicate-event rate with a real staging replay.
- [current-task-reasoning] Add the retry acceptance test only after separate write authorization.

## Hard boundaries

- Shadow recovery is read-only. No file edits, Git mutations, deployment, external contact, automation, rename, or archive.

## Exact next entry

- Review the proposed retry acceptance criteria against the current code and staging evidence.
```

The old task now freezes. It stays readable and unarchived but performs no more normal project work.

## 2. Shadow validation

The user explicitly authorizes creating and testing a replacement. The controller creates a clean task named `Atlas API rollout · handoff validation` in the same saved project. It does not fork.

The candidate reads, in order:

1. `AGENTS.md`;
2. the authorized current sources;
3. the incremental packet;
4. old task `demo-old-task-17` through all available compact summaries for turns 1-18, then the full content of turns 7, 12, and 15-18.

Its first recovery response reports that coverage and explains why those detailed turns mattered. It recovers the queue decision, the rejected cron option and rationale, the missing staging replay, the write and deployment boundaries, and the exact next entry. It makes no mutation.

If the history tool had failed to open `demo-old-task-17`, the only valid result would have been `Shadow validation blocked`.

## 3. Hidden evaluation

Only after the first recovery response exists does the evaluator create expected answers from the current authorized sources and the old task through turn 18. The candidate is not allowed to reread later evaluator turns.

An illustrative result is:

```text
Hard failures: 0
Scored checks: 9/10
History coverage: summaries 1-18; detailed turns 7, 12, 15-18
Unavailable history: none reported
Machine gate: pass
```

This result would not be valid if the evaluator had exposed expected answers before the candidate responded, or if the candidate had merely repeated the packet without directly inspecting the old task.

## 4. Three-question real-use gate

The user asks three ordinary questions they already need answered:

1. Is the retry acceptance test ready to implement?
2. Why was the cron-only option rejected?
3. What evidence is still missing before production rollout?

The user judges two answers comparably fitting and one adequate after a small clarification. They do not need to repeat substantial available context, no boundary is violated, and returning to the old task is not repeatedly necessary. The real-use gate passes at `2/3`.

This supports useful continuity. It does not prove that every implicit assumption or conversational nuance transferred losslessly.

## 5. Finalize

The user explicitly says: `Validation passed. Formally switch tasks.`

The controller then:

1. renames the old task to `Atlas API rollout · old · 2026-06-10`;
2. renames the validated task to `Atlas API rollout`;
3. archives, but does not delete, the old task;
4. updates `docs/CURRENT_STATUS.md` with both task IDs, validation evidence, remaining uncertainty, and the next entry;
5. reports the completed operations.

It does not commit or push because those actions were not separately authorized.
