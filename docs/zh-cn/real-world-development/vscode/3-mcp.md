---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：代理模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚刚使用代理模式构建了筛选功能。创建拉取请求之前，应先确认该功能确实能在浏览器中正常运行。无需亲自逐一操作应用，你可以连接 **Playwright MCP 服务器**，让 Copilot 操控真实浏览器为你测试功能，然后发布分支并创建 PR。

在本练习中，你将：

- 了解模型上下文协议 (MCP) 以及 MCP 服务器如何为 Copilot 扩展新工具；
- 将 Playwright MCP 服务器添加到工作区；
- 让 Copilot 使用该服务器在浏览器中手动测试筛选功能；
- 发布分支，并为筛选工作创建拉取请求。

## 什么是模型上下文协议 (MCP)？

当代理模式可以访问编辑器之外的资源时，它的功能会强大得多。Copilot 通过模型上下文协议 (MCP) 实现这一点：MCP 是一种让代理与外部工具和服务通信的标准方式。

![示意图展示代理模式的内部工作方式，以及它如何与上下文、LLM 和工具交互，其中包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[模型上下文协议 (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 为 AI 智能体提供与外部工具和服务通信的方式。通过 MCP，AI 智能体可以与外部工具和服务实时通信，使用资源获取最新信息，并使用工具代表你执行操作。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 智能体与外部工具及服务之间的桥梁，负责管理智能体与外部工具（例如现有 API 或 npm 包等本地工具）之间的通信。每个 MCP 服务器都会提供一组可供 AI 智能体访问的工具和资源。

以下是几个常见的 MCP 服务器：

- **[GitHub MCP 服务器](https://github.com/github/github-mcp-server)**：提供用于管理 GitHub 存储库、issue 和拉取请求的一组 API。
- **[Playwright MCP 服务器](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化功能，让 AI 智能体能够浏览网页、填写表单和选择按钮。

还有许多其他 MCP 服务器可提供不同的工具和资源。GitHub 托管了 [MCP 注册表](https://github.com/mcp)，方便大家查找和参与生态系统建设。

> [!CAUTION]
> 从安全角度来看，请像对待项目中的其他依赖项一样对待 MCP 服务器。使用前请仔细审查其源代码、验证发布者，并考虑安全影响。只使用你信任的 MCP 服务器，谨慎授予其访问敏感资源或执行敏感操作的权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置此 VS Code 工作区可使用的 MCP 服务器。

1. 在 codespace 中打开 `.vscode/mcp.json`。
2. 你应能看到已经配置好的 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

项目模板中已包含 `github` 条目，因此上一练习中 Copilot 才能读取待办列表中的 issue。该条目使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装任何内容。VS Code 会通过 HTTP 连接到该服务器；Copilot 首次使用其工具时，你需要通过 GitHub 身份验证。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]可以让 Copilot 操控浏览器，正适合用于测试功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 条目，与 `github` 条目并列，使文件如下所示：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会通过运行 `npx @playwright/mcp@latest` 启动它。`--headless` 标记表示 Playwright 将在没有可见窗口的情况下运行浏览器。codespace 中没有桌面环境，因此必须使用无头模式。

> [!NOTE]
> Tailspin Toys 项目已使用 Playwright 运行端到端测试，因此通常已安装 Playwright 所需的浏览器。如果 Copilot 之后报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会**按需**启动 MCP 服务器：Copilot 首次需要服务器工具时，VS Code 会启动服务器并询问你是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会出现 **Start** 操作。选择它以立即启动服务器并确认配置正确。
2. 如果 VS Code 询问你是否信任服务器，请审查配置并选择信任，以便服务器启动。

手动启动是可选的。如果跳过此步骤，下一步中 Copilot 首次需要服务器工具时，VS Code 会启动服务器并询问你是否信任它。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]可以让 Copilot 操控真实浏览器。与自己操作应用来验证更改不同，智能体可以打开页面、浏览页面、应用筛选条件、读取结果并向你总结观察结果。无需离开当前会话，就能快速确认功能是否符合预期。

Playwright MCP 服务器在底层使用页面的[无障碍访问树][playwright-mcp-server]，而不是截图。因此，智能体会像辅助技术一样，根据带有标签的结构化元素（按钮、链接和列表项）进行推理。快速功能检查也就兼具了简单的无障碍检查。

连接服务器并启动应用后，提示 Copilot 测试刚完成的筛选功能：

```text
使用 Playwright MCP 服务器打开浏览器，访问正在运行的应用 http://localhost:4321，并验证新的游戏筛选功能：

1. 转到游戏页面，记下列表中有多少款游戏。
2. 应用类别筛选条件，确认列表更新后只显示该类别中的游戏。
3. 清除该条件，然后应用发行商筛选条件，确认列表更新后只显示该发行商的游戏。
4. 同时应用类别和发行商筛选条件，确认结果同时符合两个条件。

报告你在每个步骤中的观察结果，并指出任何行为异常之处。
```

Copilot 会通过 Playwright MCP 服务器启动浏览器、逐步完成操作，并汇报观察结果。根据 issue 中的验收条件审查报告。如果发现问题，可以继续提问，或要求 Copilot 在创建拉取请求之前修复代码。

> [!NOTE]
> 测试期间，应用必须在 `http://localhost:4321` 上运行。如果已停止开发服务器，请在发送提示前重新启动。Copilot 首次使用 Playwright MCP 服务器时，可能需要下载浏览器。如果报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求 (PR)，让团队审查。第一步是发布 `filtering-vscode` 分支。

1. 在 codespace 中打开 **Source Control** 面板，审查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![Source Control 面板截图，其中显示 Copilot 所做的更改](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

创建拉取请求有几种方式，例如使用 github.com 或 GitHub 命令行界面 (CLI)。既然你正在使用 GitHub Copilot，不如让它代为创建 PR！它可以找到相关 issue，并创建与之关联的 PR。

1. 打开 Copilot Chat 面板，选择 **New Chat** 开始新会话。
2. 确认在智能体下拉列表中选中 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 提示 Copilot 为你创建 PR：

   ```plaintext
   查找存储库中与按类别和发行商筛选相关的 issue。为当前分支创建新的拉取请求，并将其关联到正确的 issue。
   ```

4. 根据需要选择 **Continue**，允许 Copilot 执行获取信息和执行操作所需的任务。首次使用 GitHub 工具时，系统可能会要求你通过 GitHub 身份验证；按照提示完成操作即可。
5. 留意 Copilot 如何搜索 issue、找到正确的 issue 并创建 PR。
6. 选择 Copilot 生成的链接审查拉取请求，但**暂时不要合并**。

## 总结和下一步

恭喜！在本练习中，你：

- 了解了模型上下文协议 (MCP) 以及 MCP 服务器如何为 Copilot 扩展新工具；
- 将 Playwright MCP 服务器添加到工作区；
- 使用该服务器在发布前通过浏览器手动测试筛选功能；
- 发布分支并为筛选工作创建拉取请求。

你使用 MCP 服务器测试了功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，[创建自定义智能体][next-lesson]，简化无障碍审查等专门任务。

## 资源

- [MCP 到底是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP 服务器][playwright-mcp-server]
- [GitHub MCP 服务器][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
