---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几项练习中，您让 Copilot 云智能体处理了三个独立任务，旨在改进用户体验和添加功能。云智能体虽然能够异步自主工作，但监控这些任务仍然很重要。

您可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心，您可以查看所有具有未关闭拉取请求（PR）的智能体任务，了解已执行的操作，甚至可以指导正在进行的会话。

本课将学习如何：

- 浏览智能体页面，监控云智能体任务。
- 指导正在进行的会话，请求添加其他功能。

## 场景

将任务分配给智能体以创建高对比度模式后，团队意识到也可以趁此机会添加浅色模式。既然正在更新网站样式并添加切换功能，将此功能一并纳入也合情合理。您希望指导智能体，同时添加浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的所有任务的状态。

1. 转到 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 留意主窗格和左侧窗格中的任务列表。您应能看到分配给 Copilot 的任务，包括：
    - 更新代码文档；
    - 生成用于修改游戏的 API；
    - 为网站添加高对比度模式。
3. 选择一项正在运行的任务，查看 Copilot 已执行的工作。可能包括：
    - 从存储库检出代码；
    - 为 Copilot 创建环境；
    - 设置 MCP 服务器；
    - 执行完成所分配任务所需的各种步骤。

> [!NOTE]
> 具体显示的步骤会因 Copilot 的工作状态和执行方式而异。

4. 同时留意右侧显示的拉取请求（PR）窗格，以便查看 PR 和更改的文件，进一步监控工作。

## 指导云智能体

现在您已了解活动任务，请在 Copilot 处理高对比度模式时，要求它同时添加浅色模式切换开关。

1. 选择与添加高对比度模式相关的会话。确切的标题会因 Copilot 使用的名称和当前工作状态而异。

    ![任务控制中心中的无障碍访问会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到会话显示设置已完成并开始工作。此时，您会看到类似以下内容的消息。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中的云智能体任务截图，其中高亮显示 Copilot 工作期间指导活动会话的对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认收到提示并将其纳入工作流程。

## 让 Copilot 完成工作

与之前一样，Copilot 会开始处理更新后的任务！它会在完成您发送消息时正在执行的具体步骤后，将新请求纳入工作流程。

这也需要几分钟，您可以趁此机会暂停一下，回顾目前学到和探索的内容。

## 总结和下一步

本课介绍了 Copilot 智能体页面，这是监控和引导 GitHub Copilot 云智能体任务的中心位置。您可以在此跟踪所有活动和已完成的任务、审查正在进行的工作，甚至重定向正在运行的任务以调整范围或提供额外指导。

您了解了以下概念：

- 使用任务控制中心和智能体页面监控云智能体任务。
- 重定向正在进行的会话，请求添加其他功能。

Copilot 正在完成无障碍功能，接下来可以[迭代改进它创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
