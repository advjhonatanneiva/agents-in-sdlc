---
title: "练习 4 - 监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前两项练习中，开发者请 Copilot 云智能体处理了三项旨在改善用户体验并添加功能的独立任务。云智能体的设计目标是异步自主工作，但监控这些任务仍然很重要。

可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[代理页面][agents-page]。在这个控制中心，可以查看所有带有未关闭拉取请求（PR）的代理任务、探索执行的操作，甚至引导正在进行的会话。

在本课中，将：

- 探索代理页面，监控云智能体任务。
- 引导正在进行的会话，请求添加其他功能。

## 场景

将任务分配给代理实现高对比度模式后，团队意识到也应该同时添加浅色模式。由于已经在更新网站样式并添加切换功能，因此这是顺便实现此功能的好时机。开发者将引导代理同时添加浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

现在查看分配给 Copilot 云智能体的所有任务的状态。

1. 打开 [Copilot 代理页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。应该能看到分配给 Copilot 的任务，包括：
    - 更新代码文档。
    - 为修改游戏生成应用程序编程接口（API）。
    - 为网站添加高对比度模式。
3. 选择一项正在运行的任务，查看 Copilot 已执行的工作。这些操作可能包括：
    - 从存储库检出代码。
    - 为 Copilot 创建工作环境。
    - 设置模型上下文协议（Model Context Protocol，MCP）服务器。
    - 执行完成分配任务所需的各种操作。

> [!NOTE]
> 显示的具体步骤因 Copilot 的工作状态和采用的方案而异。

4. 同时留意右侧出现的拉取请求（PR）窗格。可以通过它查看 PR 和已更改的文件，以便进一步监控。

## 引导云智能体

查看当前活动任务后，接下来请 Copilot 在处理高对比度模式时也添加浅色模式切换开关。

1. 选择用于添加高对比度模式的会话。确切标题会因 Copilot 使用的名称和当前工作状态而异。

    ![Mission control 中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到设置完成并开始执行任务。开始看到类似以下内容的消息时，就表示已进入此阶段。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![代理页面中突出显示 Steer active session while copilot is working 对话框的屏幕截图](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 如何确认收到提示并将其加入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会继续处理更新后的任务。发送消息时，它正在执行的具体步骤完成后，就会将新请求加入工作流程。

这也需要几分钟，可以利用这段时间回顾迄今为止学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot 代理页面，它是监控和引导 GitHub Copilot 云智能体任务的中心位置。在此控制中心，可以跟踪所有活动和已完成的任务、审查正在进行的工作，甚至重定向正在处理的任务以调整范围或提供更多指导。

本课介绍了以下概念：

- 使用 mission control 和代理页面监控云智能体任务。
- 重定向正在进行的会话，请求添加其他功能。

Copilot 正在完成无障碍功能。接下来[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 代理页面][agents-page]
- [关于自定义代理][custom-agents]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
