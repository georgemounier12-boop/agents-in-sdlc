---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]可创建专门的 AI 助手，针对开发工作流中的特定任务或领域进行定制。在存储库的 `.github/agents` 文件夹中通过 Markdown 文件定义智能体，即可为 Copilot 提供专注的指令、最佳实践、编码模式和领域知识，帮助它更有效地完成特定类型的工作。团队可以将专业知识编写成可重复使用的智能体，例如执行 [WCAG][wcag] 合规检查的无障碍访问智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体由项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以在 `~/.copilot/agents` 中全局定义。每个文件都包含 YAML frontmatter，至少要有 `name` 和 `description` 字段，随后是定义智能体行为、专长和指令的 Markdown 提示。

### 自定义智能体与智能体技能的区别

自定义智能体与[智能体技能][agent-skills-concept]在概念上有些重叠。两者主要通过 Markdown 文件定义，都会告诉 AI 如何执行操作。最简单的区分方式是：**自定义智能体是执行工作的人员，技能是它使用的工具**。

自定义智能体拥有自己的上下文窗口，并通过协调技能（甚至其他智能体）来完成工作。在本实验中，无障碍访问自定义智能体会根据无障碍访问指南审查并更新网站；在此过程中，它可以调用拉取请求工作流技能或运行和管理测试的技能。

> [!NOTE]
> 编写自定义智能体没有唯一的“正确”方式。与 AI 领域的其他实践一样，请在自己的环境和场景中测试并不断改进。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，无论用户的视觉能力或偏好如何。近期的用户反馈指出，由于文本与背景颜色的对比度不足，一些用户觉得当前的暗色主题难以阅读。为解决这一无障碍访问问题，设计团队要求实现可由用户开启或关闭的高对比度模式。

由于无障碍访问至关重要，您希望尽快实现此功能，因此将使用自定义智能体生成相应功能。

本练习将学习如何：

- 审查现有的无障碍访问自定义智能体。
- 在 Copilot Chat 中使用无障碍访问智能体实现高对比度模式。

## 审查无障碍访问自定义智能体

我们已为您创建无障碍访问自定义智能体。请查看其内容，了解它将如何引导 Copilot。

返回 codespace，然后打开终端，从 `main` 分支创建新的分支，用于无障碍功能的工作（筛选功能的 PR 会保留在独立分支中）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 留意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` frontmatter。

3. 接下来，浏览其余部分，了解其中介绍的内容：
   - 为无障碍网站生成代码时的核心职责；
   - 无障碍访问最佳实践；
   - HTML、CSS 和 JavaScript 代码示例；
   - 常见陷阱和错误。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉列表中显示 `.github/agents` 中定义的所有自定义智能体。选择自定义智能体后，即可让聊天会话使用该智能体的指令和工具。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，请返回 codespace，打开 Copilot Chat 面板，然后选择 **New Chat** 开始干净的新会话。不同练习的模式和模型选择可能有所不同，每个步骤都会说明相关设置。

1. 如果智能体下拉列表中尚未选择 **Agent**，请先选择它。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

2. 选择聊天视图底部的智能体下拉列表（其中显示当前智能体，默认为 **default**）。
3. 从可用智能体列表中选择 **Accessibility agent**。
4. 向无障碍访问智能体发送以下提示：

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat 会开始处理任务，探索代码库、提出更改并修改项目文件。每项编辑都会以内联方式显示在聊天中，包含文件路径和可审查的差异。
6. 随着编辑内容加入，**Files changed** 指示器会更新，显示智能体修改过的工作文件。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 正在实际修改存储库中的文件；工作期间，它可能会编辑 Astro 组件、CSS 和相关测试等现有文件。

下一课将审查并指导这项正在进行的工作。

## 总结和下一步

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，也就是针对特定任务和领域定制的专用 AI 助手。通过自定义智能体，可以将团队的专业知识和标准编写成可重复使用的智能体，从而引导 Copilot 更有效地完成特定类型的工作。

您了解了以下概念：

- 如何定义自定义智能体。
- 如何在 Copilot Chat 智能体模式中使用自定义智能体。

接下来，[监控并指导智能体的工作][next-lesson]，在更改过程中进行审查，并在同一会话中添加浅色模式切换开关。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [awesome-copilot 中的自定义智能体][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
