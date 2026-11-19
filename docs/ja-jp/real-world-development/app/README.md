---
slug: ja-jp/real-world-development/app
title: "GitHub Copilot app"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app](https://docs.github.com/copilot/concepts/agents/github-copilot-app)** は、Copilot CLI を基盤とするデスクトップアプリケーションで、エージェント主導の開発を1つの集中できるワークスペースにまとめます。並行するエージェントセッション、切り替え可能なセッションモード、共有キャンバス、GitHub issue とプルリクエストのネイティブ管理機能を利用できます。**Agent Merge** は、リベース、レビューのフィードバック、継続的インテグレーション (CI) の修正、マージを通じてプルリクエストを進めます。

このワークショップでは、Tailspin Toys の一連のワークフローに取り組みます。

1. プロジェクトを準備し、アプリをインストールしてリポジトリを接続し、ワークスペースと用意されたバックログを確認する。
2. 星評価の小さな変更を行い、ブラウザーで確認して、最初のプルリクエスト (PR) を手動でマージする。
3. フィルターの issue を起点に、**Plan** モードで実装方法を決め、**Autopilot** モードで実装して、**Interactive** モードでレビューする。
4. リポジトリの指示を更新し、フィルター機能の作業に適用する。
5. 既存の `quality-checks` スキルをカスタマイズし、プロジェクトのチェックを実行する。
6. Playwright Model Context Protocol (MCP) サーバーを追加し、ブラウザー上でフィルター機能を確認する。
7. quality assurance (QA) カスタムエージェントを作成して使い、要件、カバレッジ、検証の証拠を確認する。
8. フィルター機能全体をレビューし、2 つ目の PR に Agent Merge を使う。
9. キャンバスを利用できるか確認してから、Database Explorer を使い、リポジトリに保存するトリアージキャンバスを作成してテストする。必要な機能を利用できない場合は、ブロッカーを記録し、読み取り専用の代替方法を使うか、まとめに進みます。

ワークショップを集中して進めるため、PR は 2 つ作成します。1 つ目は星評価、2 つ目は指示、スキルの更新、QA プロファイル、テストを含むフィルター機能です。それぞれ、更新済みの `main` から開始します。フィルター機能と品質ワークフローでは、同じセッション、worktree、ブランチを使って、各ツールの使い方を学びながら作業を積み重ねます。最後のキャンバス演習では、PR ワークフローを繰り返さず、共有サーフェスの作成とテストに集中できるよう、そのセッション内で作業を続けます。

## レッスン

| レッスン | トピック | 説明 |
|--------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | Node.js をインストールし、Tailspin Toys プロジェクトの自分用コピーを作成する |
| [1. Copilot app のインストール][ex1] | セットアップ | アプリをインストールしてプロジェクトを接続し、ワークスペースを確認する |
| [2. 星評価の追加: 小さな成果][ex2] | 最初の変更 | 既存の評価と未評価時の表示を追加し、PR 1 をマージする |
| [3. エージェントモード: Plan と Autopilot][ex3] | エージェントモード | issue から機能を計画し、Autopilot で構築して、Interactive モードでレビューする |
| [4. カスタム指示で Copilot をガイドする][ex4] | コンテキスト | 指示を確認・更新し、フィルター機能に適用する |
| [5. quality-checks スキルのカスタマイズと使用][ex5] | 繰り返し可能なチェック | 既存スキルを確認し、レポート形式を変更して実行する |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーによる確認 | Customize から MCP を構成し、フィルターの動作を確認する |
| [7. QA エージェントの作成と使用][ex7] | 要件とカバレッジ | 専門家プロファイルを作成・選択して、最終検証の証拠を集める |
| [8. 機能 PR の作成とマージ][ex8] | レビューとマージ | フィルター、指示、スキル、QA プロファイル、テストをレビューし、2 つ目の PR に Agent Merge を使う |
| [9. キャンバスの確認と作成][ex9] | コラボレーション | Database Explorer を使い、リポジトリに保存するトリアージキャンバスを作成してテストする |
| [10. 振り返りと次のステップ][ex10] | まとめ | ワークフロー、成果物、追加リソースを確認する |

## 前提条件

このワークショップに参加する前に、次の準備をしてください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、または Enterprise** プランの GitHub アカウント
- [ ] **macOS、Linux、Windows** を実行するコンピューター
- [ ] コンピューターに[インストールされた Git][install-git]

> [!TIP]
> 有料プランをお持ちでない場合、認定を受けた学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使う agent、MCP、コードレビュー、Copilot CLI の機能が含まれるため、すべてのハーネスを完了できます。

> [!NOTE]
> Copilot app は Codespace ではなく自分のコンピューターで動作します。[前提条件の演習][ex0]では、アプリをインストールする前に Node.js をインストールし、プロジェクトのコピーを作成します。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使う場合は、管理者が **GitHub Copilot app** ポリシーを有効にしておく必要があります。このポリシーは Copilot CLI のポリシーとは別です。

## 開始する

**[前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-app/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-canvases/
[ex10]: 10-review/
[install-git]: https://github.com/git-guides/install-git
[callout-student-plan-education]: https://github.com/education/students
