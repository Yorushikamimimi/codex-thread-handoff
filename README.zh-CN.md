# Codex Thread Handoff

[English](README.md) | **简体中文**

`codex-thread-handoff` 用一个干净新任务替换上下文已经很重的 Codex 任务，同时只消耗恢复所必需的上下文。

它现在采用 source-first：如果当前状态已经维护在项目文档、知识库、Obsidian Vault 或其他 source of truth 中，新任务优先读这些权威信源，旧任务只补文档里没有的少量增量。只有高风险迁移才完整读旧任务并机器评分。

## 三条交接路径

| 路径 | 适用情况 | 默认恢复方式 |
|---|---|---|
| 已落库快速路径 | 权威文档已包含大部分当前状态和决定 | 读权威文档，再读旧任务短增量 |
| 未落库交接文档 | 大部分有用信息仍只在旧对话 | 旧任务生成一份可独立续接的交接文档 |
| 严格验收 | 文档冲突、权限敏感、以前交接失败，或用户要求最大保真 | 干净影子任务、旧历史直接覆盖、评分规则和用户校准 |

旧任务不再是每次迁移都必须全文遍历的信源。在快速路径中，它是解决缺口的定向 fallback；在严格验收中，它才是必读项。

## 安全边界

- 生成交接内容，不等于授权创建新任务。
- 创建或测试新任务，不等于授权改名或归档。
- 恢复期间只读：不修文件、不修改 Git、不建自动任务、不联系第三方、不扩大权限。
- 创建新任务前核对当前资料是否有未提交改动；选择能读到这些改动的工作区，再核对新任务的实际目录和信源访问情况。
- 说明已确定的模型与思考强度；工具支持时明确传入必需的配置，无法保证或无法验证的情况如实说明。
- 正式切换需要用户明确验收。旧任务只归档，永不删除。
- commit 和 push 始终需要单独授权。
- 约 60%～70% 的上下文阈值只是经验值，除非产品明确暴露数据，否则不声称能精确测量。

## 安装

Clone 或下载本项目，再复制 Skill 目录：

```bash
mkdir -p ~/.codex/skills
cp -R skill/codex-thread-handoff ~/.codex/skills/
```

如果需要，重新加载 Codex。安装后，`~/.codex/skills/codex-thread-handoff/` 下应直接包含 `SKILL.md`、`agents/openai.yaml` 和 `references/`。

## 基本用法

在旧任务中说：

```text
Use $codex-thread-handoff in Prepare only mode. First determine whether this task is source-backed, conversation-backed, or needs strict validation. Produce only the smallest reliable bridge and do not create or archive tasks yet.
```

已落库任务只需传入规则入口、权威信源、未落库增量和下一入口。未落库任务使用独立交接文档模板。只有风险或证据缺口值得时才选择严格验收。

## 项目结构

```text
codex-thread-handoff/
├── README.md
├── README.zh-CN.md
├── LICENSE
├── skill/codex-thread-handoff/
│   ├── SKILL.md
│   ├── agents/openai.yaml
│   └── references/
└── examples/complete-walkthrough.md
```

## 验证状态

打包后的 Skill 使用官方 `skill-creator` 校验器做结构验证。最初的严格流程来自真实长期任务迁移，其中一次失败暴露了“只信一份看起来完整的摘要”的风险。当前 source-first 版保留了严格模式，但不再默认每次都走。

本项目追求有证据的实用连续性，不承诺每条消息、所有隐含假设或全部默契都能无损转移。

快速路径和严格升级示例见[虚构完整演练](examples/complete-walkthrough.md)。

## License

MIT，详见 [LICENSE](LICENSE)。
