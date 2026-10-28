---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在整个实验中，您与 GitHub Copilot 合作完成了多项任务，以改进用户体验并添加功能。您要求 Copilot 为代码添加文档、构建供设计团队迭代的相关游戏功能，以及实现包含高对比度和浅色模式切换开关的无障碍功能。现在，请检查代码更改；如有需要，可向 Copilot 提供反馈，进一步改进其工作。

### 场景

如前所述，加入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循常规 DevOps 流程。基于这一点，在为团队其他成员启用审查之前，先审查 GitHub Copilot 为代码添加文档、实现相关游戏功能和无障碍功能所提出的更改。

## 安全性和 GitHub Copilot 云智能体

Copilot 云智能体会异步且无人监督地执行任务，因此设置了一些安全限制，以确保操作安全。这些限制包括：

- Copilot 只有存储库的读取权限，并且仅对其用于编写代码的分支具有写入权限。
- 云智能体在 GitHub Actions 中运行，并在独立的临时环境中工作。
- 运行任何 GitHub Actions 工作流前，都需要人工批准。
- [默认情况下会限制对外部资源的访问](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括 MCP 服务器。

## 审查生成的文档

先通过 GitHub.com 上的标准 PR 界面，查看 GitHub Copilot 云智能体创建的第一个拉取请求（PR），也就是为代码添加文档的 PR。

> [!NOTE]
> 查看 PR 时，您可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限地访问外部资源，包括外部 MCP 服务器。如果需要，可以[自定义或禁用 Copilot 云智能体防火墙][agent-firewall]。

1. 在新的浏览器选项卡中返回您的 github.com 存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似 **Add missing documentation** 或内容更详细的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求中会显示 **[WIP]** 标记。请等待 Copilot 完成工作。这可能需要几分钟，您可以稍作休息，或回顾目前学到的内容。

4. PR 准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 浏览刚更新的代码，其中包含新创建的 TSDoc 文档注释和其他文档。具体更改可能有所不同。

    浏览更改时，请留意 TSDoc 文档注释、TypeScript 编码规范和注释标头。这些内容来自您在练习 2 开始时查看的自定义指令文件。

6. 审查更新并确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
7. 您应能看到有些工作流正在等待批准。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows 按钮](../../../_images/shared-approve-workflows.png)
9. 您应能在拉取请求的检查部分看到工作流已排队。如果一切正常，单个 Astro 应用的项目检查就会通过。这可能需要几分钟。

## 向 GitHub Copilot 请求更改

与 Copilot 合作并非单向过程。您可以像在拉取请求或代码内评论中提及团队其他成员一样提及 Copilot。Copilot 会读取这些评论，并启动新的会话来处理请求。由于结果具有非确定性，我们无法提供确切的请求措辞。可以要求 Copilot 做出以下更改：

- 在每个代码文件开头添加简短注释，说明文件的用途。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用结构。

1. 通过评论请求更改生成的文档，并像提及用户一样提及 **@copilot**。可以使用上面的任一建议，也可以提出其他希望在代码库中看到的文档改进。
2. 选择 **View Session**，观察 Copilot 如何执行工作。请留意 Copilot 会启动新会话来完成更新。
3. 选择 **Back to pull request** 返回拉取请求。

    ![Back to pull request 按钮](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中应会出现新的提交。
5. 选择 **Files changed** 选项卡，审查更改。

可以继续迭代，直到满意为止。完成后，可以将 PR 从草稿状态改为就绪，然后合并到 main 分支。

![将 PR 转为可审查状态的操作](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在，返回 Copilot 为解决“在游戏详细信息页面显示相关游戏”议题而创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 或内容更详细的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查更新并确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
6. 您应能看到有些工作流正在等待批准。
7. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows 按钮](../../../_images/shared-approve-workflows.png)
8. 您应能在拉取请求的检查部分看到工作流已排队。如果一切正常，单个 Astro 应用的项目检查就会通过。这可能需要几分钟。
9. **可选：**也可以在 Codespace 中切换到此分支，手动测试相关游戏功能。转到 Codespace，打开终端，然后运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已构建相关游戏功能！与之前一样，可以通过迭代改进 Copilot 云智能体的工作。例如，可以要求 Copilot 调整显示的相关游戏数量，或添加注释标头和 TSDoc 文档注释（请记住，分配此任务时，您还没有更新自定义指令！）。与之前一样，可以在 **Conversation** 选项卡中发表评论来提出请求，Copilot 会读取评论并启动新会话。

## 审查无障碍功能

最后，审查使用无障碍访问自定义智能体实现的功能。此 PR 应同时包含您在练习 3 中分配的高对比度模式，以及您在练习 4 的任务控制中心中提出的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 或内容更详细的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求中会显示 **[WIP]** 标记。请等待 Copilot 完成工作，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查实现，特别留意：
   - 用于切换模式的 UI 组件；
   - 用于保存用户偏好的本地存储；
   - 高对比度和浅色模式的 CSS 或样式更改；
   - 无障碍访问属性（ARIA 标签、键盘导航等）；
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审查更新并确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
7. 您应能看到有些工作流正在等待批准。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows 按钮](../../../_images/shared-approve-workflows.png)
9. 您应能在拉取请求的检查部分看到工作流已排队。如果一切正常，单个 Astro 应用的项目检查就会通过。这可能需要几分钟。
10. **可选：**可以在 Codespace 中切换到此分支，手动测试无障碍功能。转到 Codespace，打开终端，然后运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，并在浏览器中测试高对比度和浅色模式切换开关，确认功能正常，且在重新加载页面后仍会保留设置。

请留意无障碍访问自定义智能体如何引导 Copilot 按照无障碍最佳实践实现这些功能。如果发现任何无障碍问题或改进空间，可以像处理前面的 PR 一样，在评论中提及 **@copilot** 并请求更新。

## 可选练习 — 继续委派工作

云智能体最适合处理实际待办事项，让您可以同时开展其他工作。若要培养这种工作习惯，可以在存储库中创建更多议题并分配给 Copilot。可以尝试以下内容：

- 在游戏详细信息页面创建支持者兴趣表单。
- 为游戏列表页面实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

您已完成云智能体环境中的全部练习。在这些课程中，您：

- **检查了存储库附带的自定义指令文件**，以便之后观察它们对云智能体输出的影响。
- **将议题分配给 Copilot 云智能体**，并观察它设置环境、制定计划和异步执行任务。
- **创建并使用自定义智能体**，实现高对比度和浅色模式切换开关，提升无障碍访问体验。
- **使用 Copilot 智能体页面作为任务控制中心**，监控并指导正在执行无障碍任务的会话。
- **审查并迭代改进云智能体创建的拉取请求**，提及 `@copilot` 请求更改并批准工作流。

## 审查和后续步骤

您已完成云智能体环境中的练习。其他环境中的练习可以补充您在此处学到的内容：

- 🖥️ **[VS Code 环境](../../vscode/)** — 在 IDE 中直接探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 环境](../../cli/)** — 在终端中使用 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 桥接功能，完成两项经过审查的更改。

在自己的存储库中，还可以尝试以下后续任务：

- 将重构或测试覆盖率议题分配给云智能体。
- 为其他领域创建自定义智能体。
- 为不同的技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
