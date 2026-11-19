---
title: "课程 5 - 自定义并使用 quality-checks 技能"
description: "探索现有的 quality-checks 技能、自定义报告格式，并用它验证筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

编写代码不只是写代码。我们已经手动验证代码能够运行，也使用说明文件确保代码符合标准，但测试呢？Lint 呢？持续集成 (CI) 中的其他步骤又该如何处理？

对于这类任务，**智能体技能**最为合适！技能可以帮助 Copilot 了解如何正确执行这些操作。

在本课程中，你将：

- 探索现有的 `quality-checks` 技能；
- 自定义结果的显示格式；
- 重新加载并运行技能。

## 场景

Tailspin Toys 使用 `quality-checks` 技能运行单元测试、lint 和类型检查。团队希望改进报告格式，让结果更易阅读。

## 说明、脚本和资源

智能体技能将可复用的任务说明、可执行脚本和支持资源打包在一起，由智能体按需加载。技能本质上是一个以技能名称命名的文件夹，其中包含一个名为 `SKILL.md` 的 Markdown 文件。该 Markdown 文件包含定义技能名称和说明的 frontmatter、技能的功能概述，以及有关何时调用技能的指导。文件夹中还可以包含供技能使用的脚本和其他资源子文件夹。

> [!NOTE]
> 技能不一定需要额外文件或文件夹。Tailspin Toys 的 `quality-checks` 技能只包含 `SKILL.md`，因为它使用项目中已有的命令。

技能可以位于项目的 `.github/skills` 文件夹中，作为存储库资产供团队共享和复用；也可以放在用户技能文件夹 `~/.copilot/skills` 中。

## 探索技能

现在来看看 Tailspin Toys 团队为运行单元测试、lint 和类型检查创建的 `quality-checks` 技能。

1. 返回 Codespace。在 Codespaces 编辑器中打开 `.github/skills/quality-checks/SKILL.md`。
2. 阅读文件顶部的 `name` 和 `description`。说明内容可帮助 Copilot 判断何时调用技能。
3. 阅读说明，留意它如何指导 Copilot 执行测试和 lint 流程。
4. 注意技能目前尚无 **Results output formatting** 部分。

## 修改前先运行技能

技能可以通过 Copilot CLI 命令或自然语言直接调用。现在让 Copilot 运行该技能中的三项检查。

1. 返回筛选功能会话并进入 Interactive 模式。
2. 使用以下提示：

   ```plaintext
   运行 quality-checks 技能，执行单元测试、lint 和类型检查。
   ```

3. 查看最终报告。

## 自定义报告

现在需要改进报告，以便清楚说明执行了哪些检查、结果如何以及工具报告了什么。接下来更新技能，使其生成这样的报告。

1. 返回 `.github/skills/quality-checks/SKILL.md`。
2. 在文件末尾添加以下部分：

   ```markdown
   ## Results output formatting

   完成后，报告运行过的每条命令，并说明其通过、失败或受阻。仅在工具提供了这些信息时，才包含测试数量、持续时间、错误、警告和其他指标。对任何失败或阻塞项指出后续操作，绝不要将跳过或未完成的检查描述为通过。
   ```

3. 文件会自动保存。

## 运行更新后的技能

现在运行并检查更新效果！Copilot CLI 可以在不重启会话的情况下重新加载经过编辑的技能。

1. 输入：

   ```plaintext
   /skills reload
   ```

2. 使用与之前完全相同的提示：

   ```plaintext
   运行 quality-checks 技能，执行单元测试、lint 和类型检查。
   ```

3. 查看最终报告并与第一次报告进行比较。

## 总结和下一步

你已自定义并使用现有的智能体技能。在本课程中，你：

- 探索了用于运行单元测试、lint 和类型检查的 `quality-checks` 技能；
- 自定义了技能结果的显示格式；
- 重新加载并运行了技能。

这项更改会与筛选功能一起包含在功能 PR 中。接下来，让 Copilot [通过 Playwright MCP 服务器直接与网站交互][next-lesson]。

## 更多技能示例

以下社区示例仅供参考，不是额外任务：

- [Agent Skills 规范][skill-spec]
- [贡献工作流：`make-repo-contribution`][contribution-example]
- [需求文档：`prd`][prd-example]
- [图表及其打包的导出脚本：`drawio`][drawio-example]
- [浏览器测试：`webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
