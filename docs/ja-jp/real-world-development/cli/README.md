---
slug: ja-jp/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Copilot CLI のモード、カスタマイズ、MCP ツール、プルリクエストの自動化を学びながら、Tailspin Toys に 2 つの変更を実装、検証、リリースします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** は、GitHub Copilot をエージェント型コーディングアシスタントとしてターミナルで利用できるようにします。コマンドラインからコードベースを探索し、コードを生成し、コマンドを実行し、外部ツールに接続できます。グラフィカルエディターに切り替えずに、作業の流れを保てます。

このワークショップでは、Tailspin Toys の一連のワークフローを通して学びます。

1. GitHub Codespaces でプロジェクトを準備し、Copilot CLI をインストールして、プロジェクトの概要を把握する。
2. 星評価の小さな変更を行い、ブラウザーで確認して、最初のプルリクエスト (PR) を手動でマージする。
3. フィルターの issue を起点に、Plan モードで実装方法を決め、Autopilot モードで実装して、Interactive モードでレビューする。
4. リポジトリの指示を更新し、フィルター機能の作業に適用する。
5. 既存の `quality-checks` スキルをカスタマイズし、プロジェクトのチェックを実行する。
6. Playwright Model Context Protocol (MCP) サーバーを追加し、ブラウザー上でフィルター機能を確認する。
7. quality assurance (QA) カスタムエージェントを作成して使い、要件、カバレッジ、検証の証拠を確認する。
8. フィルター機能全体をレビューし、Agent Merge を使ってフィルター PR を処理する。
9. コンテキスト、モデル、共有、省略可能な cloud agent への委任に使うスラッシュコマンドを確認する。

ワークショップを集中して進めるため、PR は 2 つ作成します。1 つ目は星評価、2 つ目は指示の更新、スキルの更新、QA プロファイル、テストを含むフィルター機能です。フィルターと品質ワークフローでは、同じ会話とブランチを使って、各ツールの使い方を学びながら作業を積み重ねます。

## レッスン

| レッスン | トピック | 説明 |
| ------ | ----- | ----------- |
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成する |
| [1. Copilot CLI のインストール][ex1] | インストール | Copilot CLI をインストールして認証し、使い方を確認する |
| [2. 星評価の追加: 小さな成果][ex2] | 最初の変更 | 既存の評価と未評価時の表示を追加し、最初の PR をマージする |
| [3. エージェントモード: Plan と Autopilot][ex3] | エージェントモード | issue から機能を計画し、Autopilot で構築して、Interactive モードでレビューする |
| [4. カスタム指示で Copilot をガイドする][ex4] | コンテキスト | 指示を確認・更新し、フィルター機能に適用する |
| [5. quality-checks スキルのカスタマイズと使用][ex5] | 繰り返し可能なチェック | 既存スキルを確認し、レポート形式を変更して実行する |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーによる確認 | CLI で MCP を構成し、フィルターの動作を確認する |
| [7. QA エージェントの作成と使用][ex7] | 要件とカバレッジ | 専門家プロファイルを作成・選択して、最終検証の証拠を集める |
| [8. 機能 PR の作成とマージ][ex8] | レビューとマージ | 変更全体をレビューし、PR を作成して Agent Merge を使う |
| [9. GitHub Copilot CLI のスラッシュコマンド][ex9] | CLI 機能 | コンテキスト、モデル、共有、省略可能な cloud agent への委任を確認する |
| [10. 振り返りと次のステップ][ex10] | まとめ | ワークフロー、再利用できるカスタマイズ、追加リソースを確認する |
| [省略可能: Foundry を組み込む][foundry] | ホスト型エージェント | モデルを準備し、カタログに基づくエージェントをデプロイして Web サイトに接続する |

## 前提条件

このワークショップに参加する前に、次の準備をしてください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、または Enterprise** プランの GitHub アカウント
- [ ] リポジトリと Codespace を作成する権限
- [ ] ターミナルまたはコマンドライン操作の基本的な知識

> [!TIP]
> 有料プランをお持ちでない場合、認定を受けた学生は [GitHub Education][student-plan] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使う agent、MCP、コードレビュー、Copilot CLI の機能が含まれています。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使う場合は、管理者が Copilot CLI を有効にしていることを確認してください。

## 開始する

**[前提条件から始める →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
