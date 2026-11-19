---
title: "课程 4 - 使用自定义说明引导 Copilot"
description: "探索存储库说明、添加文档标准，并将其应用到筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，就应确保 Copilot 能访问相关指导。[说明文件][instruction-files]不仅会说明你希望生成什么代码，还会说明代码应如何组织。现在你已经实现了筛选功能，接下来将探索 Copilot 使用的说明、添加文档标准，并将其应用到代码中。

在本课程中，你将：

- 探索存储库说明和路径范围说明文件如何传递给智能体；
- 更新说明文件，确保遵循编码标准；
- 了解说明文件对代码的影响。

## 场景

Tailspin Toys 和优秀的软件开发团队一样，制定了一套开发实践准则和要求，包括：

- 注释应解释意图和非显而易见的决策，而不是重复代码内容。
- `db/` 和 `src/lib/` 中的导出函数应通过 TSDoc/JSDoc 注释说明用途、参数和返回值；如果有可注入的 `db` 参数，也要说明。
- 可复用 Astro 组件应说明其 `Props` 合约，并在相关代码发生更改时同步更新注释。
- 保留现有格式和 lint 指南。

通过使用说明文件，可以确保 Copilot 了解这些实践要求，并据此完成任务。

## 说明文件

自定义说明可为 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。借助此功能，可以引导 Copilot 提供更相关的建议和代码片段。你可以指定偏好的编码约定、库，以及希望添加的注释类型。可以为整个存储库创建说明，也可以针对特定文件类型创建说明，为任务提供上下文。

说明文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个说明文件。此文件应包含对大多数请求都适用的项目级信息。
- `.github/instructions/*.instructions.md`：可针对特定语言、文件类型或任务提供指导的说明文件。

> [!NOTE]
> 其他说明格式及其支持情况会因环境而异。依赖特定格式前，请查看[自定义说明支持参考][custom-instructions-support]。

## 浏览本项目中的自定义说明文件

入门项目中已包含一组说明文件。先探索现有文件，再进行更改，以便观察更改的影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（不是终端）中打开 `.github/copilot-instructions.md`。
3. 浏览文件，注意其中简要的项目说明和编码指导。此说明适用于在此存储库中与 Copilot 的所有交互。
4. 打开 `.github/instructions` 文件夹并查看其中内容。注意其中有针对 Astro 文件、Drizzle 数据层、测试等内容的说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它指定了适用于哪些文件的 glob 模式。
6. 打开 `.github/instructions/drizzle.instructions.md`，留意其中指向其他说明文件和项目现有文件的链接。通过这种方式，可以将较大的说明集拆分成更小、可复用的文件，并链接到示例。

## 更新说明文件以符合团队指导

现有文件已经很有帮助，但还不够。现在修改核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分（应位于第 35 行附近）。
2. 在最后一条下方添加以下内容：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存！

## 应用更新后的指导

Copilot CLI 会在会话开始时加载存储库说明。保存更新后的说明后，退出并恢复筛选会话，使 Copilot 在保留功能上下文的同时加载新的指导。

1. 如果筛选会话仍处于打开状态，输入 `/exit` 关闭。Copilot CLI 会自动保存会话。
2. 在存储库根目录的 `game-filters-cli` 分支中，恢复已保存的会话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选会话并确认当前检出的是 `game-filters-cli` 分支。
3. 让 Copilot 应用更新后的指导：

   ```plaintext
   我们刚刚更新了说明和代码指导。请更新你生成的代码，使其符合这些指导。
   ```

4. 输入 `/diff` 并查看已更改的 TypeScript 文件。注意新生成的 TSDoc 注释，并确认其准确解释了代码。

## 总结和下一步

你已了解 Copilot CLI 如何读取说明文件中的上下文，并将新的标准应用到筛选功能。具体来说，你：

- 探索了存储库说明和路径范围说明文件如何传递给智能体；
- 更新说明文件，确保遵循编码标准；
- 了解了说明文件对代码的影响。

接下来，[自定义并使用可复用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot — 说明文件和其他资源集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
