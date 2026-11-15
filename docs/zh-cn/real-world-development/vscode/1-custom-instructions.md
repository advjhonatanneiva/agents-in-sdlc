---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：代理模式 →][next-lesson]

使用生成式人工智能（AI）时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解一些背景信息，就要确保它能获取这些上下文。可以通过多种方式向 Copilot 提供特定上下文，其中一种重要方式是使用[指令文件][instruction-files]，指导 Copilot 如何生成代码。

在本练习中，将：

- 了解项目特定上下文、编码指南和文档标准如何通过存储库自定义指令和按路径限定范围的指令文件提供给 Copilot。
- 使用当前生效的指令生成首个筛选数据切片（发行商辅助函数）。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。
- 重新运行相同提示，观察生成的代码如何采用新标准。
- 将指令更新和筛选数据切片提交到 `main`，以便 Copilot 在下一练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不符合某些设定的标准。Copilot 具有概率性，并非确定性工具。本练习旨在观察更新指令前后行为的*趋势*，而不是要求输出逐字一致。

## 指令文件

### 场景

Tailspin Toys 制定了一系列开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- 用户界面（UI）应采用深色模式并具备现代感。
- 应通过 TypeScript 文档注释（TSDoc）为代码添加文档。
- 每个文件开头都应添加一段注释，说明文件的用途。

通过使用指令文件，可以确保 Copilot 在执行任务时遵循这些实践要求。

### 自定义指令

自定义指令可以向 Copilot 提供上下文和偏好，帮助它理解开发者的编码风格和要求。开发者可以通过指令引导 Copilot 提供更符合需求的建议和代码片段，包括指定编码规范、库以及注释风格。指令可以应用于整个存储库，也可以应用于特定类型文件的任务级上下文。

指令文件有两种：

- `.github/copilot-instructions.md`：单个指令文件，会随存储库的**每个**请求发送给 Copilot。此文件应包含对大多数聊天或 CLI 请求都适用的项目级信息，例如所用技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可为特定任务或文件类型创建的指令文件。可以为 TypeScript 或 Astro 等特定语言提供规范，也可以针对创建用户界面（UI）组件或单元测试等任务提供指导。

> [!NOTE]
> 在集成开发环境（IDE）中工作时，指令文件只用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，都会使用存储库级指令文件和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot [还支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

关于创建指令文件的完整讨论超出了本练习的范围。不过，示例项目展示了具有代表性的方法。总体而言：

- 将 `copilot-instructions.md` 中的指令集中在项目级指导上，例如项目说明、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（如单元测试、Astro 组件和数据层）或特定任务提供指导。
- 使用自然语言，保持说明清楚，并提供代码应该如何（以及不应该如何）编写的示例。

指令文件没有唯一的正确写法，AI 的使用方式也不止一种。通过实践可以找到适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应具备完善的指令文件集合。查看本项目中的文件时，可能会发现有针对多种任务的指令，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成指令文件。不同产品界面的操作方式各不相同（例如，VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`）。当前工作环境的课程会说明相关操作。
>
> 需要模板或起点？可以查看 [awesome-copilot][awesome-copilot]，其中收录了指令文件、自定义代理和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看项目中的自定义指令文件

花些时间阅读此存储库自带的指令文件：一个核心 `copilot-instructions.md` 文件，以及一组针对不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web UI 中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。在 **Code standards** 下，留意嵌套的 **GitHub Actions Workflows** 指南。这些内容适用于与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并查看其中的内容。这里有针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置了一个相对于存储库根目录的 glob，用于决定适用的文件。这里，任何 TypeScript 测试文件（例如匹配 `**/*.test.ts` 的文件）都符合条件。
5. 查看此项目专门用于创建单元测试的指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。注意它链接到其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件。这样可以将较大的指令集拆分为可复用的小文件，并让 Copilot 参考示例生成代码。（其中的路径相对于指令文件，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目的编码标准，但尚未要求为代码添加文档。接下来将添加 TSDoc 文档注释和文件注释头的规则。

## 为更改创建分支

接下来遵循最佳实践，为更改创建分支。

1. 返回上一练习中使用的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新的终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板，并选择 **New Chat** 开始干净的新对话。模式和模型选择因练习而异；每个步骤会在相关处说明。

要观察自定义指令的影响，先在当前指令文件生效时发送提示。之后更新指令，再次发送相同提示，比较结果。

1. 关闭之前练习留下的编辑器选项卡，让 Copilot 只获取需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应该放在哪里。
3. 在 Chat 视图的代理下拉菜单中选择 **Agent**，以便 Copilot 应用文件更改。

   ![显示 Chat 视图中代理选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常涉及辅助函数文件及其测试。
6. 注意，建议的辅助函数是一个带类型的函数，它将 `db` 客户端作为第一个参数，并返回带类型的发行商数组。这些约定来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层规范。
7. 注意，建议的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有概率性，因此它也可能在没有明确指示的情况下添加文档注释。这种情况也没关系，本练习的重点是观察更新指令后行为的一致性改进。

## 添加新的存储库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。接下来将编码标准写入存储库，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，预计位于第 27 行附近。注意该部分记录了项目编码标准，但尚未包含代码文档规则，这正是生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级文件（`.github/copilot-instructions.md`）中设置全局指导，也可以使用 `*.instructions.md` 文件为特定语言、文件类型或任务提供指导。存储库级文件适合放置整个项目都适用的标准，例如刚添加的文档注释规则。

## 重新运行提示并观察更改

1. 返回 Copilot Chat，选择 **New Chat** 清除对话内容。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦于正确的文件。
3. 发送与之前相同的提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意，现在建议文件以类似以下内容的注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 注意，建议函数现在包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

刚刚引导 Copilot 遵循新的项目标准，并将其应用于真实代码。下一练习将在此基础上继续。

## 提交指令并推送分支

指令文件与其他项目资源一样，应使用相同的源代码管理流程管理。接下来提交并推送分支到存储库。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新的终端窗口。
2. 在终端中运行以下命令，确认指令更新和辅助函数更改都已存在：

   ```bash
   git status
   ```

3. 在终端中暂存并提交指令更新及 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送后，应创建拉取请求，并关联待办事项中的文档标准议题。也可以手动操作，不过 Copilot 可以使用已连接到项目的 GitHub 工具代为完成。请 Copilot 找到议题、创建用于关闭该议题的拉取请求（PR），然后合并。

> [!NOTE]
> 这是第一次让 Copilot 代表开发者操作 GitHub。项目模板已将 GitHub 工具连接到工作区，因此无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示登录 GitHub。按照提示完成登录即可。后续练习将介绍这种连接的工作方式（Model Context Protocol）。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保在代理选择器中选中 **Agent**。
3. 请 Copilot 查找与更新指令文件相关的议题，并从当前分支创建 PR，说明指令更新和新的发行商辅助函数基础：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找议题并创建 PR。
5. 系统提示 **Allow** Copilot 代表开发者执行 GitHub 操作时，请先审查命令，再酌情选择 **Allow**。
6. PR 创建后，请 Copilot 合并 PR，并将本地分支切回 main。使用以下提示：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 系统提示 **Allow** Copilot 代表开发者执行 GitHub 操作和 shell 命令时，请先审查命令，再酌情选择 **Allow**。

现在已经在 GitHub Copilot 的帮助下创建并合并了拉取请求。

## 总结和后续步骤

开发者了解了 Copilot 如何获取此项目的指令文件上下文，并在 VS Code 中使用 Copilot Chat：

- 发送代码生成提示，观察 Copilot 在现有指令下生成的内容。
- 在 `.github/copilot-instructions.md` 中添加了新的存储库级标准。
- 重新运行相同提示，观察建议的代码如何采用新标准。
- 将指令更新和辅助函数基础提交到 `main`，以便下一练习在此基础上继续。

接下来，在[代理模式][next-lesson]中运用这些指令，让 Copilot 在整个代码库中添加新功能。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot — 指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
