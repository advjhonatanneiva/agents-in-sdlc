---
title: "练习 2 - 使用 Copilot 代理模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行测试等操作。作为开发者，工作流程通常包括：找到所有相关文件、进行修改、运行测试、调试、找出遗漏的文件、再次修改……类似步骤还有很多。

Copilot 代理模式正是为此而设计的。

Copilot 代理模式可以在集成开发环境（IDE）中更自主地工作。它的行为类似开发者：先探索现有项目结构，执行必要的更新，运行测试等任务，并自动修复发现的问题。接下来了解如何使用代理模式为网站添加新功能。

> [!NOTE]
> 尽管名称相似，代理模式和云智能体针对的是两种不同的体验。代理模式在 IDE 中执行任务，便于快速反馈和交互。云智能体则像团队中的协作开发者一样异步工作，通过议题和拉取请求与你互动。

在本练习中，将了解如何：

- 使用 Copilot 代理模式探索项目、找到相关文件并协调更改。
- 使用 GitHub Copilot 代理模式跨用户界面（UI）和数据层实现新功能。
- 在将代码合并到代码库之前，审查 GitHub Copilot 代理模式生成的更改和测试。

## 场景

随着游戏列表不断扩大，开发者希望用户可以按类别和发行商筛选。上一练习已添加发行商辅助函数，现在将使用 Copilot 代理模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

进行任何更改前，先探索 Tailspin Toys 网站，了解它目前的功能。

该网站是一个以开发者为主题的桌游众筹平台。用户可以浏览游戏列表并查看详情。它是一个单独的 Astro 应用，会在构建时将页面渲染为静态超文本标记语言（HTML）。页面通过 Drizzle 对象关系映射（ORM）在 frontmatter 中直接查询本地 SQLite 数据库，没有独立的后端应用程序编程接口（API）或客户端用户界面（UI）框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能通过使用文档对象模型（DOM）API 的小型 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。按照以下步骤，可以在 GitHub Codespace 中运行该任务：

1. 返回 codespace。上一练习结束后，应该已切回 `main`。
2. 在 Codespace 中按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux），打开新的终端窗口。
3. 创建并切换到用于筛选功能的新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下内容的横幅，其中包含 URL：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否在浏览器窗口中打开 `http://localhost:4321`，请选择 **x** 将其关闭。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 使用 codespace 时，选择 Codespace 终端中的 localhost URL 后，系统会自动将其重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是连接到 codespace 的专用隧道，网站服务器现在就在其中运行。

### 浏览网站

网站运行后，可以浏览其功能。主要功能包括：

- **主页**：显示桌游列表、游戏标题、图片和说明。
- **游戏详情页**：选择某款游戏后，会打开包含更多信息的详情页，例如标题、说明、发行商和类别。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板，并选择 **New Chat** 开始干净的新对话。模式和模型选择因练习而异；每个步骤会在相关处说明。

网站的初始实现已经可以正常工作，但还需要添加新功能。先查看待办事项。使用模板创建项目时，系统已经自动创建了一组议题。请 GitHub Copilot 列出这些议题。

1. 在 Chat 视图的代理下拉菜单中选择 **Agent**。**Agent** 代理可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![显示 Chat 视图中代理选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并非推荐某个模型。编写本工作坊时使用的是 Claude Sonnet 4.5，因此教程包含了该模型，以便代码建议尽可能一致。不过，由于大语言模型具有概率性，收到的建议可能与教程所述不同。这完全正常，也在预期之内。

> [!NOTE]
> 由于大语言模型具有概率性，Copilot 可能会使用不同的模型上下文协议（Model Context Protocol，MCP）命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，询问待办事项中的议题：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 查看生成的议题列表。

注意，Copilot 还根据它对用户价值的判断对议题进行了优先级排序。

## 审查指令文件

让代理生成代码前，先审查用于为 Copilot 提供上下文的指令文件。接下来使用[用户界面（UI）](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)指令文件，其中包含向网站添加功能的方法说明。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看整体指导内容，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向各类文件专用指令的链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指令文件可以引用其他指令文件和项目中的文件。路径相对于指令文件所在位置，因此可以复用指令、将复杂内容拆分为易于管理的小块，并提供示例和模板供 Copilot 参考。

## 实现筛选功能

要完成筛选功能，至少需要对应用进行以下三项更新：

- 在数据层（`src/lib/`）中添加或完善剩余的筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页面，加入筛选 UI。

此外，合并前需要运行并通过测试。Copilot 代理模式可以完成这些任务，接下来让它实现功能。

1. 可以继续使用当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 指令文件。

   ![显示选择 UI 指令文件示例的屏幕截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的代理下拉菜单中仍然选择了 **Agent**。

   ![显示 Chat 视图中代理选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认当前模型仍为 **Claude Sonnet 4.5**。
5. 使用以下提示，请 Copilot 根据待办事项中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目并找到相关文件。它应该会找到数据层辅助函数、UI 和测试，然后修改文件并运行测试。

   ![显示 Copilot 正在探索项目文件的屏幕截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂度，这可能需要几分钟。在此过程中，代码编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会显示 **Keep** 或 **Undo** 按钮，因此无需在工作进行时选择这些按钮。

7. Copilot 提示运行测试时，选择 **Continue**。

   ![Copilot Chat 窗格中询问用户是否允许运行测试的对话框屏幕截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会出现停顿，甚至有测试失败。这没有关系。Copilot 会在生成代码和运行测试之间反复迭代，直到完成任务且不再检测到错误。

   ![Copilot 代理模式完整聊天会话的屏幕截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查看是否存在潜在问题。

> [!CAUTION]
> 务必审查 Copilot 或其他人工智能（AI）工具生成的代码。

10. 返回运行着网站的浏览器，试用新功能。
11. 确认一切正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜，在本练习中，开发者了解了如何使用 GitHub Copilot 代理模式为 Tailspin Toys 网站添加新功能，具体包括：

- 使用 GitHub Copilot 代理模式跨 UI 和数据层实现新功能。
- 使用 Copilot 代理模式探索项目、找到相关文件并协调更改。
- 在将代码合并到代码库之前，审查 Copilot 代理模式生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 加分探索练习 - 实现分页

随着游戏列表不断扩大，需要支持分页。运用本练习中学到的技能，请 Copilot 更新网站以实现分页。编写提示时可以考虑：

- 遵循现有最佳实践，包括使用现有指令文件。
- 考虑分页的实现方式，例如是否允许用户选择每页大小，还是使用固定值。
- 编写提示时，为 Copilot 提供实现所需的指导。
- 可能需要与 GitHub Copilot 反复协作，要求调整并提供上下文。这是使用 Copilot 的正常流程。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能与适用时机][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
