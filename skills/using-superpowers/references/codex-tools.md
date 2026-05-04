# Codex 工具映射

技能（Skills）使用的是 Claude Code 的工具名称。当你在技能中遇到这些名称时，请使用你所在平台的等效工具：

| 技能中引用的工具                    | Codex 等效工具                                              |
| ----------------------------------- | ----------------------------------------------------------- |
| `Task` 工具（派发子代理）           | `spawn_agent`（参见 [命名代理派发](#named-agent-dispatch)） |
| 多次 `Task` 调用（并行）            | 多次 `spawn_agent` 调用                                     |
| Task 返回结果                       | `wait`                                                      |
| Task 自动完成                       | 使用 `close_agent` 释放槽位                                 |
| `TodoWrite`（任务跟踪）             | `update_plan`                                               |
| `Skill` 工具（调用技能）            | 技能会原生加载——直接遵循其中的说明即可                      |
| `Read`、`Write`、`Edit`（文件操作） | 使用你原生的文件工具                                        |
| `Bash`（运行命令）                  | 使用你原生的 Shell 工具                                     |

## 子代理派发需要多代理支持

将其添加到你的 Codex 配置文件（`~/.codex/config.toml`）中：

```toml
[features]
multi_agent = true
```

这将启用 `spawn_agent`、`wait` 和 `close_agent`，以支持 `dispatching-parallel-agents` 和 `subagent-driven-development` 等技能。

## 命名代理派发

Claude Code 技能会引用类似 `superpowers:code-reviewer` 的命名代理类型。
Codex 没有命名代理注册表——`spawn_agent` 会从内置角色（`default`、`explorer`、`worker`）中创建通用代理。

当技能指示派发某个命名代理类型时：

1. 找到该代理的提示词文件（例如 `agents/code-reviewer.md` 或技能的本地提示词模板如 `code-quality-reviewer-prompt.md`）
2. 读取提示词内容
3. 填充所有模板占位符（`{BASE_SHA}`、`{WHAT_WAS_IMPLEMENTED}` 等）
4. 将填充后的内容作为 `message` 参数，生成（spawn）一个 `worker` 代理

| 技能指令                                       | Codex 等效操作                                                                     |
| ---------------------------------------------- | ---------------------------------------------------------------------------------- |
| `Task 工具 (superpowers:code-reviewer)`        | 使用 `code-reviewer.md` 的内容调用 `spawn_agent(agent_type="worker", message=...)` |
| 带有内联提示词的 `Task 工具 (general-purpose)` | 使用相同的提示词调用 `spawn_agent(message=...)`                                    |

### 消息结构构建

`message` 参数属于用户级输入，而非系统提示词。请对其进行结构化处理，以确保模型最大程度遵循指令：

```
Your task is to perform the following. Follow the instructions below exactly.

<agent-instructions>
[filled prompt content from the agent's .md file]
</agent-instructions>

Execute this now. Output ONLY the structured response following the format
specified in the instructions above.
```

- 使用任务委派式结构（“你的任务是……”），而非角色设定式结构（“你是……”）
- 将指令包裹在 XML 标签中——模型会将带标签的块视为权威内容
- 以明确的执行指令结尾，以防止模型对指令进行总结

### 何时可以移除此变通方案

此方法用于弥补 Codex 插件系统尚未在 `plugin.json` 中支持 `agents` 字段的不足。当 `RawPluginManifest` 获得 `agents` 字段后，插件即可通过符号链接指向 `agents/`（与现有的 `skills/` 符号链接类似），技能便可以直接派发命名代理类型。

## 环境检测

涉及创建 worktree 或完成分支的技能，在继续操作前应使用只读的 git 命令检测当前环境：

```bash
GIT_DIR=$(cd "$(git rev-parse --git-dir)" 2>/dev/null && pwd -P)
GIT_COMMON=$(cd "$(git rev-parse --git-common-dir)" 2>/dev/null && pwd -P)
BRANCH=$(git branch --show-current)
```

- `GIT_DIR != GIT_COMMON` → 已处于已链接的 worktree 中（跳过创建）
- `BRANCH` 为空 → 处于 detached HEAD 状态（无法在沙盒中创建分支/推送/发起 PR）

请参阅 `using-git-worktrees` 的步骤 0 和 `finishing-a-development-branch` 的步骤 1，了解各技能如何利用这些信号。

## Codex App 收尾

当沙盒阻止分支/推送操作（在外部管理的 worktree 中处于 detached HEAD 状态）时，代理会提交所有工作，并提示用户使用 App 的原生控件：

- **“创建分支”**——为分支命名，然后通过 App UI 进行提交/推送/发起 PR
- **“交接至本地”**——将工作转移至用户本地的代码检出目录

代理仍可运行测试、暂存文件，并输出建议的分支名、提交信息和 PR 描述，供用户复制使用。
