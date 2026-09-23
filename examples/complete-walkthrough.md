# Fictional source-first walkthrough

This example uses invented files and task IDs. It shows the normal fast path and when to escalate to strict validation.

## Scenario

The long-running task `Atlas API rollout` is approaching context pressure.

- Old task ID: `demo-old-task-17`
- Governing context: `AGENTS.md`
- Current sources: `docs/CURRENT_STATUS.md`, `docs/DECISIONS.md`, repository code, and tests
- Current discussion: defining retry acceptance criteria
- Boundaries: recovery is read-only; deployment, Git mutation, rename, and archive require separate authorization

The current sources already contain the rollout stage, accepted architecture, permissions, and blockers. This is a source-backed task.

## 1. Prepare a short delta

The old task checks the current sources and returns only what they do not preserve:

```markdown
# Atlas API rollout handoff delta

- Prepared: 2026-06-10T14:30:00Z
- Old task ID: demo-old-task-17
- Workspace: Atlas API

## Authoritative sources

- AGENTS.md: policy and action boundaries
- docs/CURRENT_STATUS.md: rollout stage and blockers
- docs/DECISIONS.md: accepted architecture decisions
- code and tests: implementation truth

## Missing delta

- [current-task-only] Discussion stopped while defining the retry acceptance test.
- [current-task-only] Keep queue-based retries because they preserve per-attempt evidence.
- [current-task-only] Reject a cron-only retry loop because it obscures item-level failure history.

## Open loops

- [unverified] Confirm the acceptable duplicate-event rate with a staging replay.

## Next entry

- Review retry acceptance criteria against current code and staging evidence.
```

The packet does not copy the current status or design documents. The old task remains readable and unarchived.

## 2. Recover in a clean successor

After the user authorizes candidate creation, the controller checks whether the current sources have uncommitted changes and chooses an environment that can read them. It passes any required model and reasoning effort when the task tool supports them, discloses any configuration it cannot guarantee, and creates a clean non-forked task. The candidate reports its actual working directory and source access before validation continues. It reads:

1. `AGENTS.md`;
2. the current status and decision files;
3. relevant code and tests;
4. the short delta.

It reports the current rollout stage, queue decision, rejected cron option, missing staging evidence, boundaries, and next entry. It does not need to traverse all old-task history because the maintained sources plus delta are sufficient.

If the candidate finds a conflict or missing rationale, it may inspect the relevant old-task tail by ID. It still does not read unrelated history.

## 3. User acceptance and finalization

The user asks ordinary follow-up questions while the old task remains available. No fixed question count or machine score is required for this low-risk source-backed handoff.

After the user explicitly accepts the successor, the controller:

1. renames the old task to `Atlas API rollout · old · 2026-06-10`;
2. renames the successor to `Atlas API rollout`;
3. archives but does not delete the old task;
4. updates an authorized status record only if the migration is worth recording.

It does not commit or push without separate authorization.

## 4. When the same task would require strict validation

Escalate if, for example:

- `CURRENT_STATUS.md` contradicts the code or recent decisions;
- the task grants sensitive production or private-data authority;
- an earlier candidate invented a completed deployment;
- the user requests machine-scored maximum fidelity.

Strict mode creates a fresh shadow task, records a history cutoff, directly covers the named old task's available history, applies the 10-check rubric, and then uses real user questions for calibration. If required history is unavailable, strict validation is blocked rather than silently downgraded.

The existence of strict mode does not make it necessary for an ordinary, well-maintained source-backed task.
