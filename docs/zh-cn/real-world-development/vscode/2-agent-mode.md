---
title: "练习 2 - 使用 Copilot 智能体模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行一些操作，例如运行测试。作为开发人员，您的工作流程通常包括：找到所有相关文件、进行更改、运行测试、调试、找出遗漏的文件、再次更新……此类工作还会持续下去。

Copilot 智能体模式正是为此而设计的。

智能体模式旨在让 Copilot 在 IDE 中更自主地工作。它的工作方式类似开发人员：先探索现有项目结构，再执行必要的更新，运行测试等任务，并自动修复发现的问题。接下来，了解如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和云智能体的名称相似，但它们针对的是两种不同的体验。智能体模式在 IDE 中执行任务，便于快速反馈和交互。云智能体则像团队成员一样异步工作，通过议题和拉取请求与您协作。

本练习将学习如何：

- 使用 Copilot 智能体模式探索项目、识别相关文件并协调更改；
- 使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能；
- 在将 Copilot 智能体模式生成的更改合并到代码库之前审查更改和测试。

## 场景

游戏列表不断增加，因此您希望允许用户按类别和发行商筛选。上一练习已添加发行商辅助函数，现在将使用 Copilot 智能体模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

开始更改前，先探索 Tailspin Toys 网站，了解其现有功能。

这是一个面向开发者的桌游众筹网站，允许用户浏览游戏列表并查看详情。网站是单体 Astro 应用，会在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能通过使用标准 DOM API 的小型 Astro `<script>` 添加。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单体 Astro 开发服务器。请按照以下步骤在 GitHub Codespace 中运行：

1. 返回 codespace。上一练习结束后，您应已切回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），在 Codespace 中打开新的终端窗口。
3. 为筛选功能创建并切换到新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，您会看到类似以下内容的横幅，其中显示服务器 URL：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果出现提示您打开 `http://localhost:4321` 的浏览器窗口的对话框，请选择 **x** 将其关闭。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 codespace 中选择终端里的 localhost URL 链接时，系统会自动将您重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向 codespace 的专用隧道，您的 Web 服务器现在通过它提供服务。

### 浏览网站

网站运行后，您可以探索其功能。主要功能包括：

- **主页**：显示桌游列表，包括名称、图片和说明。
- **游戏详细信息页面**：选择游戏后，会打开包含更多游戏信息的页面，例如名称、说明、发行商和类别。

## 使用 Copilot 查看待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，请返回 codespace，打开 Copilot Chat 面板，然后选择 **New Chat** 开始干净的新会话。不同练习的模式和模型选择可能有所不同，每个步骤都会说明相关设置。

网站的初始实现已经可用，但我们希望添加新功能来改进网站。先查看待办事项。您从模板设置项目时，系统已自动创建一组议题作为待办事项；请让 GitHub Copilot 列出这些议题。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。**Agent** 智能体可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某个模型。编写研讨会时使用的是 Claude Sonnet 4.5，因此说明中包含该模型。这样做是希望收到的代码建议相对一致，从而带来良好的体验。不过，由于 LLM 的结果具有概率性，您收到的建议可能与研讨会中的描述不同。这完全正常，也是预期情况。

> [!NOTE]
> 由于 LLM 具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，询问待办议题：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 查看生成的议题列表。

请留意，Copilot 已根据它认为对用户最有用的程度为这些项目排序。

## 查看指令文件

让智能体开始生成代码之前，先查看用于向 Copilot 提供工作上下文的指令文件。我们将使用[用户界面 (UI) 指令文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，其中介绍了向网站添加功能的方法。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意其中关于如何添加功能的整体指导，包括：
   - 架构概述；
   - 组件设计、可测试性和无障碍访问原则；
   - 指向不同文件类型的具体指令文件的链接，包括：
     - Astro；
     - Tailwind CSS；
     - Drizzle。

> [!TIP]
> 指令文件可以引用其他指令文件和项目中的文件。路径相对于指令文件所在位置，因此可以重复使用指令，将复杂内容拆分为更小、更易管理的部分，并提供示例和模板。

## 实现筛选功能

完成筛选功能至少需要对应用执行三项独立更新：

- 在数据层 (`src/lib/`) 中添加或完善剩余的筛选逻辑；
- 添加或更新筛选行为的测试；
- 更新游戏列表页面，加入筛选 UI。

此外，合并更改前还需要运行测试并确保测试通过。Copilot 智能体模式可以帮您完成这些工作。现在开始添加此功能。

1. 可以继续使用当前 Copilot 会话，也可以选择 **New Chat** 开始新会话。
2. 选择 **Add Context**、**Instructions**，然后选择 `ui` 指令文件。

   ![选择 UI 指令文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉列表中仍选择了 **Agent**。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 确认仍为模型选择了 **Claude Sonnet 4.5**。
5. 使用以下提示，让 Copilot 根据待办议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目并定位相关文件。您应能看到它找到数据层辅助函数、UI 和测试，然后开始修改文件并运行测试。

   ![Copilot 探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。在此期间，代码编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会显示相应的 **Keep** 或 **Undo** 按钮；工作进行期间无需选择它们。

7. 按 Copilot 的提示选择 **Continue**，运行测试。

   ![Copilot Chat 窗格中的对话框截图，询问用户是否确认运行测试](../../../_images/ex3-agent-mode-run-tests.png)

8. 整个过程中可能会出现暂停，也可能看到一些测试失败。这没有关系！Copilot 会在代码生成和测试之间反复迭代，直到完成任务且未检测到错误。

   ![Copilot 智能体模式的完整 Chat 会话截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查看是否有潜在问题。

> [!CAUTION]
> 请记住，务必审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，探索新功能！
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和下一步

恭喜！本练习中，您了解了如何使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加新功能。您学习了如何：

- 使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能；
- 使用 Copilot 智能体模式探索项目、识别相关文件并协调更改；
- 在将 Copilot 智能体模式生成的更改合并到代码库之前审查更改和测试。

接下来，[使用 Playwright MCP 服务器测试功能][next-lesson]并为其创建拉取请求。

### 奖励探索练习 — 实现分页

游戏列表不断增加，因此需要启用分页。使用本练习中学到的技能，提示 Copilot 更新网站以实现分页。编写代码时可以考虑：

- 遵循现有最佳实践，包括使用现有指令文件；
- 考虑分页的实现方式，例如是否允许用户选择每页大小，还是使用固定大小；
- 编写提示时，为 Copilot 提供实现所需的指导；
- 可能需要与 GitHub Copilot 多次交互，要求修改并提供上下文。这是使用 Copilot 时的正常工作方式！

## 资源

- [Copilot 的 ask、edit 和 agent 模式：用途及适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
