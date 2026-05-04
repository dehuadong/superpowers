---
name: requesting-code-review
description: 在完成任务、实现主要功能或合并前验证工作是否符合要求时使用
---

# 请求代码审查

调度 superpowers:code-reviewer 子代理，在问题扩散之前将其捕获。审查者会获得精心构建的上下文以进行评估——绝不会包含你的会话历史。这使审查者专注于工作成果，而非你的思考过程，并保留你自己的上下文以便继续工作。

**核心原则：** 尽早审查，经常审查。

## 何时请求审查

**强制：**

- 在子代理驱动开发中的每个任务完成后
- 完成主要功能后
- 合并到主分支（main）之前

**可选但有价值：**

- 当陷入困境时（获取新鲜视角）
- 重构之前（基线检查）
- 修复复杂 bug 之后

## 如何请求

**1. 获取 git SHA：**

```bash
BASE_SHA=$(git rev-parse HEAD~1)  # 或 origin/main
HEAD_SHA=$(git rev-parse HEAD)
```

**2. 调度 code-reviewer 子代理：**

使用类型为 superpowers:code-reviewer 的 Task 工具，填写 `code-reviewer.md` 中的模板

**占位符：**

- `{WHAT_WAS_IMPLEMENTED}` - 你刚刚构建的内容
- `{PLAN_OR_REQUIREMENTS}` - 它应该实现的功能
- `{BASE_SHA}` - 起始提交
- `{HEAD_SHA}` - 结束提交
- `{DESCRIPTION}` - 简要摘要

**3. 根据反馈采取行动：**

- 立即修复关键（Critical）问题
- 在继续之前修复重要（Important）问题
- 记录轻微（Minor）问题以备后续处理
- 如果审查者有误，进行反驳（需提供理由）

## 示例

```
[刚完成任务 2：添加验证函数]

你：在继续之前，让我请求代码审查。

BASE_SHA=$(git log --oneline | grep "Task 1" | head -1 | awk '{print $1}')
HEAD_SHA=$(git rev-parse HEAD)

[调度 superpowers:code-reviewer 子代理]
  WHAT_WAS_IMPLEMENTED: 对话索引的验证和修复函数
  PLAN_OR_REQUIREMENTS: 来自 docs/superpowers/plans/deployment-plan.md 的任务 2
  BASE_SHA: a7981ec
  HEAD_SHA: 3df7661
  DESCRIPTION: 添加了 verifyIndex() 和 repairIndex()，包含 4 种问题类型

[子代理返回]：
  优点：架构清晰，测试真实
  问题：
    重要：缺少进度指示器
    轻微：报告间隔使用了魔术数字 (100)
  评估：可以继续

你：[修复进度指示器]
[继续任务 3]
```

## 与工作流的集成

**子代理驱动开发：**

- 在每个任务后进行审查
- 在问题累积之前捕获它们
- 在进入下一个任务之前修复问题

**执行计划：**

- 每批任务（3 个任务）后审查
- 获取反馈，应用反馈，继续

**临时开发：**

- 合并前审查
- 陷入困境时审查

## 警示信号

**切勿：**

- 因为“很简单”而跳过审查
- 忽略关键（Critical）问题
- 带着未修复的重要（Important）问题继续
- 与有效的技术反馈争辩

**如果审查者有误：**

- 提供技术理由进行反驳
- 展示证明其有效的代码/测试
- 请求澄清

参见模板：requesting-code-review/code-reviewer.md
