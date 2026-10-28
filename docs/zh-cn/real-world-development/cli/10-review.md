---
title: "课程 10 - 总结和后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

您使用 GitHub Copilot CLI 完成了 Tailspin Toys 的连续工作流。您：

- 准备 Codespace、安装 Copilot CLI、探索项目并找到已创建的筛选议题；
- 添加星级评分，在转发的浏览器中审查结果，并手动合并第一个拉取请求 (PR)；
- 从筛选议题开始，在 Plan 模式下确定方法，在 Autopilot 模式下实现，并在 Interactive 模式下审查；
- 使用自定义指令引导智能体，随后自定义现有 `quality-checks` 技能，并使用该技能运行单元测试、lint 和类型检查；
- 添加 Playwright 模型上下文协议 (MCP) 服务器，并在真实浏览器中探索筛选功能；
- 创建并选择质量保证 (QA) 自定义智能体，以评估需求、覆盖率、技能结果和浏览器证据；
- 审查完整的筛选更改，并授权 Agent Merge 合并筛选 PR；
- 探索用于上下文、模型、共享和可选云委派的斜杠命令。

## 您交付的内容

本研讨会包含两个 PR 里程碑，每个里程碑都从更新后的 `main` 分支创建独立分支：

1. **星级评分：**在游戏卡片上显示现有的 `starRating` 和明确的未评分状态。
2. **筛选和质量工作流：**实现筛选、更新指令并将其应用于功能、自定义 `quality-checks` 报告、创建 QA 配置，并加入相关测试。

从规划筛选功能到创建 PR，您一直使用相同的对话和分支。为简化研讨会，这些工作都合并到了一个 PR 中。

## 不同类型的验证

您通过多种方式检查代码：自动化测试、亲自在浏览器中检查，以及通过 MCP 让 Copilot 探索浏览器。`quality-checks` 技能运行单元测试、lint 和类型检查，并按新格式报告结果。QA 智能体在 PR 创建前将这些结果与需求和测试覆盖率审查结合起来。

新增测试应解决真实的覆盖缺口；如果 QA 检查后不需要新测试，也可能是正确结果。授权合并前，请审查代码和证据；更改后请重新检查受影响的证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本研讨会中，您更新了指令、自定义技能、创建 QA 配置并设置 MCP 服务器。可在不同对话中重复使用这些自定义项，并根据团队需求调整。指令设置标准，技能描述可重复执行的任务，自定义智能体定义专用角色，MCP 服务器连接外部工具。请检查实际更改和工具结果，而不只是阅读智能体总结。

根据任务选择合适的**模式和模型**。使用 **Plan** 在实现前考虑方法，使用 **Interactive** 参与专注的更改，使用 **Autopilot** 处理范围明确的任务。常规编辑可选择速度较快的模型，复杂任务则可选择能力更强的模型。

上下文与基础设施同样重要。清晰说明要构建*什么*、*为什么*构建以及*如何*构建，能够显著影响输出。

## 更多可探索内容

您已完成核心工作流。还可以了解一些 CLI 功能：

- 使用 `/review` 让代码审查智能体分析更改；
- 使用 `/rubber-duck` 讨论问题并获得另一种观点；
- 使用 `/fleet` 并行协调独立子任务；
- 使用 `/worktree` 隔离单独任务；
- 使用 `/delegate` 将任务发送给 Copilot 云智能体。

## 后续步骤

提升任何工具使用能力的最佳方法就是继续使用它！可以用它编写生产代码、兴趣项目，或多年来一直想做但尚未开始的小应用。与团队分享经验，也向团队成员学习。最后，别忘了探索文档。

如果想继续从终端扩展 Tailspin Toys，可以选择[Foundry Backer Concierge 系列][foundry]。若想比较不同环境，可以探索 [VS Code 研讨会][vscode]、[GitHub Copilot app 研讨会][app]或 [Copilot 云智能体研讨会][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
