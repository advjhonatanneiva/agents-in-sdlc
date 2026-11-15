---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式人工智能（AI）时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解一些背景信息，就要确保它能获取这些上下文。[指令文件][instruction-files]可以提供指导，让 Copilot 不仅了解要做**什么**，也了解应该**如何**完成。

在本练习中，将：

- 了解项目特定上下文、编码指南和文档标准如何通过存储库自定义指令和按路径限定范围的指令文件提供给 Copilot。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。

> [!NOTE]
> 与 VS Code 和命令行界面（CLI）学习路径不同，本练习不会在更新指令前后各运行一次提示。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时并排展示指令的影响。后续练习中审查云智能体创建的拉取请求时，可以看到指令文件的影响。

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
- `.github/instructions/*.instructions.md`：可为特定任务或文件类型创建的指令文件。可以为 TypeScript 或 Astro 等特定语言提供规范，也可以针对创建 UI 组件或单元测试等任务提供指导。

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
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。注意它链接到其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件。这样可以将较大的指令集拆分为可复用的小文件，并让 Copilot 参考示例。（其中的路径相对于指令文件，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目的编码标准，但尚未要求为代码添加文档。接下来将添加 TSDoc 文档注释和文件注释头的规则。

## 添加新的存储库标准

接下来要在这里编辑的唯一内容，是添加项目级规则，要求通过 TSDoc 文档注释和文件级注释头为代码添加文档。云智能体会在本学习路径后续练习中处理分配给它的议题时使用此规则。

编辑前先创建用于工作的分支（云智能体会从议题的目标分支读取指令，但仍建议单独提交这些更改）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。接下来将编码标准写入存储库，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，预计位于第 27 行附近。注意该部分记录了项目的编码标准，但尚未包含代码文档规则，这正是生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级文件（`.github/copilot-instructions.md`）中设置全局指导，也可以使用 `*.instructions.md` 文件为特定语言、文件类型或任务提供指导。存储库级文件适合放置整个项目都适用的标准，例如刚添加的文档注释规则。

## 提交、推送并合并指令更新

云智能体会读取议题目标分支中的指令文件。下一练习中将议题分配给 Copilot 时，它会从 `main` 分支创建新分支。因此，必须先将指令更改合并到 `main`，云智能体才能读取。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求，并合并该请求。下一练习中，云智能体处理分配的议题时便会读取这些指令。

> [!TIP]
> 如果希望在本工作坊中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。此处创建分支是为了让工作坊更接近真实项目的处理方式。

## 总结和后续步骤

开发者了解了 Copilot 如何获取此项目的指令文件上下文，并在 `.github/copilot-instructions.md` 中添加了新的存储库级标准。后续练习中，可以在云智能体生成的拉取请求中看到该标准的应用。

接下来，[将首个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
