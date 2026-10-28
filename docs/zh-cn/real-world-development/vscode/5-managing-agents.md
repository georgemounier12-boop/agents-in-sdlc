---
title: "练习 5 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义智能体][previous-lesson] |
|:--|

使用智能体模式时，GitHub Copilot 会自主工作，包括探索代码库、提出更改、编辑文件和运行命令。由于这些操作会在您的计算机上实时执行，VS Code 中的 Copilot Chat 会显示每项工具调用和文件编辑的进度。您可以直接在编辑器中审查每项差异、接受或拒绝单个更改，并通过后续提示引导对话以改进或扩展工作。

本练习将学习如何：

- 实时监控无障碍访问智能体的工作。
- 审查并接受建议的文件编辑。
- 在会话进行期间引导智能体，除了高对比度模式外还添加浅色模式切换开关。

## 场景

您在[练习 4][previous-lesson]中使用的无障碍访问自定义智能体正在实现高对比度模式。工作期间，设计团队提出了额外的要求：他们希望为觉得默认主题过暗的用户添加浅色模式切换开关。无需从头开始新会话，您可以引导当前会话扩展工作内容。

## 监控 Copilot 的进度

Copilot Chat 视图会显示智能体执行的每个步骤，包括它探索的文件、运行的命令和提出的编辑。每项文件编辑都会以内联方式显示差异，并附有 **Keep** / **Undo** 控件。

1. 返回练习 4 中打开的 Copilot Chat 视图。
2. 智能体工作时，滚动聊天内容，查看每次工具调用。您会看到它读取和编辑文件，以及请求运行的终端命令。
3. 智能体提出文件编辑时，聊天中会以内联方式显示差异：
    - 选择 **Keep** 接受更改。
    - 选择 **Undo** 撤销更改，让智能体重新尝试。
4. 也可以打开聊天顶部的 **Files changed** 指示器，查看智能体在本次会话中修改的全部文件。

> [!NOTE]
> 如果工具调用请求权限（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。只批准您认可的命令。

5. 等待智能体完成高对比度模式。您应能看到总结所做更改的最终消息。

## 指导智能体添加浅色模式

高对比度模式已就绪，现在可以继续在同一对话中添加浅色模式切换开关。由于仍处于同一个聊天会话中，智能体会保留先前工作的上下文，包括修改的组件、样式方法和设置的本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 智能体会从之前的工作继续，探索现有更改并提出浅色模式所需的补充内容。
3. 新编辑加入时，请像之前一样逐项审查，并使用 **Keep** 或 **Undo**。
4. 如果智能体的处理方向不符合预期，例如尝试以不喜欢的方式重构切换 UI，可以选择聊天输入框旁边的 **Stop** 按钮停止任务，然后发送说明更清晰的后续消息。

> [!TIP]
> 在工作进行期间引导会话，比从头开始更快。可以使用后续提示请求调整（“简化 CSS，改用 CSS 变量，避免重复选择器”）、纠正方向（“不要添加第三个切换开关，请将浅色模式集成到现有设置面板”）或扩展范围（“为两个切换开关添加键盘快捷键”）。

## 审查工作文件

提交之前，请快速检查智能体修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 查看已更改文件的完整列表。您应能看到 Astro 组件、样式和相关测试的更新。
3. 打开几个已更改文件并查看差异。确认其中体现了自定义智能体建议的无障碍模式，包括 ARIA 属性、键盘导航、语义 HTML 和通过本地存储保存设置。
4. 确认满意后，在 **Source Control** 面板中暂存并提交更改。下一课会发布这些更改。

## 总结和下一步

您使用 Copilot Chat 智能体模式监控并引导了正在进行的会话，在每项文件编辑加入时进行审查、接受或拒绝更改，并发送后续提示来扩展工作，同时保留现有上下文。

本课探索了以下内容：

- 实时监控智能体的工作。
- 接受或拒绝单项文件编辑。
- 在会话进行期间通过后续提示指导智能体。

在[下一课][next-lesson]中，您将把无障碍功能作为拉取请求发布，并审查本实验中本地自定义智能体生成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用智能体模式][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
