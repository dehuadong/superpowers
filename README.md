# Superpowers（超能力）

Superpowers 是一套完整的软件开发方法论，为你的编程助手打造，构建于一组可组合的技能和一些初始指令之上，确保你的助手会真正使用它们。

## 工作原理

这一切从你启动编程助手的那一刻就开始了。一旦它发现你正在构建什么东西，它*不会*直接跳进去尝试写代码。相反，它会退后一步，先问清楚你到底想做什么。

一旦它从对话中梳理出规格说明，就会分块展示给你看，每块都短到足以让你真正读完并消化。

在你对设计签字确认后，你的助手会制定一个实现计划，清晰到足以让一个品味堪忧、缺乏判断力、对项目背景一无所知且厌恶测试的初级工程师也能照做。它强调真正的红/绿 TDD、YAGNI（你不会需要它）和 DRY 原则。

接下来，当你说“开始”时，它会启动一个*子代理驱动开发*流程，让代理逐项完成工程任务，检查并审查其工作，然后继续推进。Claude 经常能够自主工作数小时而不偏离你们一起制定的计划。

还有很多其他功能，但这就是系统的核心。而且因为这些技能会自动触发，你不需要做任何特别的事情——你的编程助手自然就拥有了超能力。

## 赞助

如果 Superpowers 帮你赚了钱，并且你愿意的话，我将非常感激你考虑[赞助我的开源工作](https://github.com/sponsors/obra)。

谢谢！

——Jesse

## 安装

**注意：** 安装方式因平台而异。

### Claude Code 官方市场

Superpowers 可通过 [Claude 官方插件市场](https://claude.com/plugins/superpowers) 获取。

从 Anthropic 官方市场安装插件：

```bash
/plugin install superpowers@claude-plugins-official
```

### Claude Code（Superpowers 市场）

Superpowers 市场为 Claude Code 提供 Superpowers 及其他一些相关插件。

在 Claude Code 中，先注册市场：

```bash
/plugin marketplace add obra/superpowers-marketplace
```

然后从此市场安装插件：

```bash
/plugin install superpowers@superpowers-marketplace
```

### OpenAI Codex CLI

- 打开插件搜索界面：

```bash
/plugins
```

搜索 Superpowers：

```bash
superpowers
```

选择 `Install Plugin`。

### OpenAI Codex App

- 在 Codex 应用中，点击侧边栏的 Plugins。
- 你应该能在 Coding 部分看到 `Superpowers`。
- 点击 `Superpowers` 旁边的 `+` 并按照提示操作。

### Cursor（通过插件市场）

在 Cursor Agent 聊天中，从市场安装：

```text
/add-plugin superpowers
```

或在插件市场中搜索 "superpowers"。

### OpenCode

告诉 OpenCode：

```
Fetch and follow instructions from https://raw.githubusercontent.com/obra/superpowers/refs/heads/main/.opencode/INSTALL.md
```

**详细文档：** [docs/README.opencode.md](docs/README.opencode.md)

### GitHub Copilot CLI

```bash
copilot plugin marketplace add obra/superpowers-marketplace
copilot plugin install superpowers@superpowers-marketplace
```

### Gemini CLI

```bash
gemini extensions install https://github.com/obra/superpowers
```

更新命令：

```bash
gemini extensions update superpowers
```

## 基本工作流程

1.  **brainstorming（头脑风暴）** - 在编写代码前激活。通过提问细化粗略想法，探索替代方案，分段展示设计以供验证。保存设计文档。

2.  **using-git-worktrees（使用 git 工作树）** - 在设计批准后激活。在新分支上创建隔离工作区，运行项目设置，验证干净的测试基线。

3.  **writing-plans（编写计划）** - 在设计获批后激活。将工作分解为小任务（每个任务 2-5 分钟）。每个任务都有确切的文件路径、完整代码和验证步骤。

4.  **subagent-driven-development（子代理驱动开发）** 或 **executing-plans（执行计划）** - 在计划就绪时激活。为每个任务派遣新的子代理，进行两阶段审查（先是规格符合性，然后是代码质量），或分批执行并设置人工检查点。

5.  **test-driven-development（测试驱动开发）** - 在实现过程中激活。强制执行红-绿-重构：先写一个失败的测试，看着它失败，然后编写最少量的代码，看着它通过，最后提交。删除在测试之前写的代码。

6.  **requesting-code-review（请求代码审查）** - 在任务之间激活。对照计划进行审查，按严重程度报告问题。严重问题会阻止进度。

7.  **finishing-a-development-branch（完成开发分支）** - 在任务完成后激活。验证测试，提出处理方案（合并/发起 PR/保留/丢弃），清理工作树。

**编程助手在执行任何任务前都会检查相关技能。** 这些是强制性工作流程，而非建议。

## 内容概要

### 技能库

**测试**

- **test-driven-development（测试驱动开发）** - 红-绿-重构循环（包含测试反模式参考）

**调试**

- **systematic-debugging（系统化调试）** - 四阶段根因分析过程（包含 root-cause-tracing（根因追踪）、defense-in-depth（纵深防御）、condition-based-waiting（基于条件的等待）技术）
- **verification-before-completion（完成前验证）** - 确保问题确实被修复

**协作**

- **brainstorming（头脑风暴）** - 苏格拉底式的设计细化
- **writing-plans（编写计划）** - 详细的实现计划
- **executing-plans（执行计划）** - 带检查点的批量执行
- **dispatching-parallel-agents（派遣并行代理）** - 并发子代理工作流
- **requesting-code-review（请求代码审查）** - 审查前检查清单
- **receiving-code-review（接收代码审查）** - 响应反馈
- **using-git-worktrees（使用 git 工作树）** - 并行开发分支
- **finishing-a-development-branch（完成开发分支）** - 合并/发起 PR 决策工作流
- **subagent-driven-development（子代理驱动开发）** - 通过两阶段审查（规格符合性，然后是代码质量）实现快速迭代

**元技能**

- **writing-skills（编写技能）** - 遵循最佳实践创建新技能（包含测试方法）
- **using-superpowers（使用超能力）** - 技能系统简介

## 哲学理念

- **测试驱动开发** - 始终坚持测试先行
- **系统化而非临时应对** - 信任流程而非靠运气猜测
- **降低复杂性** - 简洁是首要目标
- **证据胜过声明** - 在宣布成功之前务必验证

阅读[原始发布公告](https://blog.fsck.com/2025/10/09/superpowers/)。

## 参与贡献

Superpowers 的一般贡献流程如下。请注意，我们通常不接受新技能的贡献，并且任何对技能的更新都必须兼容我们支持的所有编程助手。

1. Fork 此仓库
2. 切换到 'dev' 分支
3. 为你的工作创建一个新分支
4. 遵循 `writing-skills（编写技能）` 技能来创建和测试新的以及修改过的技能
5. 提交 PR，务必填写拉取请求模板。

完整指南见 `skills/writing-skills/SKILL.md`。

## 更新

Superpowers 的更新方式在一定程度上取决于所使用的编程助手，但通常是自动的。

## 许可证

MIT 许可证 - 详情见 LICENSE 文件

## 社区

Superpowers 由 [Jesse Vincent](https://blog.fsck.com) 以及 [Prime Radiant](https://primeradiant.com) 的其他成员共同构建。

- **Discord**：[加入我们](https://discord.gg/35wsABTejz)获取社区支持、提问，并分享你使用 Superpowers 正在构建的项目
- **问题反馈**：https://github.com/obra/superpowers/issues
- **发布公告**：[注册](https://primeradiant.com/superpowers/)以便在新版本发布时收到通知
