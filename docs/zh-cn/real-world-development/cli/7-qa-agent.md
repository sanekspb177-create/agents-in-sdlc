---
title: "课程 7 - 创建和使用 QA 智能体"
description: "创建一个 QA 自定义智能体，将 issue 需求、quality-checks 技能和 Playwright MCP 结合起来。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动化检查，并使用 Playwright MCP 在浏览器中观察筛选体验。现在将这些能力整合到一个定义清晰的 QA 流程自定义智能体中。

在本课程中，你将：

- 探索自定义智能体如何结合说明、技能和 MCP 工具；
- 创建并检查可复用的质量保证配置文件；
- 选择 QA 智能体，根据筛选 issue 审查其发现。

## 场景

Tailspin Toys 希望在创建拉取请求 (PR) 前，统一检查需求、代码质量、自动化检查、测试覆盖率和浏览器行为。自定义智能体可以协调 QA 流程并提供可复用的报告。

## 什么是自定义智能体？

自定义智能体是通过 Markdown 配置文件定义的 Copilot 专业角色。配置文件说明智能体的用途、说明和可用工具。在本工作坊中，你将在 `.github/agents/qa.agent.md` 中定义 QA 角色，然后在 Copilot CLI 中选择它。

你所使用的自定义项各有不同用途：存储库说明描述团队标准；`quality-checks` 技能封装可重复运行的检查；Playwright MCP 提供浏览器工具；QA 配置文件则告诉 Copilot 如何利用这些能力评估需求并报告结果。它们彼此互补，不需要另开会话。

## 创建 QA 配置文件

创建功能 PR 前，让 Copilot 创建一个可复用的 QA 配置文件。该配置文件既要定义 QA 执行的检查，也要规定它必须遵守的限制。

1. 返回 Codespace，并确认筛选会话处于 Interactive 模式。
2. 让默认智能体创建新的自定义智能体：

   ```plaintext
   创建一个名为 QA 的自定义智能体，文件路径为 .github/agents/qa.agent.md。它应根据 issue 和已达成共识的要求检查功能，遵循存储库说明，运行 quality-checks 技能，使用 Playwright MCP 验证行为，并在测试覆盖不足时添加测试。

   报告每项要求的状态（通过、失败或受阻），并提供支持证据。修改实现代码前必须征求我的同意，不得提交更改或创建拉取请求。

   目前只创建配置文件，供我审查。
   ```

## 检查配置文件

使用新智能体前，先审查其配置文件，确认 Copilot 捕获了预期的 QA 工作流程和边界。这样可避免配置不完整或范围过宽，导致智能体在你只想验证功能时修改实现。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 为必填项；`name` 是可选的，但填写后会显示清晰的名称。
3. 阅读配置文件说明，确认 QA 会先查看需求、遵循存储库说明、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持证据、修改实现代码前会征求同意，也不会提交更改或创建拉取请求。
5. 如果生成的配置文件遗漏了上述职责或边界，请在继续之前让默认智能体修改。

## 根据 issue 运行 QA

Copilot CLI 会在会话启动时加载项目智能体。创建配置文件后，恢复同一个筛选会话并选择 QA，以便它使用已在上下文中的 issue 和规划决策。

1. 使用以下提示启用智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 刚创建的智能体可能不会立即出现在可用列表中，但仍然可以通过上述命令激活。

2. 使用以下提示，让 QA 智能体审查功能：

   ```plaintext
   根据 issue 和计划中达成的决定审查筛选功能。现在可以创建 PR 了吗？
   ```

3. QA 智能体开始工作！
4. 完成后阅读它提供的报告。

## 总结和下一步

你已为工作流添加可复用的专业角色，并审查了其工作。在本课程中，你：

- 探索了自定义智能体如何结合说明、技能和 MCP 工具；
- 创建并检查了可复用的质量保证配置文件；
- 选择 QA 智能体，根据筛选 issue 审查其发现。

现在，实现、技能更新、QA 配置文件、测试和验证报告都已准备就绪，可以进行审查。接下来，[整合这些内容并创建功能 PR，然后使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
