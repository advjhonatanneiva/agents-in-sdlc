---
title: "第 3 课 - 代理模式：Plan 和 Autopilot"
description: "使用 Plan 确定方案、使用 Autopilot 根据议题构建筛选功能，并在 Interactive 模式中审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

首先，我们为项目添加了一项小功能。较大的更改则需要更完善的流程。GitHub Copilot CLI 能够融入组织现有的工作流，帮助开发者以正确方式构建正确功能。本课是多个课程中的第一课，将介绍典型的代理驱动开发流程：从议题实现新功能，确认代码有效、功能符合预期，最终成功合并到项目中。

> [!NOTE]
> 继续完成此功能工作流时，会使用同一对话和分支。通常会为要处理的不同文件类型创建不同分支或拉取请求（PR），但本工作坊会略过这些步骤，以便专注于核心概念。

本课将：

- 从 GitHub 议题开始新的 Copilot 对话。
- 在 Plan 模式中确定需求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

继续完善此功能时，将更新存储库指令、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 代理并打开功能拉取请求（PR）。

## 场景

Tailspin Toys 的目录不断扩大，访客需要按类别和发行商筛选游戏。待办事项中的议题描述了该功能，但组合多个类别等细节需要先达成一致。开发者将使用 Plan 模式确定这些决策，然后授权 Autopilot 在明确范围内实现功能。

## 背景

在开发流程中引入人工智能（AI）编码代理，并不会改变基本原则。事实上，这些原则变得更加重要。多数开发者遵循的流程大致如下：

1. 从已创建、描述待办工作的议题开始。
2. 制定构建方案。
3. 编写并审查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码通过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程因团队和组织而异，但大多数流程都与上述步骤类似。

遵循这一标准流程，可以确保 AI 生成的代码满足需求，并像手动编写的代码一样经过审查。

## 对话模式

**对话模式**决定代理拥有多大自主权。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在不同模式间切换：

- **Interactive**：开发者与代理协作。代理提出更改建议，并等待开发者输入后再继续。
- **Plan**：代理先制定计划，但不能编辑项目文件。
- **Autopilot**：代理自主工作，编写代码、运行测试并不断迭代，直至任务完成。

从 Plan 模式开始，审查计划，然后使用 Autopilot 实现计划。

## 从议题开始

开始筛选工作前，先返回 Codespace，并确保存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后创建筛选功能分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题添加到提示中，然后返回 **Session** 选项卡。

注意，提示现在以 `#7`（或类似编号）开头。`#` 可将 GitHub 上的议题或拉取请求（PR）加入上下文。

## 规划筛选功能

在实现功能或执行任务前先规划，可以确定方案。对于复杂任务，花些时间规划通常很有帮助。接下来切换到 Plan 模式，请 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的议题引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会开始制定计划：先探索项目，再确定最佳方案。

3. Copilot 可能会询问筛选功能的工作方式。根据偏好回答，没有标准答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 上下滚动审查计划。
6. 如果计划中的任何部分不符合已作出的决定，请要求 Copilot 修改。

## 批准 Autopilot

计划撰写并审查完毕后，就可以开始实现。接下来使用 Autopilot，让 Copilot 开始工作。

Autopilot 会让 Copilot 持续迭代问题，直到它认为任务已经完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或当前安装版本中含义相近的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 如何按照既定计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再将检查视为已完成。

## 审查并验证实现

生成代码后，像合并任何其他代码一样，先审查代码。接下来同时审查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能实现和测试。
3. 将结果与议题及规划时作出的决定进行比较。
4. 审查代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 审查项目检查的输出，并要求 Copilot 解决任何失败项。

## 试用新功能

代码看起来不错，但能否正常运行？像之前一样启动应用，并通过 Codespaces 转发的端口打开网站。

1. 请 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试类别筛选、发行商筛选，以及计划中约定的筛选组合。
4. 确认重置和无结果时的行为符合议题要求和已作出的决定。
5. 返回 Codespace，并要求 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

开发者已经使用不同的对话模式构建并审查功能。在本课中，开发者：

- 从 GitHub 议题开始新的 Copilot 对话。
- 在 Plan 模式中确定需求。
- 使用 Autopilot 模式实现新功能。
- 审查了代码。
- 在转发的浏览器中手动验证了功能。

接下来，通过[使用自定义指令][next-lesson]，深入了解如何生成符合文档规范的代码。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
