---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

几乎所有组织都会面临技术债务问题。这可能是尚未解决的安全问题、需要更新的旧代码，或因时间不足而长期搁置的功能请求。GitHub Copilot 云智能体旨在自主执行更新代码和添加功能等任务。智能体完成工作后，会创建一个草稿 PR，供开发者审查。这样可以将繁琐任务交给智能体，加快开发流程，让开发者专注于更重要的工作。

你将在本练习中探索 Copilot 云智能体的以下用法：

- 自定义代码生成环境；
- 确保操作以安全的方式执行；
- 明确界定 issue 范围的重要性；
- 将 issue 分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初受雇开发网站的承包商没有做好文档工作——你会发现，代码完全缺少文档。首先，团队希望为应用中的所有导出函数添加 TSDoc 文档注释。

此外，设计团队准备改进游戏发现体验。他们希望游戏详情页显示相关游戏，也就是同一类别中的其他游戏。团队目前不要求完善的实现，只需要能用于用户体验验收测试的版本。此任务目前是一个阻碍，不过还有其他优先级更高的 issue。

这两类任务都很容易被降级为低优先级，正适合分配给 Copilot 云智能体。Copilot 云智能体可以异步完成这些任务，让开发者先处理其他工作，之后再回来审查 Copilot 的成果并确认一切符合预期。

## 认识 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发者一样在后台执行任务。你可以通过多种方式启动任务，例如[将 GitHub issue 分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。任务分配后，Copilot 会创建用于跟踪进度的草稿拉取请求、设置环境并开始工作。会话进行中或完成后，你都可以查看 Copilot 的会话。提出的解决方案准备好供你审查时，Copilot 会在拉取请求中标记你。

## 明确界定说明范围的重要性

GitHub Copilot 并非魔法——它无法替你省去思考。即使是看似简单的操作，深入分析后也会发现其复杂性，因此[为 Copilot 云智能体明确界定任务范围][cloud-agent-best-practices]非常重要。应将它视为 AI 结对编程伙伴：分阶段工作、学习、实验并不断调整。引入生成式 AI 并不会改变软件开发的基本原则。

## 此存储库中的自定义说明

前面的练习介绍了自定义说明及其如何引导 Copilot。如果已经完成自定义说明练习，你就了解了 `.github/copilot-instructions.md` 的作用；如果还没完成，现在可以快速查看一下。将工作分配给 Copilot 云智能体之前，先以只读方式查看存储库中已有的说明文件，之后就能留意它们的影响。

在 GitHub Web 界面中打开存储库中的以下文件；如果已启动 codespace，也可以在其中打开：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其留意 TypeScript 约定和 TSDoc 文档注释的要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter，它会将这些说明限定用于 `**/*.test.ts` 文件。

在下一节中将 `Code lacks documentation` issue 分配给云智能体时，留意它创建的拉取请求是否包含 TSDoc 文档注释、TypeScript 约定和注释头。这些内容来自你审查过的说明文件。之后的练习中，我们还会再次检查这一点。

## 为 Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，以确保项目状态良好。将任务分配给 Copilot 时也一样，因为它会像软件工程师一样完成任务。

云智能体使用 [GitHub Actions][github-actions] 环境来执行工作。你可以创建一个[专用设置工作流][setup-workflow]，配置在 `.github/workflows/copilot-setup-steps.yml` 文件中，并让它在云智能体开始工作前运行，以便提供所需的开发工具和依赖项。此工作坊已预先配置了该工作流，确保课程顺利进行并提供学习机会。对于这个 Astro 单体应用，该工作流会为 Copilot 安装 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及已迁移和填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许你从存储库的“Actions”选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 仅设置步骤实际需要的最低权限。Copilot 会获得自己的令牌来执行操作。
    permissions:
      # 如果希望在设置步骤中克隆存储库（例如为了安装依赖项），需要 `contents: read` 权限。如果未在设置步骤中克隆存储库，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在都是 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试使用其中的数据。
      - name: Set up the database
        run: npm run db:setup
```

该文件看起来和其他 GitHub 工作流文件类似，但有几个关键点：

- 它包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会运行此作业。
- 注意 `workflow_dispatch` 触发器。它允许你从存储库的 **Actions** 选项卡手动运行工作流，以便测试其能否成功运行，而不必等待 Copilot 启动它。

## 添加文档

每个人都知道文档的重要性，但大多数项目的信息要么已过时，要么完全缺失。这类技术债务经常被忽视，降低工作效率，也让维护代码库或引入新团队成员变得更加困难。幸运的是，Copilot 非常擅长生成文档，因此这是分配给 Copilot 云智能体的理想 issue。它会在后台生成所需文档。之后的练习中，你会回来审查生成的内容。

1. 在新的浏览器标签页中，转到 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新建 issue 对话框。
4. 选择 **Blank issue**，创建新 issue。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    我们组织要求在适当情况下为函数和方法添加 TSDoc 文档注释。遗憾的是，近期更新没有遵循此标准。我们需要更新现有代码，确保在有助于说明行为的地方添加文档注释。
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 Copilot 分配给 issue](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

  ![Copilot 分配详细信息](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的拉取请求 (PR)，其标题可能类似于 `[WIP]: Code lacks documentation`。如果列表中尚未显示新 PR，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 生成的待办事项列表可能需要几分钟才会出现在 PR 中。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方案。

13. 查看 Copilot 即将完成的任务列表。
14. 向下滚动拉取请求时间线，应能看到 Copilot 已开始处理 issue。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的状态指示器。

16. 注意，你可以滚动查看实时会话，了解 Copilot 如何解决问题，包括探索代码并了解当前状态、暂停思考并确定合适的计划，以及创建代码。

这可能需要几分钟。Copilot 云智能体的主要优势之一，是可以异步执行任务，让你腾出时间处理其他工作。接下来，我们会利用这一点：先给 Copilot 云智能体分配另一个任务，然后转而编写代码，为应用添加功能。

## 为游戏详情页添加相关游戏

如前所述，GitHub Copilot 云智能体的一大优势是可以拆分工作：你专注于一组任务的同时，它可以处理另一组任务。为设计团队添加相关游戏区域可能不会花太长时间，但这段时间仍可用于处理其他工作。现在就将此任务分配给 Copilot 云智能体！

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新建 issue 对话框。
4. 选择 **Blank issue**，使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

    ```markdown
    我们希望在每款游戏的详情页上显示相关游戏，帮助用户发现更多游戏。设计团队希望探索用户体验并进行验收测试。要求如下：

   - 在 `src/lib/` 中添加数据访问辅助函数，返回与指定游戏属于同一类别的其他游戏，但不包括指定游戏本身
   - 在游戏详情页上显示一个使用该辅助函数的“相关游戏”部分
   - 处理游戏没有相关游戏的情况
   - 为新辅助函数创建单元测试
   - 创建 PR 前，确保所有测试都通过
   ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 Copilot 分配给 issue](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

很快，你就会看到 issue 的第一条评论旁边出现一组 👀，表示 Copilot 已开始处理！

![Copilot 使用眼睛表情符号表示正在处理 issue](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 现在正在认真处理你的第二个请求！Copilot 云智能体的工作方式类似于软件工程师，因此你不需要主动监控它；等任务完成后再审查即可。接下来，我们来创建和使用自定义智能体。

## 总结和下一步

本课程介绍了 [GitHub Copilot 云智能体][copilot-agents]。使用云智能体可以将 issue 分配给 Copilot，让它异步执行任务。你可以使用 Copilot 处理技术债务、创建新功能，或协助将代码迁移到其他框架。

你已了解以下内容：

- 自定义代码生成环境；
- 确保操作以安全的方式执行；
- 明确界定 issue 范围的重要性；
- 将 issue 分配给 Copilot。

Copilot 云智能体正在后台处理任务。接下来，我们将了解如何创建和使用自定义智能体。[Copilot 云智能体还可以使用 MCP 服务器][cloud-agent-mcp]，并且能够使用前面模块中介绍的自定义说明。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
