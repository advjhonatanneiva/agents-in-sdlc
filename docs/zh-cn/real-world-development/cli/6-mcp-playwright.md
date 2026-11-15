---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中检查或配置 Playwright MCP，并使用它在浏览器中试用筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

正如前面所说，编写代码不只是编写代码。还需要处理数据、外部服务，并让 Copilot 可以使用更多自动化工具。模型上下文协议（Model Context Protocol，MCP）服务器可以让 Copilot 超越 CLI 内置功能，使用更多工具和服务。

在本课中，将：

- 了解 Model Context Protocol（MCP）是什么，以及 Copilot CLI 如何使用它。
- 如果尚未添加 Playwright MCP 服务器，则进行添加。
- 请代理操作浏览器并试用筛选功能。

## 场景

单元测试和端到端测试固然重要，但验证用户界面（UI）更新还需要实际交互。开发者希望 Copilot 能像用户一样使用正在开发的网站，以自动验证更改，并提高对预期行为的信心。

## 什么是 Model Context Protocol（MCP）？

[Model Context Protocol（MCP）][mcp-blog-post]让人工智能（AI）代理能够与外部工具和服务通信。使用 MCP 后，AI 代理可以实时访问最新信息，并代表开发者执行操作。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 代理与外部工具和服务之间的桥梁。每个 MCP 服务器都代表 AI 代理可以访问的一组工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供用于管理 GitHub 存储库、议题和拉取请求的应用程序编程接口（API）。
- **[Playwright MCP Server][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化能力。

还有许多其他 MCP 服务器。GitHub 提供 [MCP 注册表][mcp-registry]，帮助开发者发现资源并参与生态建设。

> [!CAUTION]
> 对待 MCP 服务器应与对待项目中的其他依赖项一样。使用前请审查其源代码、验证发布者，并考虑安全影响。

## 添加 Playwright MCP 服务器

如果 Copilot CLI 尚未配置 Playwright MCP 服务器，请添加它，以便 Copilot 像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 已配置 Playwright MCP，请跳过这些设置步骤，继续下一节。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 输入名称 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴以下命令：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 请 Copilot 使用 Playwright 探索功能

之前已手动确认功能符合预期。现在使用 Copilot 中的 Playwright MCP 服务器重复验证。

1. 使用以下提示，请 Copilot 使用 Playwright MCP 服务器验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常无需明确要求 Copilot 使用 MCP 服务器，它一般会自行判断。但既然知道该使用什么工具，明确指出也有好处，可以让结果更一致，并节省一些 token。

2. 观察 Copilot 如何列出在浏览器中执行的步骤，以确认功能正常。
3. 阅读报告，确认一切符合预期。

Copilot 会启动服务器，使用 Playwright 操作网站，停止服务器，并提供报告。

## 总结和后续步骤

恭喜，开发者已经使用 Playwright MCP 服务器，通过 Copilot CLI 在真实浏览器中试用了功能。回顾一下，开发者：

- 了解了 Model Context Protocol（MCP）是什么，以及 Copilot CLI 如何使用它。
- 如果尚未添加 Playwright MCP 服务器，则进行了添加。
- 请代理操作浏览器并试用了筛选功能。

接下来，[创建 QA 自定义代理][next-lesson]，将技能与浏览器工具结合到专用角色中。

## 资源

- [MCP 到底是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [向 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
