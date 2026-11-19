---
title: "练习 3 - 自定义智能体"
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

接下来，你将使用自定义智能体了解：

- 如何定义自定义智能体；
- 如何将任务分配给自定义智能体。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，不受视觉能力或偏好影响。近期用户反馈指出，由于文字与背景颜色对比度不足，有些用户难以阅读当前的深色主题。为解决这一无障碍问题，设计团队要求实现一种可供用户开关的高对比度模式。

无障碍访问至关重要，因此你希望尽快实现此功能。接下来将使用自定义智能体生成相关功能。

## 审查无障碍访问自定义智能体

已经为你创建了一个无障碍访问自定义智能体。先审查其内容，了解它将如何引导 Copilot。

返回 codespace，然后查看无障碍访问自定义智能体文件：

1. 打开 `.github/agents/accessibility.md`。
2. 注意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含带有 `name` 和 `description` 的 frontmatter。

3. 浏览并审查接下来的部分，其中包括：
   - 生成无障碍网站代码时的核心职责；
   - 无障碍访问最佳实践；
   - HTML、CSS 和 JavaScript 代码示例；
   - 常见陷阱和错误。

## 创建并分配 issue

任务控制中心是管理环境中所有智能体的中心位置。你可以在此处将任务分配给 Copilot 云智能体、监控任务，也可以重定向并提供额外指导。现在，先将实现高对比度模式的任务分配给 Copilot。

1. 转到存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新建 issue 对话框。
4. 选择 **Blank issue**，创建空白 issue。
5. 将 **Title** 设置为 `Add high contrast mode to website`。
6. 将 **Description** 设置为：

    ```plaintext
    我们需要为网站添加高对比度模式。应提供一个供用户选择是否启用高对比度的切换开关，并在浏览器的 local storage 中保存设置。
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。
9. 从自定义智能体列表中选择 **Accessibility agent**。

    ![云智能体分配界面截图，其中突出显示了自定义智能体和无障碍访问智能体](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 将在后台开始处理任务！

## 总结和下一步

本课程介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，也就是为特定任务和领域定制的 AI 助手。借助自定义智能体，你可以将团队的专业知识和标准整理成可复用的智能体，引导 Copilot 更有效地完成特定类型的工作。

你已了解以下内容：

- 如何定义自定义智能体；
- 如何将任务分配给自定义智能体。

Copilot 正在实现高对比度模式。接下来，你将学习如何在[任务控制中心监控和指导智能体会话][next-lesson]。

## 资源

- [关于自定义智能体][custom-agents]
- [为组织使用自定义智能体做好准备][org-custom-agents]
- [为企业使用自定义智能体做好准备][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot 云智能体][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
