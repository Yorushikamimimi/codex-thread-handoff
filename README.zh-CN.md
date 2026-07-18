# Codex Thread Handoff

[English](README.md) | **简体中文**

`codex-thread-handoff` 是一个用于长期 Codex 任务安全迁移的 Skill。它不会把一份看起来完整的交接摘要直接当成“无损接管”，而是先恢复有信源支撑的当前状态、近期决定、未决事项、权限边界和协作方式，再通过影子任务验证候选接管者，最后才归档旧任务。

它适用于由项目文档、知识库、Obsidian Vault 或其他明确 source of truth 支撑的长期任务。

## 它解决什么问题

一份自动生成的交接摘要可能看起来很完整，却遗漏近期决定、重新采用已否决的方案，或者把推断写成事实。直接 Fork 也会把旧对话的上下文压力一起带到新任务中。

本 Skill 使用干净的新任务，并要求四层独立证据：

1. 用户的 governing context；
2. 当前获授权的 source of truth；
3. 旧任务生成的短增量接管包；
4. 对明确指定旧任务的直接只读检查。

目标是获得有证据、可实际使用的连续性，而不是承诺每条消息、每个隐含假设或全部默契都能无损迁移。

## 核心安全模型

| 阶段 | 所需授权 | 允许的结果 |
|---|---|---|
| Prepare only | 用户要求准备交接 | 旧任务输出短增量接管包，然后冻结 |
| Shadow validation | 用户明确要求创建或测试替代任务 | 新建非 Fork 的干净任务，执行只读恢复与验证 |
| Finalize | 机器测试通过、三个真实问题校准通过，并得到用户明确切换确认 | 新旧任务交换名称，旧任务归档但不删除，并更新状态记录 |

完成一个阶段不会自动授权下一个阶段。Shadow validation 期间禁止修改文件、执行 Git 写操作、创建自动任务、联系第三方、改名或归档任务。即使 Finalize 完成，commit 和 push 仍需要单独授权。

## 安装

Clone 或下载本项目，然后把 Skill 目录复制到 Codex Skills 目录：

```bash
mkdir -p ~/.codex/skills
cp -R skill/codex-thread-handoff ~/.codex/skills/
```

如果当前 Codex 环境需要重新加载或重启才能发现新 Skill，请完成对应操作。安装后，`~/.codex/skills/codex-thread-handoff/` 下应直接包含 `SKILL.md`、`agents/openai.yaml` 和 `references/`。

## 基本用法

先在现有长期任务中执行：

```text
Use $codex-thread-handoff in Prepare only mode. My governing context and source-of-truth list are <locations>. Produce the incremental packet, then freeze this task. Do not create or archive anything yet.
```

当你明确授权 Shadow validation 后，Skill 会优先使用当前可用的 task/thread tools，在同一项目中创建一个干净任务，传入接管包，并要求新任务通过准确的旧任务 ID 直接检查旧任务历史。它不会使用 Fork。

机器验收必须达到硬失败为 0、评分至少 `8/10`。随后，你需要在候选任务中提出三个平时真正会问的问题；至少两个回答仍然贴切，不需要反复退回旧任务，也不应要求你重新解释已经存在的大量背景。只有这些条件满足后，才能明确批准 Finalize。

## Skill 会强制执行什么

- 增量接管包由旧任务生成，而不是由脱离现场的主控凭空猜测。
- 接管包中的状态条目必须区分 `durable-source`、`current-task-reasoning` 和 `unverified`。
- 新任务必须遍历旧任务全部可用的轮次摘要或索引，并精读关键轮次。
- 无法读取旧任务历史时，Shadow validation 必须阻塞。
- 隐藏预期答案只能在候选任务提交第一次恢复答卷后生成。
- 机器验收需要硬失败为 0 且至少 `8/10`，之后还必须经过真实使用校准。
- 只有获得用户明确确认后，才能归档旧任务；旧任务永远不删除。
- 建议在上下文压力达到约 60%～70% 时开始准备迁移，但这只是启发式判断。除非产品明确提供数据，否则 Skill 不会声称可以测得准确占用比例。

## 工具要求与限制

Prepare only 只需要当前任务和获授权信源的读取能力。完整流程需要 Codex task/thread tools 支持：

- 获取准确的任务 ID 和所属项目；
- 创建非 Fork 的干净任务；
- 发送、读取和等待任务消息；
- 遍历旧任务的可用历史或压缩摘要；
- 在 Finalize 阶段改名和归档任务。

如果缺少旧任务历史读取工具，Skill 必须停留在 Prepare only，或报告 `Shadow validation blocked`。它不能假装已经读取历史，也不能谎报任务操作成功。本项目不预设 MCP、图标或外部服务依赖。

## 项目结构

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

## 当前验证状态

打包后的 Skill 已通过官方 `skill-creator` 结构校验。一个全新的只读 Prepare-only 前向测试也成功生成了预期的增量接管包、权限边界、冻结行为和 Shadow 阻塞说明。

这套流程已经来自两次本地长期任务迁移，其中一次暴露了恢复缺口，并促使“直接读取旧任务”升级为强制要求。这些是有价值的早期证据，但还不是跨 Codex 版本和不同工具组合的端到端兼容性测试，也不能证明上下文能够无损迁移。

完整的准备、影子恢复、机器验收、真实使用校准和正式切换过程，可参考[虚构完整示例](examples/complete-walkthrough.md)。

## License

MIT，详见 [LICENSE](LICENSE)。
