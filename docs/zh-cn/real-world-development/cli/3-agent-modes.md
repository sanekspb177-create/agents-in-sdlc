---
title: "课程 3 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 确认实现方法，使用 Autopilot 根据 issue 实现筛选功能，并在 Interactive 模式中审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们先为项目添加了一项小功能，但更大的更改需要更完善的流程。GitHub Copilot CLI 可以融入组织现有的工作方式，确保我们用正确的方法构建正确的功能。接下来的几节课程将介绍典型的智能体驱动开发流程：从 issue 开始创建功能，确保代码正确、功能符合预期，最后成功合并到项目中。

> [!NOTE]
> 继续完成此功能工作流时，你会使用同一个会话和分支。通常，每类文件的工作会使用不同分支或 PR，但本工作坊为了专注于核心概念会简化流程。

在本课程中，你将：

- 根据 GitHub issue 启动新的 Copilot 会话；
- 在 Plan 模式中确定需求；
- 使用 Autopilot 模式实现新功能；
- 审查代码；
- 在转发的浏览器中手动验证功能。

继续实现此功能时，你还会更新存储库说明、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体并创建功能 PR。

## 场景

Tailspin Toys 的游戏目录正在不断扩大，访问者需要按类别和发行商缩小范围。待办 issue 描述了此功能，但组合类别等详细信息需要在编码前达成共识。你将使用 Plan 模式确定这些决策，然后批准 Autopilot 执行范围明确的实现。

## 背景

在开发流程中引入 AI 编码智能体并不会改变基本原则。事实上，这些原则变得更加重要！大多数开发者遵循的流程大致如下：

1. 从描述需求的 issue 开始。
2. 规划需要构建的内容。
3. 构建并审查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求 (PR)。
7. 代码通过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程会因团队和组织而异，但大多数流程都与上述流程类似。

遵循这种标准方法，可以确保 AI 生成的代码满足需求，并像手写代码一样经过审查和验证。

## 会话模式

**会话模式**控制智能体的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可循环切换模式：

- **Interactive**：你与智能体协作。智能体提出更改建议，并等待你的输入后再继续。
- **Plan**：智能体先创建计划，且不能编辑项目文件。
- **Autopilot**：智能体自主工作，编写代码、运行测试并不断迭代，直到任务完成。

先从 Plan 模式开始，审查计划，然后切换到 Autopilot 模式实现计划。

## 从 issue 开始

开始筛选功能前，返回 Codespace 并确认存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后创建筛选功能分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd>，打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有 issue。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的 issue。
9. 按 <kbd>C</kbd> 将 issue 添加到提示中，然后返回 **Session** 选项卡。

请注意，提示现在以 `#7`（或类似编号）开头。`#` 可将 GitHub issue 或拉取请求 (PR) 加入上下文。

## 规划筛选功能

规划可以先确定功能实现方法或任务执行方式，然后再交给 Copilot。处理任何复杂任务时，花一些时间规划总是值得的。现在切换到 Plan 模式，让 Copilot 创建计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的 issue 引用后输入以下提示：

   ```plaintext
   为实现此功能制定计划。
   ```

Copilot 会开始创建计划！它会先探索项目，再确定最佳方法。

3. Copilot 可能会询问筛选功能应如何工作。根据自己的偏好回答即可，没有错误答案！
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 滚动查看并审查计划。
6. 如果计划中的某些内容不符合你的决定，请让 Copilot 修改。

## 批准 Autopilot

计划已编写并审查，现在可以开始实现！切换到 Autopilot，让 Copilot 开始工作。

Autopilot 会让 Copilot 不断迭代问题，直到它认为任务已经完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或你所用版本中类似的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 按计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准计划后，实现可能会立即开始，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再将检查视为已完成。

## 审查并验证实现

代码生成后，和其他代码一样，需要先审查才能合并。现在审查代码并运行网站，确保一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能的实现和测试。
3. 将结果与 issue 和规划期间做出的决定进行比较。
4. 审查完代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 检查项目检查结果，并要求 Copilot 解决任何失败项。

## 试用新功能

代码看起来没问题，但能否正常运行？像之前一样启动应用，然后通过 Codespaces 转发端口打开网站。

1. 让 Copilot 启动应用：

   ```plaintext
   启动应用，让我在浏览器中试用筛选功能。告诉我 URL，并让服务器继续运行。
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按发行商筛选，以及根据计划中达成共识的条件组合筛选。
4. 确认重置筛选和无结果状态符合 issue 和你的决定。
5. 返回 Codespace，并让 Copilot 停止它启动的开发服务器。

## 总结和下一步

你已使用不同会话模式构建并审查功能。在本课程中，你：

- 根据 GitHub issue 启动了新的 Copilot 会话；
- 在 Plan 模式中确定了需求；
- 使用 Autopilot 模式实现了新功能；
- 审查了代码；
- 在转发的浏览器中手动验证了功能。

接下来，了解如何通过[自定义说明][next-lesson]更深入地控制代码生成，让代码遵循记录在案的实践。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
