---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：代理模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解一些背景信息，就应确保它能访问这些上下文。可以通过多种方式向 Copilot 提供特定上下文，其中一种重要方式是使用[说明文件][instruction-files]来指导代码生成。

在本练习中，你将：

- 了解项目专属上下文、编码准则和文档标准如何通过存储库自定义说明及路径范围说明文件传递给 Copilot；
- 在当前说明文件的指导下，为筛选功能生成第一个数据切片（发行商辅助函数）；
- 在 `.github/copilot-instructions.md` 中添加一条新的存储库级标准；
- 重新运行相同提示，观察生成的代码如何遵循新标准；
- 将说明更新和筛选功能切片提交到 `main`，让 Copilot 在下一练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不符合你设定的部分标准。Copilot 的输出具有非确定性。本练习旨在展示更新说明后行为变化的*趋势*，而不是让每个字符都与示例完全一致。

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
2. 浏览文件，注意其中简要的项目说明，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。Copilot 在你与它交互时都会使用这些说明。在 **Code standards** 下方，注意嵌套的 **GitHub Actions Workflows** 指导。
3. 打开 `.github/instructions` 文件夹并查看其中内容。注意其中有针对 Astro 文件、Drizzle 数据层、测试等内容的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它指定了适用于哪些文件的 glob 模式。此处匹配任何 TypeScript 测试文件，例如符合 `**/*.test.ts` 模式的文件。
5. 查看此项目中创建单元测试的专门说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到文件底部。注意指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。通过这种方式，可以将较大的说明集拆分成较小、可复用的文件，并链接到可供 Copilot 参考的示例。（这些路径是相对于说明文件所在位置，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求添加代码内文档。接下来，你会添加 TSDoc 文档注释和文件注释头的规则。

## 为更改创建分支

遵循最佳实践，为本次更改创建一个分支。

1. 返回上一练习中使用的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始下面的练习前，返回 codespace，打开 Copilot Chat 面板，然后选择 **New Chat** 开始干净的新会话。每个练习所用的模式和模型可能不同，相关步骤会在适用之处说明。

先发送一个提示，在当前说明文件的指导下观察 Copilot 的效果。之后更新说明文件，再发送相同提示进行对比。

1. 关闭之前练习中打开的编辑器标签页，让 Copilot 只获取你需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，以便 Copilot 修改文件。

   ![Chat 视图中的智能体选择器截图。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   创建或更新 src/lib/publishers.ts，添加一个数据访问辅助函数，返回所有发行商的列表，并为每个发行商提供名称和 ID。应用文件更改。
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和测试。
6. 注意，提出的辅助函数是一个经过类型定义的函数，以 `db` 客户端作为第一个参数，并返回具有类型定义的发行商数组。这来自 `.github/instructions/drizzle.instructions.md` 中的数据层约定（该文件适用于 `src/lib/*.ts`）。
7. 注意，提出的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 的输出具有概率性，因此即使没有收到指示，它也可能添加文档注释。如果发生这种情况也没关系，本练习的重点仍是观察更新说明后的一致性改进。

## 添加新的存储库标准

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

## 重新发送提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat**，清空对话。
2. 再次选择 `src/lib/publishers.ts`，让 Copilot 专注于正确的文件。
3. 发送**相同提示**：

   ```plaintext
   创建或更新 src/lib/publishers.ts，添加一个数据访问辅助函数，返回所有发行商的列表，并为每个发行商提供名称和 ID。应用文件更改。
   ```

4. 注意，现在提出的文件开头会包含类似以下内容的注释块：

   ```typescript
   /**
    * Tailspin Toys 众筹平台的发行商数据访问辅助函数。
    * 提供从数据库检索发行商信息的函数。
    */
   ```

5. 注意，现在提出的函数会包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * 返回所有发行商的列表，包括其 ID 和名称。
    *
    * @param db - Drizzle 数据库客户端。
    * @returns 一个 Promise，解析为发行商对象数组。
    */
   ```

你刚才引导 Copilot 遵循新的项目标准，并将其应用于真实代码。下一练习会在此基础上继续构建。

## 提交说明并推送分支

说明文件和存储库中的其他资源一样，需要通过源代码管理进行维护。现在将更改提交并推送到存储库。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
2. 在终端中运行以下命令，确认说明更新和辅助函数更改已存在：

   ```bash
   git status
   ```

3. 在终端中暂存并提交说明更新，以及 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示了其他支持性更新（例如测试），也要添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 在终端中将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送完成后，就可以创建拉取请求并关联待办列表中的文档标准 issue。可以手动完成，也可以使用已连接到项目的 GitHub 工具，让 Copilot 代为处理。接下来提示 Copilot 查找 issue、创建可关闭该 issue 的 PR，然后合并 PR。

> [!NOTE]
> 这是第一次让 Copilot 代表你在 GitHub 上执行操作。项目模板已将 GitHub 工具连接到工作区，因此无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub。按照提示完成登录即可。后续练习会介绍该连接如何通过模型上下文协议 (MCP) 工作。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保在智能体选择器中选中 **Agent**。
3. 让 Copilot 查找与更新说明文件相关的 issue，并从当前分支创建 PR，说明其中既包含说明更新，也包含新的发行商辅助函数基础：

   ```plaintext
   查找与更新说明文件相关的 issue。从当前分支创建新的 PR，指出该 PR 会关闭此 issue，并说明我们还为即将开展的筛选工作添加了发行商辅助函数基础。
   ```

4. Copilot 会开始查找 issue 并创建 PR。
5. 如果系统提示你 **Allow** Copilot 代表你执行 GitHub 操作，请审查操作内容并酌情选择 **Allow**。
6. 创建 PR 后，使用以下提示要求 Copilot 将 PR 合并到 `main`，然后将本地分支切换回 `main`：

   ```plaintext
   将 PR 合并到 main。然后在本地切回 main，并拉取最新代码以确保代码是最新的。
   ```

7. 如果系统提示你 **Allow** Copilot 代表你执行 GitHub 操作和运行 shell 命令，请审查操作内容并酌情选择 **Allow**。

现在，你已借助 GitHub Copilot 创建并合并了拉取请求！

## 总结和下一步

你已了解 Copilot 如何读取说明文件中的上下文，并在 VS Code 中使用 Copilot Chat：

- 发送代码生成提示，观察 Copilot 根据*现有*说明生成的内容；
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准；
- 重新发送相同提示，观察生成的代码如何遵循新标准；
- 将说明更新和辅助函数基础提交到 `main`，为下一练习做好准备。

接下来，在[代理模式][next-lesson]中运用这些说明，让 Copilot 跨代码库添加新功能。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更优质 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义说明][personal-instructions]
- [Awesome Copilot — 说明文件集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
