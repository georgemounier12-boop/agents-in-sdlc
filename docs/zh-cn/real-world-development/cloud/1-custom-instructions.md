---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解一些背景信息，就应确保它能访问这些上下文。[指令文件][instruction-files]可提供此类指导，让 Copilot 不仅了解您希望完成什么，还了解应该如何完成。

本练习将学习如何：

- 通过存储库自定义指令和按路径限定范围的指令文件，了解如何向 Copilot 提供项目上下文、编码指南和文档标准；
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 环境不同，本练习不会比较提示的前后效果。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时并排展示影响。稍后审查云智能体创建的拉取请求时，您会看到指令文件的作用。

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

指令文件没有唯一的编写方式，正如 AI 也没有唯一的使用方式。通过尝试，您会找到适合项目、环境和场景的方法。

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

## 添加新的存储库标准

接下来要做的唯一编辑，是添加一条项目级规则，要求在代码中使用 TSDoc 文档注释和文件级注释标头。之后本环境中分配给云智能体的议题都会采用这条规则。

编辑前，请先创建一个分支（云智能体会读取议题目标分支中的指令，但仍建议将更改整洁地提交）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

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

## 提交、推送并合并指令更新

云智能体会读取议题目标分支中的指令。下一练习中，将议题分配给 Copilot 时，它会从 `main` 分支创建新分支。因此，必须先将指令更改合并到 `main`，云智能体才能使用这些指令。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求，然后合并该请求。这样，云智能体在下一练习中处理分配给它的议题时，就能读取这些指令。

> [!TIP]
> 如果希望在本研讨会中直接使用 `main` 分支，可以跳过创建分支的步骤，并直接提交到 `main`。此处采用分支，是为了模拟真实项目中的工作方式。

## 总结和下一步

您已了解 Copilot 如何使用本项目的指令文件获取上下文，并在 `.github/copilot-instructions.md` 中添加了新的存储库级标准。接下来的练习中，您会看到云智能体生成的拉取请求如何遵循这项标准。

接下来，[将第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
