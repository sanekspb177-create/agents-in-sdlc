---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

多くの組織が、何らかの技術的負債を抱えています。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能リクエストなどです。GitHub Copilot の cloud agent は、コードの更新や機能の追加といった作業を自律的に実行できます。作業が完了すると、人間の開発者がレビューできるドラフト PR を作成します。時間のかかる作業を任せることで、開発者はより大きな課題に集中できます。

この演習では、Copilot cloud agent を使って次のことを行います。

- コード生成環境をカスタマイズする。
- 操作を安全に行う方法を確認する。
- 範囲を明確に定めた issue の重要性を理解する。
- issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys には解消したい技術的負債があります。最初のサイトを開発した委託先がドキュメントを適切に整備しておらず、実際にはほとんど存在しません。まず、アプリケーションのエクスポートされたすべての関数に TSDoc のドキュメントコメントを追加します。

さらに、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する他のタイトルを「関連ゲーム」として表示する機能を求めています。完成度の高い実装はまだ必要ありません。UX の受け入れテストに使えるものを用意したいのです。この作業は現在ブロッカーですが、優先度の高い issue が他にもあります。

どちらも、優先順位が下がりやすい作業の例です。Copilot cloud agent に割り当てると、非同期に作業を進めてもらえます。その間、別の作業に集中し、後から戻って作業内容をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドで作業できます。たとえば、[GitHub issue を Copilot に割り当て][assign-issue]ます。割り当てると、Copilot は進行状況を追跡するドラフトプルリクエストを作成し、環境をセットアップして、作業を開始します。作業中も完了後も Copilot のセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単そうな操作にも、掘り下げると複雑さがあります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。AI ペアプログラマーとして扱い、段階的に進めながら学び、試し、調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と Copilot への影響を確認しました。カスタム指示の演習を完了していれば、`.github/copilot-instructions.md` が機能することを確認済みです。まだの場合は、cloud agent に作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認し、後からその効果を確かめましょう。

リポジトリの GitHub Web UI、または起動済みの Codespace で次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクション、特に TypeScript の規約と TSDoc ドキュメントコメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` フロントマターを確認します。この指示は `**/*.test.ts` ファイルに適用されます。

次のセクションで `Code lacks documentation` issue を cloud agent に割り当てると、指示ファイルに基づく TSDoc コメント、TypeScript の規約、コメントヘッダーを確認できます。後の演習で PR をレビューするときに、改めて注目します。

## Copilot cloud agent の開発環境を設定する

誰がコードを書く場合でも、通常は適切な環境とセットアップスクリプトが必要です。Copilot にタスクを割り当てる場合も同様で、Copilot はソフトウェアエンジニアのように作業します。

Cloud agent は作業時に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップワークフロー][setup-workflow]を作成すると、作業開始前に実行するツールや依存関係を指定できます。このラボでは、Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザー、単一 Astro アプリ用にマイグレーションとシードを適用した SQLite データベースを利用できるよう、あらかじめ設定されています。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *手順*に必要な最小限の権限を設定します。Copilot には操作用の独自トークンが付与されます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順の一部としてリポジトリをクローンするには、`contents: read` 権限が必要です。セットアップ手順でクローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js（アプリ全体は Astro + Drizzle/libSQL です）
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

      # ビルドとテストでデータを利用できるように、ローカルの SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

このファイルは一般的な GitHub ワークフローと同じですが、重要な点がいくつかあります。

- `copilot-setup-steps` という単一のジョブがあり、Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot の実行を待たずに、正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く認識されていますが、多くのプロジェクトでは情報が古かったり、まったく存在しなかったりします。この種の技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しい開発者の受け入れを難しくします。Copilot はドキュメントの作成を得意とするため、cloud agent に割り当てるのに適した issue です。バックグラウンドで必要なドキュメントを生成し、後の演習で作業をレビューできます。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して、新しい issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して issue を作成します。
8. 右側で **Assign to Copilot** を選択し、割り当てダイアログを開きます。

  ![issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

  ![Copilot の割り当て詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のような内容です。一覧に PR が表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が todo リストを作成していることを確認します。

> [!NOTE]
> Copilot が PR に todo リストを表示するまで、数分かかる場合があります。前述のワークフローを実行して環境を作り、プロジェクトを分析して、問題に取り組む方法を決めています。

13. todo リストと、Copilot が実行するタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot のセッション画面](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示が現れるまで、ウィンドウの再読み込みが必要な場合があります。

16. ライブセッションをスクロールし、Copilot が問題を解決する様子を確認します。コードを調べて状況を把握し、適切な計画を考え、コードを作成する過程を確認できます。

この作業には数分かかる場合があります。Copilot cloud agent の主な目的の 1 つは、非同期にタスクを処理し、開発者が別の作業に集中できるようにすることです。この機能を活用するため、別のタスクも cloud agent に割り当て、その間にアプリケーションの機能を実装します。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の大きな利点は、作業を分担できることです。デザインチーム向けに関連ゲームの表示を追加する作業は短時間で終わるかもしれませんが、その時間を別の作業に充てることもできます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して issue を作成します。
8. 右側で **Assign to Copilot** を選択し、割り当てダイアログを開きます。

  ![issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、issue の最初のコメントに 👀 が表示され、Copilot が作業を始めたことを確認できます。

![issue の作業中を示す目の絵文字を Copilot が表示している画面](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組んでいます。Copilot cloud agent はソフトウェアエンジニアのように動くため、常に監視する必要はありません。完了後にレビューできます。それでは、カスタムエージェントの作成と利用に進みましょう。

## まとめと次のステップ

このレッスンでは [GitHub Copilot cloud agent][copilot-agents] を確認しました。cloud agent に issue を割り当てると、非同期に作業してもらえます。技術的負債への対応、新機能の作成、あるフレームワークから別のフレームワークへのコード移行などに利用できます。

次の概念を学びました。

- コード生成環境をカスタマイズする。
- 操作を安全に行う方法を確認する。
- 範囲を明確に定めた issue の重要性を理解する。
- issue を Copilot に割り当てる。

cloud agent がバックグラウンドで作業している間に、カスタムエージェントの作成と利用に進みます。[Copilot cloud agent は MCP サーバーも利用できます][cloud-agent-mcp]。前のモジュールで扱ったカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
