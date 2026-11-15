---
title: "练习 5 - 监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义代理][previous-lesson] |
|:--|

使用 GitHub Copilot 代理模式时，它会自主探索代码库、提出更改建议、编辑文件并运行命令。由于这些工作实时发生在本地计算机上，VS Code 中的 Copilot Chat 会持续显示每次工具调用和文件编辑。开发者可以在聊天中查看每个差异、接受或拒绝单项更改，并通过后续提示引导对话，在不离开编辑器的情况下完善或扩展工作。

在本练习中，将：

- 实时监控无障碍代理的工作。
- 审查并接受建议的文件编辑。
- 在会话进行期间引导代理添加浅色模式切换开关，与高对比度模式并存。

## 场景

[练习 4][previous-lesson]中使用的无障碍自定义代理正在实现高对比度模式。工作进行期间，设计团队又提出了一项需求：还需要提供浅色模式切换开关，方便觉得默认主题太暗的用户。与其从头开始新对话，不如引导现有会话扩展工作。

## 监控 Copilot 的进度

Copilot Chat 视图会显示代理执行的每个步骤，包括探索的文件、运行的命令和提出的编辑。每项文件编辑都会以内联方式显示差异，并提供 **Keep** 和 **Undo** 控件。

1. 返回练习 4 中使用的 Copilot Chat 视图。
2. 代理工作期间，滚动聊天内容并查看每次工具调用。会看到文件读取、编辑，以及代理请求运行的终端命令。
3. 代理提出文件编辑时，聊天中会显示差异：
    - 选择 **Keep**，接受更改。
    - 选择 **Undo**，撤销更改并让代理再次尝试。
4. 也可以打开聊天顶部的 **Files changed** 指示器，查看代理在此会话中修改的完整文件列表。

> [!NOTE]
> 如果工具调用请求权限（例如运行终端命令），代理会暂停并显示 **Allow** 提示。只批准自己愿意运行的命令。

5. 等待代理完成高对比度模式的实现。完成后应会看到总结所作更改的最终消息。

## 引导代理添加浅色模式

高对比度模式实现后，可以在同一对话中继续添加浅色模式切换开关。由于继续使用同一聊天会话，代理会保留先前工作的上下文，包括修改的组件、采用的样式方案和设置的本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 代理会接着此前的工作，探索已有更改并提出浅色模式的实现。
3. 与高对比度功能一样，逐项审查新更改。对每项差异使用 **Keep** 或 **Undo**。
4. 如果代理采取了不希望的方向（例如以不喜欢的方式重构切换用户界面（UI）），可以选择聊天输入框旁的 **Stop** 按钮停止它，然后发送澄清性的后续消息。

> [!TIP]
> 引导正在进行的会话比从头开始更快。可以使用后续提示提出调整（“简化层叠样式表（CSS），使用 CSS 变量而不是重复选择器”）、纠正方向（“不要添加第三个切换开关，而是把浅色模式集成到现有设置面板中”），或扩展范围（“还要为两个切换开关添加键盘快捷方式”）。

## 审查工作集

提交工作前，快速检查代理修改的全部内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 审查所有已更改文件。应该会看到 Astro 组件、样式和相关测试的更新。
3. 打开几个已更改的文件并查看差异。确认其中体现了自定义代理要求的无障碍模式，包括无障碍富互联网应用（ARIA）属性、键盘导航、语义化超文本标记语言（HTML）和本地存储持久化。
4. 确认满意后，在 Source Control 面板中暂存并提交更改。下一课将发布这些更改。

## 总结和后续步骤

开发者使用 Copilot Chat 代理模式监控并引导了正在进行的会话：审查每项文件编辑、接受或拒绝更改，并通过后续提示扩展工作，同时保留上下文。

在本课中，开发者：

- 实时监控代理的工作。
- 接受并拒绝了单项文件编辑。
- 使用后续提示在会话进行期间引导代理。

[下一课][next-lesson]将把无障碍功能作为拉取请求发布，并审查本实验中本地自定义代理生成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用代理模式][agent-mode]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
