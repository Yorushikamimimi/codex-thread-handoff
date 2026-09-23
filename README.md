# Codex Thread Handoff

**English** | [简体中文](README.zh-CN.md)

`codex-thread-handoff` replaces a context-heavy Codex task with a clean successor while spending only the context needed for reliable recovery.

The Skill is source-first: when current state is already maintained in project docs, a knowledge base, an Obsidian Vault, or another source-of-truth system, the successor reads those sources and the old task contributes only the missing delta. Full old-task replay and machine scoring are reserved for higher-risk migrations.

## Three handoff paths

| Path | Use when | Default recovery |
|---|---|---|
| Source-backed fast path | Maintained sources contain most current state and decisions | Read authoritative sources, then a short old-task delta |
| Conversation-backed document | Most useful state still lives only in the old conversation | Old task creates one standalone handoff document |
| Strict validation | Sources conflict, permissions are sensitive, a previous handoff failed, or maximum fidelity is requested | Clean shadow task, direct old-task history coverage, rubric, and user calibration |

The old task is no longer a mandatory full-history read for every migration. It remains a targeted fallback in fast paths and a required source in strict validation.

## Safety model

- Preparing a bridge does not authorize creating a successor.
- Creating or testing a successor does not authorize renaming or archiving tasks.
- Recovery remains read-only: no file edits, Git mutations, automations, third-party contact, or permission expansion.
- Before creating a successor, check whether current facts include uncommitted changes. Use an environment that can read them, then verify the successor's actual working directory and source access.
- State the selected model and reasoning effort when known, pass required values explicitly when supported, and disclose any configuration or runtime details the platform cannot verify.
- Finalization requires explicit user acceptance. The old task is archived, never deleted.
- Commit and push require separate authorization.
- A suggested 60% to 70% context threshold is only a heuristic unless the product exposes an exact measurement.

## Install

Clone or download the repository, then copy the Skill directory:

```bash
mkdir -p ~/.codex/skills
cp -R skill/codex-thread-handoff ~/.codex/skills/
```

Reload Codex if required. The installed folder should contain `SKILL.md`, `agents/openai.yaml`, and `references/` directly beneath `~/.codex/skills/codex-thread-handoff/`.

## Basic use

In the old task:

```text
Use $codex-thread-handoff in Prepare only mode. First determine whether this task is source-backed, conversation-backed, or needs strict validation. Produce only the smallest reliable bridge and do not create or archive tasks yet.
```

For a source-backed task, pass the successor the governing instructions, authoritative source map, missing delta, and exact next entry. For a conversation-backed task, use the standalone handoff template. Select strict validation only when the risk or evidence gap warrants it.

## Project structure

```text
codex-thread-handoff/
├── README.md
├── README.zh-CN.md
├── LICENSE
├── skill/
│   └── codex-thread-handoff/
│       ├── SKILL.md
│       ├── agents/openai.yaml
│       └── references/
│           ├── handoff-packet.md
│           ├── source-of-truth-adapter.md
│           └── validation-rubric.md
└── examples/complete-walkthrough.md
```

## Validation status

The packaged Skill is structurally validated with the official `skill-creator` validator. Its original strict workflow was developed from real long-running-task migrations, including a failed recovery that exposed the danger of trusting a polished summary. The current source-first revision preserves that strict mode but makes it an exception rather than the default.

This project supports evidence-backed continuity. It does not claim lossless transfer of every message, hidden assumption, or aspect of rapport.

See the [fictional walkthrough](examples/complete-walkthrough.md) for the fast path and strict escalation.

## License

MIT. See [LICENSE](LICENSE).
