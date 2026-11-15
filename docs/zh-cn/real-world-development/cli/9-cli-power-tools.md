---
title: "第 9 课 - GitHub Copilot CLI 中的斜杠命令"
description: "探索用于管理上下文、选择模型、共享会话和可选云委派的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

与其他优秀的 CLI 工具一样，GitHub Copilot CLI 提供了许多斜杠命令。它们可以访问高级功能、幕后信息和其他配置选项。之前已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令。接下来探索其他实用命令。

在本课中，将：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 探索可用模型。
- 了解如何使用 `/share` 导出或共享会话。
- 探索用于并行工作、工作树和委派任务给云智能体的可选命令。

## 场景

核心 CLI 工作流已完成。现在探索一些其他功能：管理上下文、切换模型、共享会话，以及选择是否将工作委派给 [Copilot 云智能体][about-cloud-agent]。

## 探索 Copilot CLI 上下文

处理规模较大或较复杂的任务时，可能会达到模型的最大上下文窗口。Copilot CLI 会在需要时自动压缩对话；也可以通过斜杠命令检查上下文或手动压缩。

1. 返回 Codespace。如果 Copilot CLI 尚未打开，请在存储库根目录中启动它。
2. 输入：

   ```plaintext
   /context
   ```

3. 查看模型、当前 token 用量，以及系统指令、工具、消息和剩余空间之间的上下文分配。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话原本就很短，变化可能并不明显。

> [!NOTE]
> 上下文窗口接近上限时，Copilot CLI 会自动压缩上下文。想自行决定压缩时机时，可以使用 `/compact`。切换到无关任务并希望开启新对话时，可以使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有优势，不同开发者也有各自偏好。Copilot CLI 可列出并选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 查看可用模型和用量信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队合作和分享经验有助于大家提升人工智能（AI）工具的使用水平。`/share` 命令可以将会话导出为 Markdown 或超文本标记语言（HTML）文件、创建可共享链接，或发布为 GitHub gist。

1. 输入 `/help`，查看当前安装版本中 `/share` 的选项。
2. 如果要共享此会话，请选择合适的目标，例如 `/share file`，将其导出为本地 Markdown 文件。
3. 发送或发布导出内容前，请先审查内容。会话导出可能包含提示、响应和项目详情。

发布链接或 gist 是可选操作。不要公开团队不打算共享的存储库或对话内容。

## 可选：扩展并行工作或委派任务

本工作坊的核心流程已完成。Copilot CLI 还为规模较大的任务提供了以下命令：

- `/fleet` 可以将独立子任务分配给子代理并行处理。
- `/worktree` 可以为单独的任务创建隔离的 Git 工作树。
- `/delegate` 可以将任务委派给 Copilot 云智能体，由其异步工作并可能创建拉取请求。

这些命令是可选的，因为它们可能会创建其他工作树或远程工作。尝试前，请启动范围明确的新任务，并通过常规工作流审查结果。想深入了解异步代理工作，可继续学习[云智能体研讨会][cloud-workshop]。

## 总结和后续步骤

使用 Copilot CLI 斜杠命令可以对其进行配置、共享会话并查看幕后工作。在本课中，开发者：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 探索可用模型。
- 了解了如何使用 `/share` 导出或共享会话。
- 探索了用于并行工作、工作树和委派任务给云智能体的可选命令。

Copilot CLI 还有更多斜杠命令和功能值得探索。最后，[回顾所学内容][next-lesson]，并了解后续学习步骤。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot 云智能体][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
