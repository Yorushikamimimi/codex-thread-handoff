# Source-of-truth adapter

Use this reference to map the workflow to the user's actual documentation system. A source of truth may be a repository document, knowledge base, Obsidian Vault, issue tracker, database, or another authorized system. Do not assume any particular filename or product.

## Contents

- Map governing, current, history, and finalization sources
- Resolve source conflicts
- Adapt repository, knowledge-base, and no-durable-source setups
- Handle tool limits

## Map four source classes

### 1. Governing context

List the instructions that define read order, source precedence, privacy, and allowed actions. Examples include an `AGENTS.md`, an operating protocol, or workspace instructions.

Record:

- exact authorized location;
- required read order;
- scope it governs;
- conflicts it resolves;
- access restrictions.

### 2. Current source of truth

List only the current authoritative sources needed for this task. For each source, record:

- exact authorized location or system;
- what claim or state it owns;
- whether it is live state, durable policy, current project state, or historical evidence;
- its precedence when sources disagree;
- whether it is read-only during the current phase.

Prefer live code or data for implementation facts, current status records for declared state, and historical logs only for past events. Do not treat a handoff packet or an agent's statement as more authoritative than the source it summarizes.

### 3. Old task history

Record the exact old task ID and a history cutoff. During Shadow validation, treat that named task as a required read-only source for recent reasoning, decisions, rejected options, open loops, commitments, and collaboration calibration.

Reading the old task does not authorize:

- unrelated task-history searches;
- new file or private-data access;
- Git mutations;
- automation creation;
- task renames or archives;
- third-party contact or other external actions.

### 4. Finalization records

List the minimal current-status and historical records that may need a final migration entry. These remain read-only until the user explicitly authorizes Finalize.

## Use a compact mapping

```yaml
governing_context:
  - location: "<authorized location>"
    purpose: "<read order, permissions, or policy>"

current_sources:
  - location: "<authorized location or system>"
    owns: "<specific state or claim>"
    precedence: "<relative authority>"

old_task:
  id: "<exact task ID>"
  history_cutoff: "<timestamp or last turn>"

finalization_records:
  - location: "<authorized status or log record>"
    update_only_after: "explicit Finalize approval"
```

Keep the mapping in the task exchange unless the user asks to persist it.

## Resolve conflicts explicitly

When two sources disagree:

1. Apply the user's configured precedence.
2. Prefer current authoritative state over historical summaries for current facts.
3. Preserve the conflict in the packet when no precedence resolves it.
4. Mark the claim `unverified` instead of guessing.

Do not let recent conversational confidence silently overwrite durable evidence, and do not let stale durable documentation erase a newer fact that is clearly recorded in an authoritative live source.

## Adapt common setups

### Repository-backed task

- Governing context: repository instructions and contribution rules.
- Current sources: code, tests, current-status documents, issue or design records.
- Historical evidence: commits and logs, read only within existing authorization.
- Finalization records: a current-status or handoff document if the project already uses one.

### Knowledge-base or Obsidian-backed task

- Governing context: the user's entry protocol and authorized read order.
- Current sources: the specific status, decision, and topic notes named for this task.
- Historical evidence: only the logs required to resolve current handoff questions.
- Private areas: remain excluded unless they were already explicitly authorized for this task.

### No durable source set

Do not invent one. Prepare a packet from the old task, label unsupported claims `unverified`, and tell the user that Shadow validation can test old-task continuity but cannot verify durable current state. If the old task itself cannot be read directly, Shadow validation is blocked.

## Handle tool limits honestly

- If file or knowledge-system reads are unavailable, disclose which current facts cannot be verified.
- If old-task history tools are unavailable, stop at Prepare only or report Shadow validation blocked.
- If task creation is unavailable, stop at Prepare only or report Shadow validation blocked; do not claim a shadow task exists.
- If rename or archive tools are unavailable, report Finalize blocked and leave the affected state unchanged; do not claim completion or silently substitute manual steps.
- Do not add an MCP or external dependency merely because a source type could use one. Use only tools actually available and authorized in the current environment.
