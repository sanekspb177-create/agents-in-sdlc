---
title: "练习 1 - 自定义说明（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解一些背景信息，就应确保它能访问这些上下文。[说明文件][instruction-files]用于提供此类指导，让 Copilot 不仅了解你希望它做什么，也了解如何完成。

在本练习中，你将：

- 了解项目专属上下文、编码准则和文档标准如何通过存储库自定义说明及路径范围说明文件传递给 Copilot；
- 在 `.github/copilot-instructions.md` 中添加一条新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 工作环境不同，本练习不会运行“前后对比”提示。Copilot 云智能体会异步处理 GitHub issue，因此难以实时并排展示说明的效果。稍后审查云智能体创建的拉取请求时，你会看到说明文件的影响。

## 说明文件

### 场景

Tailspin Toys 和优秀的软件开发团队一样，制定了一套开发实践准则和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用深色模式并具有现代感。
- 应通过 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应有一段注释，说明文件的用途。

通过使用说明文件，可以确保 Copilot 了解这些实践要求，并据此完成任务。

### 自定义说明

自定义说明可为 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。借助此功能，可以引导 Copilot 提供更相关的建议和代码片段。你可以指定偏好的编码约定、库，以及希望添加的注释类型。可以为整个存储库创建说明，也可以针对特定文件类型创建说明，为任务提供上下文。

说明文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个说明文件。此文件应包含项目级信息，即大多数 Copilot Chat 或 CLI 请求都适用的上下文，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建的说明文件。你可以为 TypeScript 或 Astro 等语言编写指南，也可以为创建 UI 组件或一组新测试等任务编写指南。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，都会使用存储库级说明文件以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot [还支持其他标准的说明文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理说明文件的最佳实践

完整介绍如何创建说明文件超出了本工作坊的范围。不过，示例项目展示了具有代表性的做法。概括来说：

- 将 `copilot-instructions.md` 的内容限定为项目级指导，例如项目说明、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（例如单元测试、Astro 组件和数据层）或任务提供具体说明。
- 使用自然语言，确保指导清晰，并提供代码示例说明应该和不应该采用的形式。

创建说明文件没有唯一的正确方式，正如使用 AI 也没有唯一的正确方式。你可以通过实践找出适合自己项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应配备完善的说明文件。浏览本项目中的文件时，你可能会注意到其中包含多种任务的说明，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成说明文件。不同产品界面的操作方式有所不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。当前工作环境的课程会在相关处介绍具体操作。
>
> 想找模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中收集了说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览本项目中的自定义说明文件

花点时间阅读此存储库附带的说明文件：其中有一个核心 `copilot-instructions.md` 文件，以及一组针对不同任务的 `*.instructions.md` 文件。请在编辑器或 GitHub Web 界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，注意其中简要的项目说明，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。Copilot 在你与它交互时都会使用这些说明。
3. 打开 `.github/instructions` 文件夹并查看其中内容。注意其中有针对 Astro 文件、Drizzle 数据层、测试等内容的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它指定了适用于哪些文件的 glob 模式。此处匹配任何 TypeScript 测试文件，例如符合 `**/*.test.ts` 模式的文件。
5. 查看此项目中创建单元测试的专门说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到文件底部。注意指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。通过这种方式，可以将较大的说明集拆分成较小、可复用的文件，并链接到可供 Copilot 参考的示例。（这些路径是相对于说明文件所在位置，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求添加代码内文档。接下来，你会添加 TSDoc 文档注释和文件注释头的规则。

## 添加新的存储库标准

接下来需要编辑一处内容：添加一条项目级规则，要求代码使用 TSDoc 文档注释和文件级注释头。稍后在本工作环境中为云智能体分配 issue 时，它会遵循这条规则。

编辑前，先创建并切换到一个分支（云智能体会从 issue 目标分支读取说明，但仍应以干净的方式提交本次更改）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在为项目添加编码标准，以改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（应位于第 27 行附近）。注意其中记录了项目编码标准，但尚未提及代码内文档，因此生成的辅助函数没有文档注释。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级别（`.github/copilot-instructions.md`）创建说明文件，为整个项目提供指导；也可以使用 `*.instructions.md` 文件针对特定语言、文件类型或任务提供说明。你刚添加的文档注释规则属于项目级标准，因此存储库级文件是合适的位置。

## 提交、推送并合并说明更新

云智能体会从 issue 目标分支读取说明。下一练习中将 issue 分配给 Copilot 时，它会从 `main` 分支创建新分支，因此必须将这些说明更新合并到 `main`，云智能体才能使用。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求并完成合并。下一练习中，云智能体就会读取这些说明。

> [!TIP]
> 如果希望在本工作坊中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。此处创建分支是为了让工作坊更接近真实项目中的处理方式。

## 总结和下一步

你已了解 Copilot 如何读取说明文件中的上下文，并在 `.github/copilot-instructions.md` 中添加了新的存储库级标准。接下来几节中，云智能体创建的拉取请求会体现这条标准的作用。

接下来，将[第一个 issue 分配给 Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更优质 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 说明文件和其他资源集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
