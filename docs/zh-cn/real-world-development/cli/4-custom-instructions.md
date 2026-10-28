---
title: "课程 4 - 使用自定义指令引导 Copilot"
description: "浏览存储库指令，添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，就应确保 Copilot 能获得相关指导。[指令文件][instruction-files]不仅说明您希望生成*什么*代码，还说明代码应采用*什么*结构。筛选功能已经完成，现在浏览 Copilot 使用的指令、添加文档标准，并将其应用于代码。

本课将学习如何：

- 了解存储库指令和按路径限定范围的指令文件如何传递给智能体；
- 更新指令文件，确保遵循编码标准；
- 了解指令文件对代码的影响。

## 场景

与其他优秀的开发团队一样，Tailspin Toys 也有一套开发实践指南和要求，包括：

- 注释应解释意图和不明显的决策，而不是重复代码内容。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明用途、参数和返回值；如果有可注入的 `db` 参数，也应予以说明。
- 可复用的 Astro 组件应说明其 `Props` 接口契约，并在相关代码更改时同步更新注释。
- 保持现有格式和 lint 指南不变。

借助指令文件，可以确保 Copilot 获得正确的信息，并按照上述实践完成任务。

## 指令文件

自定义指令可向 Copilot 提供上下文和偏好设置，帮助它理解您的编码风格和要求。借助此功能，您可以引导 Copilot 提供更符合需求的建议和代码片段。您可以指定偏好的编码规范、库，甚至希望添加的注释类型。您可以为整个存储库创建指令，也可以为特定文件类型创建指令。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库提出请求时，都会将这个唯一的指令文件发送给 Copilot。文件应包含适用于大多数请求的项目级指导。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指导。

> [!NOTE]
> 不同环境对其他指令格式的支持情况可能不同。依赖某种格式前，请查看[自定义指令支持参考][custom-instructions-support]。

## 浏览本项目中的自定义指令文件

初始项目已经附带一组指令文件。更改之前，先浏览现有内容，以便了解更改带来的影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而非终端）中打开 `.github/copilot-instructions.md`。
3. 浏览文件，留意项目简介和编码指导。此文件中的指令适用于 Copilot 在此存储库中的每次交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。请留意针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它设置了用于确定指令适用文件的 glob。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它指向其他指令文件和项目现有文件的链接。这样可以将较大的指令集拆分成更小、可重复使用的文件，并为 Copilot 提供示例。

## 更新指令文件以符合团队标准

现有文件是不错的起点，但仍缺少一项标准。修改核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，该部分应位于第 35 行附近。
2. 将以下内容添加为该部分的最后一项：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存！

## 应用更新后的指导

Copilot CLI 会在每次对话开始时加载存储库指令。保存更新后的指令后，退出并恢复筛选功能对话，以便 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选功能对话仍处于打开状态，请输入 `/exit` 关闭。Copilot CLI 会自动保存对话。
2. 在 `game-filters-cli` 分支的存储库根目录中，恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选功能对话，并确认检出的是 `game-filters-cli` 分支。
3. 让 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff`，检查已更改的 TypeScript 文件。留意新增的 TSDoc 注释，并确认它们准确说明了代码。

## 总结和下一步

您了解了 Copilot CLI 如何从指令文件获取上下文，并将新的标准应用于功能。具体来说，您：

- 了解了存储库指令和按路径限定范围的指令文件如何传递给智能体；
- 更新指令文件，确保遵循编码标准；
- 观察指令文件对代码的影响。

接下来，[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保以一致的方式运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot — 指令文件及其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
