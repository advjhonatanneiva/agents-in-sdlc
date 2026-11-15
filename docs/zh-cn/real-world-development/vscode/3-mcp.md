---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：代理模式][previous-lesson] · [下一课：自定义代理 →][next-lesson]

刚刚使用代理模式构建了筛选功能。打开拉取请求之前，应先确认功能在浏览器中正常运行。本练习将连接 **Playwright MCP 服务器**（MCP 即 Model Context Protocol，模型上下文协议），让 Copilot 操作真实浏览器测试功能，然后发布分支并打开拉取请求（PR），而无需亲自逐项点击应用。

在本练习中，将：

- 了解 Model Context Protocol（MCP）是什么，以及 MCP 服务器如何为 Copilot 扩展工具。
- 将 Playwright MCP 服务器添加到工作区。
- 请 Copilot 使用 Playwright MCP 在浏览器中手动测试筛选功能。
- 发布分支，并为筛选功能工作打开拉取请求。

## 什么是 Model Context Protocol（MCP）？

当代理模式可以访问编辑器以外的资源时，功能会大幅增强。Model Context Protocol（MCP）是 Copilot 用来实现此目的的标准方式，允许代理与外部工具和服务通信。

![代理模式内部工作原理示意图，以及它与上下文、大语言模型（LLM）和工具之间的交互，包括由 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol（MCP）](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/)让人工智能（AI）代理能够与外部工具和服务通信。使用 MCP 后，AI 代理可以实时访问最新信息（使用资源），并代表开发者执行操作（使用工具）。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 代理与外部工具和服务之间的桥梁，负责管理代理与外部工具（例如现有应用程序编程接口（API）或 npm 包等本地工具）之间的通信。每个 MCP 服务器都代表 AI 代理可以访问的一组工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供一组用于管理 GitHub 存储库的 API。AI 代理可以使用它创建存储库、更新现有存储库以及管理议题和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化能力。AI 代理可以通过它浏览网页、填写表单和选择按钮。

还有许多其他 MCP 服务器可以提供对不同工具和资源的访问。GitHub 提供 [MCP 注册表](https://github.com/mcp)，帮助开发者发现资源并参与生态建设。

> [!CAUTION]
> 从安全角度来看，对待 MCP 服务器应与对待项目中的其他依赖项一样。使用前请仔细审查其源代码、验证发布者，并考虑安全影响。只使用信任的 MCP 服务器，并谨慎授予其访问敏感资源或执行敏感操作的权限。

## 检查 MCP 配置

`.vscode/mcp.json` 文件用于配置 VS Code 工作区可用的 MCP 服务器。

1. 在 codespace 中打开 `.vscode/mcp.json`。
2. 应该会看到已配置好的 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

`github` 条目由项目模板提供，因此上一练习中 Copilot 可以读取待办事项中的议题。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装任何内容。VS Code 通过 HTTP 连接到服务器，并在 Copilot 首次使用其工具时使用 GitHub 身份验证。

## 添加 Playwright MCP 服务器

接下来添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控的浏览器，可用于测试功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 条目，与 `github` 条目并列，最终文件如下：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会在本地运行 `npx @playwright/mcp@latest` 来启动服务器。`--headless` 标志会让 Playwright 在后台运行浏览器而不显示窗口。由于 codespace 没有可显示浏览器的桌面环境，因此需要此选项。

> [!NOTE]
> Tailspin Toys 项目已使用 Playwright 运行端到端测试，因此通常已安装 Playwright 所需的浏览器。如果 Copilot 后续报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会按需启动 MCP 服务器，也就是 Copilot 首次需要服务器工具时才启动，并要求确认是否信任该服务器。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会出现 **Start** 操作。选择它立即启动服务器，并确认配置正确。
2. 如果 VS Code 询问是否信任该服务器，请审查配置并选择信任，以便服务器启动。

手动启动是可选的。如果跳过此步骤，VS Code 会在下一步中 Copilot 首次需要相关工具时启动服务器，并提示是否信任。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可操作的真实浏览器。开发者无需亲自点击应用检查工作，代理可以打开页面、导航、应用筛选、读取结果并总结观察情况。无需离开对话即可快速确认功能是否符合预期。

Playwright MCP 服务器通过页面的[无障碍树][playwright-mcp-server]而不是屏幕截图进行操作。这意味着代理会分析结构化且带标签的元素（按钮、链接和列表项），与辅助技术的处理方式相同。因此，快速功能检查也能顺便进行简单的无障碍检查。

连接服务器并启动应用后，请 Copilot 测试刚构建的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器，逐步执行这些操作并报告结果。根据议题中的验收标准审查报告。如果发现问题，请提出后续问题，或要求 Copilot 在打开 PR 前修复代码。

> [!NOTE]
> 测试时应用必须运行在 `http://localhost:4321`。如果开发服务器已停止，请在发送提示前重新启动。Copilot 首次使用 Playwright MCP 服务器时，可能需要下载浏览器。如果提示缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以打开拉取请求（PR），让团队审查。首先发布 `filtering-vscode` 分支。

1. 在 codespace 中打开 **Source Control** 面板，审查 Copilot 所做的更改。
2. 选择 **+** 暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![显示更改内容的 Source Control 面板屏幕截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

创建拉取请求的方式有多种，包括通过 github.com 和 GitHub 命令行界面（CLI）创建。不过，既然已经在使用 GitHub Copilot，不妨让它代为创建 PR。它可以找到相关议题，并将 PR 与议题关联起来。

1. 打开 Copilot Chat 面板，并选择 **New Chat** 开始新会话。
2. 确认在代理下拉菜单中选择 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 请 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 执行获取信息和操作所需的任务。首次使用 GitHub 工具时，系统可能会提示使用 GitHub 身份验证，请按照提示操作。
5. 查看 Copilot 如何搜索议题、找到相关议题并创建 PR。
6. 选择 Copilot 生成的链接审查拉取请求，但**暂时不要合并**。

## 总结和后续步骤

恭喜，在本练习中，开发者：

- 了解了 Model Context Protocol（MCP）是什么，以及 MCP 服务器如何为 Copilot 扩展工具。
- 将 Playwright MCP 服务器添加到了工作区。
- 使用该服务器在发布前通过浏览器手动测试了筛选功能。
- 发布了分支，并为筛选功能工作打开了拉取请求。

已经使用 MCP 服务器**测试**了一项功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，[创建自定义代理][next-lesson]，简化无障碍审查等专注任务。

## 资源

- [MCP 到底是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
