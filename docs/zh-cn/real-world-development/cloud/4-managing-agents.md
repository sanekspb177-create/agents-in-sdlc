---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前两节练习中，你让 Copilot 云智能体处理了三个独立任务，用于改进用户体验和添加功能。云智能体的设计目标是异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心，你可以查看所有具有开放拉取请求 (PR) 的智能体任务、探索已执行的操作，甚至在会话进行中提供指导。

在本课程中，你将：

- 探索智能体页面并监控云智能体任务；
- 在进行中的会话中提出额外功能要求。

## 场景

将实现高对比度模式的任务分配给智能体后，团队意识到也可以顺便添加浅色模式。既然正在更新网站样式并添加切换功能，加入这项功能也很合理。你需要指导智能体，确保它同时添加浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

查看分配给 Copilot 云智能体的所有任务的当前状态。

1. 转到 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。列表中应能看到你分配给 Copilot 的任务，包括：
    - 更新代码库的文档；
    - 为修改游戏生成 API；
    - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的步骤，包括：
    - 从存储库检出代码；
    - 为 Copilot 创建环境；
    - 设置 MCP 服务器；
    - 执行完成所分配任务所需的各种步骤。

> [!NOTE]
> 具体步骤会因 Copilot 工作的当前状态及其所采用的方法而异。

4. 同时留意右侧出现的拉取请求 (PR) 窗格。在此处可以查看 PR 和更改的文件，以便进一步监控。

## 指导云智能体

现在，你已了解正在执行的任务。接下来，在智能体处理高对比度模式时，要求 Copilot 同时添加浅色模式切换开关。

1. 选择与添加高对比度模式有关的会话。会话的确切标题取决于 Copilot 使用的名称和当前工作状态。

    ![任务控制中心中的无障碍访问会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它显示已完成设置并开始工作。出现类似于以下消息时，表示智能体已开始执行任务。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示：

    ```plaintext
    我们正在实现高对比度模式，也请同时添加浅色模式。该模式也应有一个开关，供用户选择自己偏好的显示模式。
    ```

    ![智能体页面上的云智能体任务截图，其中突出显示了 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认提示并将其纳入工作流程。

## 等待 Copilot 完成

和之前一样，Copilot 会继续处理更新后的任务！完成收到消息时正在执行的步骤后，它会将新请求加入工作流程。

这同样需要几分钟，是暂停并回顾目前学到的内容和探索过的功能的好时机。

## 总结和下一步

本课程介绍了 Copilot 智能体页面，这是监控和指导 GitHub Copilot 云智能体任务的中心位置。在这个任务控制中心中，你可以跟踪所有正在进行和已完成的任务、审查智能体执行的工作，甚至重定向正在运行的任务，以调整范围或提供额外指导。

你已了解以下内容：

- 使用任务控制中心和智能体页面监控云智能体任务；
- 重定向正在运行的会话，要求添加额外功能。

Copilot 正在完成无障碍功能的工作。接下来，我们将[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
