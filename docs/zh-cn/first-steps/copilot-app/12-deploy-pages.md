---
title: "课程 12 - 使用 GitHub Pages 部署网站"
description: "使用 GitHub Copilot 应用配置 Pages 工作流、审查拉取请求并验证 Space Quiz 已上线。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

使用 GitHub Pages 为 Space Quiz 创建公开的访问入口。Copilot 可以准备工作流并指导部署，但仍需由你负责审查更改并确认网站已上线。

在本课程中，你将：

- 让 Copilot 配置 GitHub Pages 部署。
- 在合并前审查工作流和拉取请求。
- 跟进部署并验证已发布的网站。

## 场景

Space Quiz 已在 GitHub 存储库中，但用户还没有方便的试用方式。使用 GitHub Pages 发布这个单文件网站，就能通过公开 URL 访问它，无需添加服务器或运行时依赖项。

## 让 Copilot 准备部署

> [!WARNING]
> 创建拉取请求可能会提交并推送代码。发送此提示前，请检查批准设置和目标存储库。如果选择了 **全部批准**，不要期待系统会再次要求确认。

在 `space-quiz` 的 Copilot 应用会话中发送以下提示：

```plaintext
为此存储库准备 GitHub Pages 部署。如果需要，请创建用于构建网站的 GitHub Actions 工作流，并在更改进入默认分支后将网站部署到 GitHub Pages。如果存储库中尚无 README，请添加一个，介绍此应用以及如何在本地运行它。创建包含这些更改的拉取请求，并告知我链接以及任何必需的 Pages 设置或权限。不要合并、启用自动合并、更改 Pages 设置或部署。完成后暂停并等待我审查。
```

## 审查工作流和拉取请求

1. 确认仅当存储库原本没有 README 时才添加了 README。
2. 审查拉取请求中的更改，包括工作流的构建和部署步骤、触发条件及权限。
3. 确认工作流从默认分支部署，而不是从尚未合并的拉取请求部署。
4. 如果需要更正，请提出要求并审查更新后的差异。授权合并前，等待必需的拉取请求检查通过。

## 授权合并和部署

> [!WARNING]
> 以下提示授权智能体合并已审查的拉取请求、更改存储库的 Pages 设置并发布网站。只有在完成审查并确认存储库和拉取请求后，才发送此提示。根据批准设置，系统可能不会再次要求确认。

将 `<PR_NUMBER>` 替换为已审查的拉取请求编号，然后单独发送以下提示：

```plaintext
我已审查此存储库中的拉取请求 <PR_NUMBER>，并授权合并和部署。使用 GitHub Actions 启用 GitHub Pages；只有在必需检查通过后才合并该拉取请求；并监控部署，直到成功为止。告知我已上线网站的最终 URL。如果你没有启用 Pages 或合并的权限，请准确说明我需要批准或执行的操作；只有网站已上线后才能报告成功。
```

![三阶段示意图：已合并的 space-quiz 拉取请求；GitHub Actions 中成功运行的 Deploy website 工作流，其中包含 Build website、Upload artifact 和 Deploy to Pages 步骤；以及位于 yourname.github.io/space-quiz/ 的在线 Space Quiz。](../../../_images/first-steps-app-pages-deploy.svg)

## 验证部署

1. 跟进 GitHub Actions 部署运行，直到成功。
2. 打开报告中的 URL，确认 Space Quiz 已加载。

如果智能体无法启用 Pages 或合并拉取请求，请按照它提供的具体说明操作。只有部署成功且网站已加载，才能视为任务完成。

## 总结和后续步骤

Space Quiz 现已作为在线网站发布。你也亲自验证了部署，而不是只依赖智能体的报告。继续学习[课程 13：审查和后续步骤][next-lesson]。

[next-lesson]: ../13-review/
