---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都面临技术债务问题，例如未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云智能体旨在自主执行更新代码、添加功能等任务。智能体完成工作后，会创建草稿 PR，供开发人员审查。这样可以将繁琐任务交给智能体，加快开发流程，让开发人员专注于更重要的事项。

本练习将探索 Copilot 云智能体的以下用途：

- 自定义代码生成环境；
- 确保安全地执行操作；
- 了解明确界定议题范围的重要性；
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初受雇开发网站的承包商没有留下完善的文档，事实上，您会发现代码完全缺少文档。团队希望首先为应用中的所有导出函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现功能。他们希望在每款游戏的详细信息页面显示相关游戏（即同一类别中的其他游戏），让用户可以继续浏览。团队目前不需要完善的实现，只需要能用于 UX 验收测试的功能。这个任务目前会造成阻碍，但还有优先级更高的议题。

这些任务很容易被不断延后，因此非常适合交由 Copilot 云智能体处理。云智能体可以异步完成任务，让开发人员继续处理其他工作，之后再回来审查 Copilot 的成果。

## 了解 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发人员一样在后台执行任务。您可以通过多种方式与它合作，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求来跟踪进度、设置环境并开始处理任务。您可以在 Copilot 会话进行期间或完成后查看详情。解决方案准备好供您审查时，Copilot 会在拉取请求中提及您。

## 明确界定指令范围的重要性

GitHub Copilot 并非魔法工具，使用它并不意味着可以跳过思考。即使看似简单的操作，也可能包含复杂环节，因此[应仔细界定分配给 Copilot 云智能体的任务范围][cloud-agent-best-practices]。请将它视为 AI 结对程序员：分阶段工作、学习、尝试并不断调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义指令

前面的练习介绍了自定义指令及其指导 Copilot 的方式。如果您已经完成自定义指令练习，就会看到 `.github/copilot-instructions.md` 的作用；如果尚未完成，现在正适合快速浏览一下。将工作分配给 Copilot 云智能体前，请只读查看本存储库附带的指令文件，以便之后观察其影响。

在 GitHub Web 界面中打开以下文件；如果已启动 codespace，也可以在其中打开：

- `.github/copilot-instructions.md` — 查看 **Code standards** 部分，尤其留意 TypeScript 规范和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md` — 留意 `applyTo` frontmatter，它将指令限定为 `**/*.test.ts` 文件。

在下一部分将 `Code lacks documentation` 议题分配给云智能体时，请留意生成的拉取请求是否采用了 TSDoc 文档注释、TypeScript 编码规范和注释标头。这些要求来自上述指令文件。稍后的练习中审查 PR 时还会再次介绍。

## 为 Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，以确保环境正常。将任务分配给 Copilot 时也需要这样做，因为它会像软件工程师一样处理任务。

云智能体使用 [GitHub Actions][github-actions] 执行任务。您可以通过创建[专用设置工作流][setup-workflow]来自定义环境，该工作流配置在 `.github/workflows/copilot-setup-steps.yml` 文件中，并会在智能体开始工作前运行。这样可以确保它能够使用所需的开发工具和依赖项。为方便实验流程并提供学习机会，本实验已预先配置了该工作流。它会为单体 Astro 应用安装 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及经过迁移和填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许您在存储库的“Actions”选项卡中测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 仅设置步骤实际需要的最低权限。Copilot 会获得自己的令牌来执行操作。
    permissions:
      # 如果需要在设置步骤中克隆存储库（例如安装依赖项），则需要 `contents: read` 权限。如果未在设置步骤中克隆存储库，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现已采用 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试时有可用数据。
      - name: Set up the database
        run: npm run db:setup
```

此文件与其他 GitHub 工作流文件类似，但有几个关键点：

- 文件中只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会运行此作业。
- 请留意 `workflow_dispatch` 触发器。它允许您在存储库的 **Actions** 选项卡中手动运行工作流，这样可以在等待 Copilot 运行之前确认工作流是否成功。

## 添加文档

虽然人人都了解文档的重要性，但大多数项目的文档都已过时或完全缺失。这类技术债务常常无人处理，降低工作效率，也增加维护代码库或帮助新开发人员熟悉项目的难度。Copilot 擅长创建文档，因此非常适合将这类议题分配给 Copilot 云智能体，让它在后台生成所需文档。后续练习中，您会回来审查它的成果。

1. 在新的浏览器选项卡中转到 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新议题对话框。
4. 选择 **Blank issue**，创建新议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的拉取请求（PR），其标题类似 `[WIP]: Code lacks documentation`。如果列表中没有新 PR，请稍等片刻，然后刷新浏览器窗口。
12. 几分钟后，您应能看到 Copilot 创建的待办事项列表。

> [!NOTE]
> Copilot 的待办事项列表可能需要几分钟才会显示在 PR 中。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方法。

13. 查看列表及 Copilot 将要完成的任务。
14. 向下滚动拉取请求的时间线，应能看到 Copilot 已开始处理该议题。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 您可能需要刷新窗口，才能看到更新后的状态指示器。

16. 浏览实时会话，了解 Copilot 如何解决问题，包括探索代码并了解当前状态、暂停思考并决定合适的计划，以及编写代码。

这可能需要几分钟。Copilot 云智能体的主要目标之一是异步执行任务，让我们可以专注于其他工作。接下来，我们将利用这一点：先再分配一项任务给 Copilot 云智能体，然后编写代码，为应用添加功能。

## 在游戏详细信息页面添加相关游戏部分

如前所述，GitHub Copilot 云智能体的一大优势是可以拆分工作，让您和智能体分别处理不同的任务。为设计团队添加相关游戏部分可能不会花很长时间，但也可以将这段时间用于其他工作。现在就将此任务分配给 Copilot 云智能体！

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新议题对话框。
4. 选择 **Blank issue**，使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，您会在议题的第一条评论中看到一组 👀，表示 Copilot 已开始处理任务！

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在认真处理您的第二项请求！Copilot 云智能体的工作方式类似软件工程师，因此您不需要一直监控它，而是可以等任务完成后再审查结果。现在，让我们转而创建和使用自定义智能体。

## 总结和下一步

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。可以将议题分配给云智能体，让它异步处理任务。您可以用 Copilot 处理技术债务、创建新功能，或协助将代码从一种框架迁移到另一种框架。

本课探索了以下概念：

- 自定义代码生成环境；
- 确保安全地执行操作；
- 了解明确界定议题范围的重要性；
- 将议题分配给 Copilot。

云智能体正在后台处理任务，现在可以开始创建和使用自定义智能体。[Copilot 云智能体还可以使用 MCP 服务器][cloud-agent-mcp]，并可使用前面模块介绍的自定义指令。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
