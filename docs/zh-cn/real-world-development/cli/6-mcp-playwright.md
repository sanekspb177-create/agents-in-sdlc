---
title: "课程 6 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中检查或配置 Playwright MCP，并在浏览器中探索筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

正如前面提到的，编写代码不只是写代码。我们还需要处理数据、外部服务，并允许 Copilot 使用更多自动化工具。MCP 服务器可以帮助实现这些目标，让 Copilot CLI 使用内置功能之外的工具和服务。

在本课程中，你将：

- 了解模型上下文协议 (MCP) 以及 Copilot CLI 如何使用它；
- 如果尚未配置，则添加 Playwright MCP 服务器；
- 让智能体驱动浏览器并探索筛选功能。

## 场景

单元测试和端到端测试固然重要，但要验证 UI 更新，就需要实际与 UI 交互。你希望 Copilot 能像用户一样使用正在开发的网站，进一步自动验证更改是否符合预期，从而提升对更新的信心。

## 什么是模型上下文协议 (MCP)？

[模型上下文协议 (MCP)][mcp-blog-post]为 AI 智能体提供与外部工具和服务通信的方式。通过 MCP，AI 智能体可以与外部工具和服务实时通信，获取最新信息并代表你执行操作。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 智能体与外部工具及服务之间的桥梁。每个 MCP 服务器都会提供一组可供 AI 智能体访问的工具和资源。

以下是几个常见的 MCP 服务器：

- **[GitHub MCP 服务器][github-mcp]**：提供用于管理 GitHub 存储库、issue 和拉取请求的 API。
- **[Playwright MCP 服务器][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化功能。

还有许多其他 MCP 服务器。GitHub 托管了 [MCP 注册表][mcp-registry]，方便大家查找和参与生态系统建设。

> [!CAUTION]
> 请像对待项目中的其他依赖项一样对待 MCP 服务器。使用前请审查其源代码、验证发布者，并考虑安全影响。

## 添加 Playwright MCP 服务器

如果尚未配置 Playwright MCP 服务器，请添加它，以便 Copilot 像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 中已配置 Playwright MCP，请跳过这些设置步骤并继续下一节。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器的对话框：

   ```plaintext
   /mcp add
   ```

3. 输入名称 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴以下命令：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 让 Copilot 使用 Playwright 探索功能

之前你已经手动确认功能符合预期。现在使用 Copilot 中的 Playwright MCP 服务器完成同样的操作！

1. 使用以下提示，要求 Copilot 使用 Playwright MCP 服务器验证功能：

   ```plaintext
   启动应用，并使用 Playwright MCP 对照 issue 和我们的计划检查筛选功能。告诉我哪些功能正常、哪些不正常，不要进行任何更改。完成后停止你启动的服务器。
   ```

> [!NOTE]
> 通常无需明确要求 Copilot 使用 MCP 服务器，它通常会自行选择合适的工具。但既然你知道要用什么，明确指出工具也无妨。这有助于获得更一致的结果，也能节省一些令牌。

2. 观察 Copilot 如何列出它在浏览器中执行的各个步骤，以确认功能是否正常。
3. 阅读报告并确认所有行为都符合预期。

Copilot 会启动服务器、使用 Playwright 与网站交互、停止服务器并向你提交报告。

## 总结和下一步

恭喜，你已通过 Copilot CLI 中的 Playwright MCP 服务器在真实浏览器中探索功能！回顾一下，你：

- 了解了模型上下文协议 (MCP) 以及 Copilot CLI 如何使用它；
- 如果尚未配置，则添加了 Playwright MCP 服务器；
- 让智能体驱动浏览器并探索筛选功能。

接下来，创建一个[质量保证 (QA) 自定义智能体][next-lesson]，将技能和浏览器工具整合到专业角色中。

## 资源

- [MCP 到底是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP 服务器][playwright-mcp-server]
- [将 MCP 服务器添加到 Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
