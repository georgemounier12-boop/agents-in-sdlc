---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解一些背景信息，就应确保它能访问这些上下文。您可以通过多种方式向 Copilot 提供特定上下文，其中一种主要方式是使用[指令文件][instruction-files]来指导代码生成。

本练习将学习如何：

- 通过存储库自定义指令和按路径限定范围的指令文件，了解如何向 Copilot 提供项目上下文、编码指南和文档标准；
- 使用当前指令生成第一部分筛选数据代码（发行商辅助函数）；
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准；
- 再次运行相同提示，观察生成的代码如何采用新标准；
- 将指令更新和筛选代码提交到 `main`，让 Copilot 在下一练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不符合您设置的所有标准。Copilot 的结果具有非确定性。本练习旨在让您观察更新指令前后 Copilot 行为的*整体变化趋势*，而不是逐字符匹配代码输出。

## 指令文件

### 场景

与其他优秀的开发团队一样，Tailspin Toys 也有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用暗色模式，并具有现代感。
- 应通过 TSDoc 文档注释为代码添加说明。
- 每个文件开头都应添加一段注释，说明该文件的用途。

借助指令文件，可以确保 Copilot 获得正确的信息，并按照上述实践完成任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好设置，帮助它理解您的编码风格和要求。借助此功能，您可以引导 Copilot 提供更符合需求的建议和代码片段。您可以指定偏好的编码规范、库，甚至希望添加的注释类型。您可以为整个存储库创建指令，也可以为特定文件类型创建指令，提供任务级上下文。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库提出请求时，都会将这个唯一的指令文件发送给 Copilot。文件应包含项目级指导，即大多数面向 Copilot Chat 或 CLI 的请求都会用到的上下文，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对具体任务或文件类型创建这些文件。例如，为 TypeScript 或 Astro 等语言，或者创建 UI 组件或一组新单元测试等任务提供指导。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，都会使用存储库级指令文件以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 最后，Copilot 还[支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md 文件。

### 管理指令文件的最佳实践

完整介绍如何创建指令文件超出了本研讨会的范围。不过，示例项目展示了一种有代表性的方法。总体而言：

- 将 `copilot-instructions.md` 中的指导限定为项目级内容，例如项目说明、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用自然语言，确保指导清晰，并提供代码示例说明应如何以及不应如何编写。

指令文件没有唯一的编写方式，正如 AI 也没有唯一的使用方式。通过尝试，您会找到适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应具备完善的指令文件。浏览本项目中的文件时，您会看到其中包含多种任务的指导，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助您生成指令文件。不同产品的操作方式有所不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。对应产品的课程会在相关内容中介绍。
>
> 需要模板或起点？可以查看 [awesome-copilot][awesome-copilot]，其中包含大量指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览本项目中的自定义指令文件

花点时间阅读此存储库附带的指令文件，其中包含一个核心 `copilot-instructions.md` 文件和一组用于不同任务的 `*.instructions.md` 文件。请在编辑器或 GitHub Web 界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览该文件，留意项目简要说明以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。在 **Code standards** 下，留意嵌套的 **GitHub Actions Workflows** 指导。这些内容适用于您与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的内容。请留意针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。请留意顶部的 `applyTo` 字段，它设置了一个相对于存储库根目录的 glob，用于确定指令适用的文件。这里，任何匹配 `**/*.test.ts` 的 TypeScript 测试文件都会符合条件。
5. 留意专门用于在本项目中创建单元测试的指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。请留意其中指向其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成更小、可重复使用的文件，并为 Copilot 提供生成代码时可参照的示例。（其中的路径相对于指令文件，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目的编码标准，但目前尚未要求在代码中编写说明。接下来的步骤会添加 TSDoc 文档注释和文件注释标头的规则。

## 为更改创建分支

接下来按照最佳实践，为更改创建一个分支。

1. 返回上一练习中使用的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新的终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，请返回 codespace，打开 Copilot Chat 面板，然后选择 **New Chat** 开始干净的新会话。不同练习的模式和模型选择可能有所不同，每个步骤都会说明相关设置。

要查看自定义指令的影响，先在当前指令文件的基础上发送提示。稍后更新指令后，再次发送相同提示并对比结果。

1. 关闭之前练习中打开的编辑器选项卡，确保 Copilot 只获取需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，让 Copilot 应用文件更改。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和测试。
6. 请留意，建议的辅助函数是一个带类型的函数，以 `db` 客户端作为第一个参数，并返回带类型的发行商数组。这来自 `.github/instructions/drizzle.instructions.md` 中的数据层规范（该文件适用于 `src/lib/*.ts`）。
7. 请留意，建议的代码**缺少** TSDoc 文档注释和文件级注释标头。

> [!CAUTION]
> Copilot 的结果具有概率性，因此即使没有相关指令，它也可能添加文档注释。如果发生这种情况没有关系，本练习的重点仍然是观察更新指令后的一致性改进。

## 添加新的存储库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。请添加编码标准，改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，该部分应位于第 27 行附近。请留意其中记录的项目编码标准；目前没有关于代码说明的规则，这也是生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 行，要求 Copilot 为文件添加注释标头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级别（`.github/copilot-instructions.md`）创建指令文件提供全局指导，也可以通过 `*.instructions.md` 文件为特定语言、文件类型或任务提供指导。您刚添加的注释规则属于项目级标准，因此适合放在存储库级文件中。

## 再次运行提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat** 清除聊天记录。
2. 再次选择 `src/lib/publishers.ts`，让 Copilot 专注于正确的文件。
3. 发送与之前**相同的提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 请留意，现在建议的文件开头会包含类似以下内容的注释块：

   ```typescript
   /**
    * Tailspin Toys 众筹平台的发行商数据访问辅助函数。
    * 提供从数据库检索发行商信息的函数。
    */
   ```

5. 请留意，现在建议的函数包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * 返回包含 ID 和名称的所有发行商列表。
    *
    * @param db - Drizzle 数据库客户端。
    * @returns 解析为发行商对象数组的 Promise。
    */
   ```

您刚刚引导 Copilot 遵循新的项目标准，并将其应用到真实代码中。下一练习会以这些代码为基础继续工作。

## 提交指令并推送分支

指令文件与存储库中的其他资源一样，需要通过源代码管理进行维护。现在将更改提交并推送到存储库。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新的终端窗口。
2. 在终端中运行以下命令，确认指令和辅助函数更改已存在：

   ```bash
   git status
   ```

3. 在终端中暂存并提交指令更新，以及 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示还有其他相关更新（例如测试），也请添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 在终端中将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送后，创建拉取请求，并将其关联到待办事项中的文档标准议题。虽然可以手动操作，但也可以让 Copilot 使用已连接到项目的 GitHub 工具代为完成。请提示 Copilot 查找议题、创建用于关闭该议题的 PR，然后合并 PR。

> [!NOTE]
> 这是 Copilot 首次代表您在 GitHub 上操作的练习。项目模板已经将 GitHub 工具连接到工作区，无需进行设置。Copilot 首次使用其中一个工具时，VS Code 可能会提示您登录 GitHub。请按照提示授权。后续练习会介绍这种连接所用的模型上下文协议。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认在智能体选择器中选择了 **Agent**。
3. 让 Copilot 查找与更新指令文件相关的议题，并从当前分支创建 PR，说明指令更新和新增的发行商辅助函数基础：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找议题并创建 PR。
5. 如果 Copilot 提示您 **Allow** 它代您执行 GitHub 操作，请审查命令，并酌情选择 **Allow**。
6. PR 创建后，要求 Copilot 合并 PR，并将本地分支切回 main：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 如果 Copilot 提示您 **Allow** 它代您执行 GitHub 操作和 shell 命令，请审查命令，并酌情选择 **Allow**。

现在，您已借助 GitHub Copilot 创建并合并了拉取请求！

## 总结和下一步

您了解了 Copilot 如何从项目指令文件获取上下文，并在 VS Code 中使用 Copilot Chat：

- 发送代码生成提示，观察 Copilot 如何根据*现有*指令生成代码；
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准；
- 再次运行相同的提示，观察建议的代码如何采用新标准；
- 将指令更新和辅助函数基础提交到 `main`，供下一练习使用。

接下来，在[智能体模式][next-lesson]中运用这些指令，让 Copilot 跨代码库添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot — 指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
