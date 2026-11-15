---
title: "第 8 课 - 创建并合并功能 PR"
description: "一并审查筛选功能、指令、技能更新、QA 配置文件和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、指令更新、技能更新、质量保证（QA）配置文件和测试都保存在同一个分支中。现在可以一起审查这些更改并打开拉取请求。之前由开发者亲自合并了星级评分拉取请求（PR），这次将使用 **Agent Merge** 管理合并流程。

> [!NOTE]
> 通常会将功能、指令更新、技能更新和 QA 代理分别放在几个 PR 中。为简化本工作坊，筛选功能及完整质量工作流都保存在同一个对话和分支中，并通过此 PR 提交。

在本课中，将：

- 了解 Agent Merge 的作用及其如何自动化合并生命周期。
- 检查完整的功能更改和验证证据。
- 创建筛选功能 PR。
- 仅在审查后启用 Agent Merge，并确认 PR 已合并。

## 场景

在筛选功能工作流中，开发者使用 Copilot 规划、实现并验证了一项功能。Tailspin Toys 现在希望自动化剩余 PR 工作，同时仍由开发者控制合并授权。

## 认识 Agent Merge

**Agent Merge** 会自动处理合并拉取请求所需的剩余工作。启用后，Copilot 会处理阻碍 PR 合并的问题，例如修复失败的持续集成（CI）检查、回复审查意见，以及在需要时进行变基，然后在存储库允许的情况下启用 GitHub 自动合并。

到目前为止，开发者都是自行选择 **Merge pull request**。Agent Merge 可以接手这项工作。启用 Agent Merge 前，请先审查工作并确认已准备就绪。

## 使用 Agent Merge 管理 PR

所有代码都已完成，现在可以一起审查、创建 PR，并让 Agent Merge 管理后续流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开代理对话框。
3. 从选项列表中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 使用命令 `/pr create` 创建新的 PR。
5. 使用 `/pr agentmerge` 激活 Agent Merge。
6. Copilot 将监视 PR 的持续集成流程。所有检查成功后，它会执行合并。
7. 确认 Copilot 显示类似“PR #14 was squash-merged successfully.”的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需审批、分支保护、合并队列、存储库设置或权限不足问题。如果流程受阻，请阅读报告的原因，并在存储库允许时手动完成已审查的合并。

## 总结和后续步骤

开发者已经自动化了开发流程的多个环节，包括生成代码、测试和验证代码，以及处理拉取请求。具体来说，开发者：

- 了解了 Agent Merge 的作用及其如何自动化合并生命周期。
- 检查了完整的功能更改和验证证据。
- 创建了筛选功能 PR。
- 仅在审查后启用了 Agent Merge，并确认 PR 已合并。

接下来，[探索更多实用的 Copilot CLI 斜杠命令][next-lesson]，了解上下文、模型、共享和可选的云委派。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
