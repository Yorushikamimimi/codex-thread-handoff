# Source-of-truth adapter

Use this reference when the user's documentation system, source precedence, or durable coverage is unclear.

## Map the sources

### Governing context

List the instructions that define read order, precedence, privacy, and allowed actions. Record the exact authorized location and scope.

### Current authoritative sources

List only sources needed to recover this task. For each, record what it owns, whether it is current state or historical evidence, and its precedence when sources disagree.

Prefer live code or data for implementation facts, current status records for declared state, and historical logs only for past events. A handoff bridge never outranks the source it summarizes.

### Durable coverage

Classify coverage before selecting a handoff path:

- `high`: current state, decisions, boundaries, progress, and next entry are mostly maintained;
- `low`: most useful state remains conversation-only;
- `conflicted`: sources disagree, appear stale, or omit important decisions.

Choose source-backed fast path for `high`, a standalone handoff document for `low`, and strict validation when `conflicted` creates meaningful risk.

### Old task

Record the exact task ID when available. In fast paths it is a targeted fallback source. In strict validation it becomes a required read-only source through a recorded history cutoff.

Reading the old task does not authorize unrelated history searches, new file or private-data access, Git mutations, automations, task renames or archives, third-party contact, or other external actions.

### Finalization records

List only the status or historical records that may need a migration entry. Keep them read-only until finalization is explicitly authorized.

## Compact mapping

```yaml
governing_context:
  - location: "<authorized location>"
    purpose: "<policy or read order>"

current_sources:
  - location: "<authorized location or system>"
    owns: "<specific state or claim>"
    precedence: "<relative authority>"

durable_coverage: "high | low | conflicted"
handoff_path: "source-backed | conversation-backed | strict"

old_task:
  id: "<exact task ID, if available>"
  role: "targeted fallback | required strict source"

finalization_records:
  - location: "<authorized record>"
    update_only_after: "explicit finalization approval"
```

## Resolve conflicts

1. Apply configured precedence.
2. Prefer current authoritative state over historical summaries for current facts.
3. Preserve unresolved conflicts in the bridge.
4. Mark the claim `unverified` instead of guessing.

## Common setups

- **Repository-backed:** instructions plus code, tests, decisions, and current worklog usually support the fast path.
- **Knowledge-base-backed:** entry protocol plus topic source of truth and current status usually support the fast path; private areas remain excluded unless explicitly authorized.
- **No durable source set:** use a conversation-backed handoff document; do not invent authority.
- **Conflicted or high-risk:** use strict validation and require direct old-task inspection.

If task creation, history, rename, or archive tools are unavailable, disclose the limitation and stop only the affected operation. Do not simulate success or add an external dependency merely because one might help.
