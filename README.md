# Codex Thread Handoff

**English** | [简体中文](README.zh-CN.md)

`codex-thread-handoff` is a Codex Skill for safely replacing a context-heavy, long-running task with a clean one. It carries forward source-backed state, recent decisions, open loops, permissions, and working style, then tests the successor before the old task is archived.

It is designed for tasks backed by project documentation, a knowledge base, an Obsidian Vault, or another explicit source-of-truth system.

## The problem

A generated handoff summary can sound complete while omitting a recent decision, reviving a rejected option, or turning an inference into a fact. Forking also keeps the old conversation's context pressure.

This Skill uses a clean successor task and four independent evidence layers:

1. the user's governing context;
2. current authorized sources of truth;
3. a short incremental packet from the old task;
4. direct read-only inspection of the explicitly named old task.

The goal is useful, evidence-backed continuity. The project does not claim lossless transfer of every message, hidden assumption, or aspect of rapport.

## Core safety model

| Phase | Required authorization | Allowed outcome |
|---|---|---|
| Prepare only | Request to prepare a handoff | Old task returns a short incremental packet, then freezes |
| Shadow validation | Explicit request to create or test a replacement | Clean non-forked task performs read-only recovery and validation |
| Finalize | Machine pass, three-question user calibration, and explicit switch approval | Titles switch, old task is archived but not deleted, status records are updated |

Completing one phase never authorizes the next. File writes, Git operations, automations, external contact, task renames, and archives are forbidden during Shadow validation. Commit and push require separate authorization even after Finalize.

## Install

Clone or download this repository, then copy the Skill directory into your Codex skills folder:

```bash
mkdir -p ~/.codex/skills
cp -R skill/codex-thread-handoff ~/.codex/skills/
```

Restart or reload Codex if your environment requires it to discover new Skills. The installed directory must contain `SKILL.md`, `agents/openai.yaml`, and `references/` directly beneath `~/.codex/skills/codex-thread-handoff/`.

## Basic use

Start in the existing long-running task:

```text
Use $codex-thread-handoff in Prepare only mode. My governing context and source-of-truth list are <locations>. Produce the incremental packet, then freeze this task. Do not create or archive anything yet.
```

When you explicitly authorize Shadow validation, the Skill should use available task tools to create a clean task in the same saved project, pass the packet, and require direct inspection of the old task by exact ID. It must not use Fork.

After the machine gate reports zero hard failures and at least `8/10`, use the candidate for three ordinary questions. At least two answers must feel comparably fitting, without repeated fallback to the old task or substantial repetition of already available context. Only then explicitly approve Finalize.

## What the Skill enforces

- The old task, not a detached controller's guess, produces the incremental packet.
- Every state-bearing packet item is classified as durable source, current-task reasoning, or unverified.
- The successor traverses all available old-task summaries or index entries and deep-reads key turns.
- Inaccessible old-task history blocks Shadow validation.
- Hidden expected answers are generated only after the candidate submits its first recovery response.
- Passing requires zero hard failures and at least `8/10`, followed by a separate real-use gate.
- The old task is archived, never deleted, and only after explicit approval.
- A suggested 60% to 70% context threshold is only a heuristic; the Skill does not claim it can measure context percentage unless the product exposes it.

## Tool requirements and limits

Prepare only can work with the current task and authorized source reads. The full workflow requires Codex task/thread tools that can:

- identify an exact task ID and saved project;
- create a clean task without forking;
- send, read, and wait for task messages;
- traverse the old task's available history or compact summaries;
- rename and archive tasks during Finalize.

If required task-history tools are unavailable, the Skill must stop at Prepare only or report `Shadow validation blocked`. It must never pretend that history was read or that a task operation succeeded. No MCP server, icon, or external service is assumed by this package.

## Project structure

```text
codex-thread-handoff/
├── README.md
├── README.zh-CN.md
├── LICENSE
├── skill/
│   └── codex-thread-handoff/
│       ├── SKILL.md
│       ├── agents/
│       │   └── openai.yaml
│       └── references/
│           ├── handoff-packet.md
│           ├── source-of-truth-adapter.md
│           └── validation-rubric.md
└── examples/
    └── complete-walkthrough.md
```

## Validation status

The packaged Skill passes the official `skill-creator` structural validator. A fresh read-only Prepare only forward test also produced the expected incremental packet, permission boundaries, freeze behavior, and blocked-shadow instructions. The workflow was developed through two local long-running-task migrations; one of them exposed a recovery gap that led to making direct old-task inspection mandatory. This is useful early evidence, not an end-to-end compatibility test or proof of lossless continuity across Codex versions and tool configurations.

See [the fictional walkthrough](examples/complete-walkthrough.md) for the full prepare, shadow, evaluation, calibration, and finalization sequence.

## License

MIT. See [LICENSE](LICENSE).
