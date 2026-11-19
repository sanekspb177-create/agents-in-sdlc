---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是针对开发工作流中特定任务或领域定制的 AI 助手。通过在存储库的 `.github/agents` 文件夹中使用 Markdown 文件定义智能体，你可以为 Copilot 提供专注的说明、最佳实践、编码模式和领域知识，帮助它更有效地完成特定类型的工作。团队可以将专业知识整理成可复用的智能体，例如执行 [WCAG][wcag] 合规要求的无障碍访问智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

可以通过项目 `.github/agents` 文件夹中的 Markdown 文件定义自定义智能体，也可以在 `~/.copilot/agents` 中进行全局定义。每个文件都包含 YAML frontmatter，至少需要 `name` 和 `description` 字段，后面是用于定义智能体行为、专长和说明的 Markdown 提示词。

### 自定义智能体与智能体技能的区别

自定义智能体与[智能体技能][agent-skills-concept]在逻辑上有一些重叠。两者主要通过 Markdown 文件定义，并告诉 AI 如何执行操作。最简单的区分方式是：**自定义智能体是执行工作的人员，技能是工具**。

自定义智能体有独立的上下文窗口，能够在工作时协调技能（甚至其他智能体）。在本实验中，无障碍访问自定义智能体会按照无障碍指南审查和更新网站；在执行过程中，它可以调用拉取请求工作流技能，或运行和管理测试的技能。

> [!NOTE]
> 编写自定义智能体没有唯一的“正确”方式。与 AI 领域的其他实践一样，应针对自己的环境和场景测试并迭代改进。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，不受视觉能力或偏好影响。近期用户反馈指出，由于文字与背景颜色对比度不足，有些用户难以阅读当前的深色主题。为解决这一无障碍问题，设计团队要求实现一种可供用户开关的高对比度模式。

无障碍访问至关重要，因此你希望尽快实现此功能。接下来将使用自定义智能体生成相关功能。

在本练习中，你将：

- 审查现有的无障碍访问自定义智能体；
- 在 Copilot Chat 中使用无障碍访问智能体实现高对比度模式。

## 审查无障碍访问自定义智能体

已经为你创建了一个无障碍访问自定义智能体。先审查其内容，了解它将如何引导 Copilot。

返回 codespace，然后打开终端，从 `main` 分支创建新的分支，用于无障碍功能工作（这样可以将筛选功能 PR 与练习 3 中的工作分开）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 注意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含带有 `name` 和 `description` 的 frontmatter。

3. 浏览并审查接下来的部分，其中包括：
   - 生成无障碍网站代码时的核心职责；
   - 无障碍访问最佳实践；
   - HTML、CSS 和 JavaScript 代码示例；
   - 常见陷阱和错误。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉列表中显示 `.github/agents` 中定义的所有自定义智能体。选择自定义智能体后，该聊天会话便会遵循相应智能体的说明和工具配置。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始下面的练习前，返回 codespace，打开 Copilot Chat 面板，然后选择 **New Chat** 开始干净的新会话。每个练习所用的模式和模型可能不同，相关步骤会在适用之处说明。

1. 如果 Chat 视图的智能体下拉列表中尚未选择 **Agent**，请先选择它。

   ![Chat 视图中的智能体选择器截图。](../../../_images/shared-chat-mode-selector.png)

2. 选择聊天视图底部的智能体下拉列表（其中显示当前智能体，默认为 **default**）。
3. 从可用智能体列表中选择 **Accessibility agent**。
4. 向无障碍访问智能体发送以下提示：

    ```plaintext
    为网站添加高对比度模式。应提供一个供用户选择是否启用高对比度的切换开关，并使用浏览器的 local storage 保存设置，以便刷新页面后仍然保留。
    ```

5. Copilot Chat 将开始工作：探索代码库、提出更改建议并编辑项目文件。每项编辑都会以内联方式显示在聊天中，并包含文件路径和可供审查的差异。
6. 随着编辑完成，**Files changed** 指示器会更新，显示智能体修改的工作集。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 会实际修改存储库中的文件，例如 Astro 组件、CSS 和相关测试。

下一练习中，你会审查并引导这项正在进行的工作。

## 总结和下一步

本课程介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，也就是为特定任务和领域定制的 AI 助手。借助自定义智能体，你可以将团队的专业知识和标准整理成可复用的智能体，引导 Copilot 更有效地完成特定类型的工作。

你已了解以下内容：

- 如何定义自定义智能体；
- 如何在 Copilot Chat 代理模式中使用自定义智能体。

接下来，你将[监控并指导智能体的工作][next-lesson]，审查实时更改，并在同一会话中添加浅色模式切换开关。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [awesome-copilot 上的自定义智能体][awesome-copilot-agents]

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
