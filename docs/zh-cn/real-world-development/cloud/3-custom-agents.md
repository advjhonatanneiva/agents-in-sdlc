---
title: "练习 3 - 自定义代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义代理？

GitHub Copilot 中的[自定义代理][custom-agents-concept]是针对开发工作流中特定任务或领域定制的专用人工智能（AI）助手。通过在存储库的 `.github/agents` 文件夹中使用 Markdown 文件定义代理，可以提供专注的指令、最佳实践、编码模式和领域知识，帮助 Copilot 更有效地完成特定类型的工作。团队可以将专业经验编码为可复用代理，例如强制遵循《Web 内容无障碍指南》（WCAG）[规范][wcag]的无障碍代理、遵循安全编码实践的安全代理，或保持测试模式一致的测试代理。

项目的 `.github/agents` 文件夹中使用 Markdown 文件定义自定义代理，也可以在全局 `~/.copilot/agents` 中定义。每个文件都包含 YAML frontmatter，其中至少要有 `name` 和 `description`，后面是定义代理行为、专业知识和指令的 Markdown 提示。

### 自定义代理与代理技能

自定义代理和[代理技能][agent-skills-concept]之间存在一定的逻辑重叠。两者主要通过 Markdown 文件定义，并告诉 AI 如何执行操作。最简单的区分方式是：**自定义代理是工作者，技能是工具**。

自定义代理有各自的上下文窗口，能够在工作时协调技能（甚至其他代理）。在本实验中，无障碍自定义代理会根据无障碍指南审查并更新网站；执行过程中，它可以调用拉取请求工作流技能或用于运行和管理测试的技能。

> [!NOTE]
> 自定义代理没有唯一的正确写法。与其他 AI 用法一样，应在不同环境和场景中测试并不断迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

接下来使用自定义代理：

- 了解如何定义自定义代理。
- 将任务分配给自定义代理。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，不受视觉能力或偏好的影响。最近的用户反馈指出，当前深色主题中的文字与背景颜色对比度不足，导致部分用户难以阅读。为了解决这个无障碍问题，设计团队要求实现可由用户开启或关闭的高对比度模式。

无障碍至关重要，因此希望尽快实现此功能。接下来使用自定义代理生成实现。

## 审查无障碍自定义代理

已为无障碍工作创建了自定义代理。返回 codespace，查看其内容，了解代理将如何引导 Copilot。

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义代理必须包含 `name` 和 `description` frontmatter。

3. 查看后续部分，了解其中列出的内容：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见问题和错误列表。

## 创建并分配议题

Mission control 是管理环境中所有代理的中心位置。可以将任务分配给 Copilot 云智能体、监控任务并重定向或提供其他指导。接下来将任务分配给 Copilot，让它创建高对比度模式。

1. 打开存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新议题对话框。
4. 选择 **Blank issue**，创建新议题。
5. 将 **Title** 设置为 `Add high contrast mode to website`。
6. 将 **Description** 设置为：

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。
9. 从自定义代理列表中选择 **Accessibility agent**。

    ![显示已突出显示自定义代理和 Accessibility agent 的云智能体分配屏幕截图](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 将在后台开始处理任务。

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义代理][custom-agents]，也就是针对特定任务和领域定制的专用 AI 助手。自定义代理可以将团队的专业知识和标准转化为可复用代理，引导 Copilot 更有效地完成特定类型的工作。

本课介绍了以下概念：

- 如何定义自定义代理。
- 将任务分配给自定义代理。

Copilot 正在后台实现高对比度模式。接下来前往 mission control，[监控并引导代理会话][next-lesson]。

## 资源

- [关于自定义代理][custom-agents]
- [为组织使用自定义代理做准备][org-custom-agents]
- [为企业使用自定义代理做准备][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot 云智能体][previous-lesson] | [下一课：监控和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
