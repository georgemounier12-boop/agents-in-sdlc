---
title: "课程 5 - 自定义并使用 quality-checks 技能"
description: "浏览现有 quality-checks 技能、自定义其报告格式，并用它验证筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

编写代码不只是编写代码。我们已经手动验证代码有效，并使用指令文件确保代码符合标准。那么测试、lint 和持续集成 (CI) 的其他部分呢？

对于这类任务，**智能体技能**最为合适！技能可以帮助 Copilot 了解如何正确执行这些操作。

本课将学习如何：

- 浏览现有的 `quality-checks` 技能；
- 自定义技能结果的格式；
- 重新加载并运行技能。

## 场景

Tailspin Toys 使用 `quality-checks` 技能运行单元测试、lint 和类型检查。团队希望改进报告，使结果更易阅读。

## 指令、脚本和资源

智能体技能将可重复使用的任务指令、可执行脚本和支持资源打包起来，供智能体按需加载。技能的核心是一个以技能名称命名的文件夹，其中包含 Markdown 文件 `SKILL.md`。该 Markdown 文件包含定义技能名称和说明的 frontmatter、技能概述，以及调用条件的指导。文件夹中还可以包含供技能调用的脚本和其他资源。

> [!NOTE]
> 技能不一定需要额外文件夹或文件。Tailspin Toys 的 `quality-checks` 技能仅包含 `SKILL.md`，因为它使用项目现有命令。

技能可以放在项目的 `.github/skills` 文件夹中，作为团队共享和重复使用的存储库资源，也可以放在用户的技能文件夹 `~/.copilot/skills` 中。

## 浏览技能

我们来查看 Tailspin Toys 团队创建的 `quality-checks` 技能，该技能用于运行单元测试、lint 和类型检查。

1. 返回 Codespace。在 Codespaces 编辑器中打开 `.github/skills/quality-checks/SKILL.md`。
2. 阅读顶部的 `name` 和 `description`。说明有助于 Copilot 了解何时调用技能。
3. 阅读指令，留意它们如何引导 Copilot 执行测试和 lint。
4. 留意技能中目前还没有 **Results output formatting** 部分。

## 更改前先运行技能

技能可以通过 Copilot CLI 直接调用，也可以使用自然语言调用。现在让 Copilot 运行这三项检查。

1. 返回处于 Interactive 模式的筛选功能对话。
2. 使用以下提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 留意最后生成的报告。

## 自定义报告

我们希望报告能清晰说明运行了什么、运行是否成功，以及工具实际报告的结果。现在更新技能，让它生成这样的报告！

1. 返回 `.github/skills/quality-checks/SKILL.md`。
2. 在文件末尾添加以下部分：

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. 文件会自动保存。

## 运行更新后的技能

更改已完成，现在查看效果！Copilot CLI 可以在不重启对话的情况下重新加载已编辑的技能。

1. 输入：

   ```plaintext
   /skills reload
   ```

2. 使用与之前完全相同的提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 留意最后的报告，并与第一次的报告比较。

## 总结和下一步

您已自定义并使用现有智能体技能。本课中，您：

- 浏览了用于运行单元测试、lint 和类型检查的 `quality-checks` 技能；
- 自定义了技能结果的格式；
- 重新加载并运行了技能。

这项更改会随筛选功能一同包含在功能 PR 中。接下来，让 Copilot 通过 [Playwright MCP 服务器][next-lesson]直接与网站交互。

## 更多技能示例

以下社区示例仅供参考，并非额外任务：

- [Agent Skills 规范][skill-spec]
- [贡献工作流：`make-repo-contribution`][contribution-example]
- [需求文档：`prd`][prd-example]
- [图表和捆绑的导出脚本：`drawio`][drawio-example]
- [浏览器测试：`webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
