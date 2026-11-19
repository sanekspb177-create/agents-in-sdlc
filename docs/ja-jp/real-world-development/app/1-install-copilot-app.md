---
title: "レッスン 1 - GitHub Copilot app のインストール"
description: "GitHub Copilot app をインストールし、テンプレートから作成したリポジトリを接続して、ワークスペースを確認し、クイックチャットを試します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app][about-copilot-app]** は、エージェント主導の開発向けデスクトップアプリケーションです。GitHub Copilot CLI を基盤とし、GitHub とネイティブに統合されているため、リポジトリ、ブランチ、CI パイプラインをすぐに利用できます。複数のエージェントをそれぞれ分離されたワークスペースで並行して動かし、作業を指示したり、繰り返しタスクを自動化したりできます。Node.js のインストールとプロジェクトの準備ができたので、次はアプリをインストールしてリポジトリに接続します。

このレッスンでは、次のことを行います。

- GitHub Copilot app をインストールしてサインインする。
- GitHub リポジトリからプロジェクトをアプリに追加する。
- テンプレートから作成したバックログを含め、ワークスペースの使い方を確認する。
- アプリについて理解するため、簡単なチャットを試す。

## シナリオ

チームは、増え続けるバックログに取り組むため、AI エージェントを導入しています。Copilot app では、issue の選択、エージェントの実行、変更のレビュー、プルリクエストのマージを 1 か所から指示できます。このレッスンでは、アプリをインストールして接続し、プロジェクトについての会話を始められるようにします。

> [!NOTE]
> 対象となる Copilot プランが必要です。Copilot Free、Copilot Student、または有料プラン (Pro、Pro+、Business、Enterprise) を利用してください。Copilot Business または Enterprise を使う場合は、管理者が **GitHub Copilot app** ポリシーを有効にしておく必要があります。

## GitHub Copilot app をインストールして構成する

GitHub Copilot app を使うには、まずアプリをインストールします。Windows、macOS、Linux 向けのバージョンがあります。アプリをインストールして認証し、Tailspin Toys リポジトリを追加します。

1. ブラウザーで [GitHub Copilot app のランディングページ][download-app]を開きます。
2. 使用しているプラットフォーム向けのアプリをダウンロードし、ページの手順に従ってインストールします。
3. インストール後、アプリを開きます。
4. **Sign in to GitHub** を選択し、GitHub.com または GHE.com アカウントでサインインします。データ所在地に対応する GitHub Enterprise Cloud を使う場合は **GitHub Enterprise Cloud** を選択し、求められたら `*.ghe.com` のホスト名を入力します。

   GitHub Enterprise Server を使う場合は、まずクラウドアカウントでオンボーディングを完了します。その後、**Settings** を開き、**Accounts**、**Add account**、**GitHub Enterprise Server** の順に選択します。[Enterprise Server のサインインガイド][ghes-sign-in]に従い、ブラウザーでアプリを承認します。

5. オンボーディング中にリポジトリを選択する画面が表示された場合は、テンプレートから作成した Tailspin Toys リポジトリを選択します。リポジトリの選択をスキップして、オンボーディング後に追加することもできます。
6. テーマを選ぶよう求められたら選択し、オンボーディングを完了します。
7. サイドバーの **Sessions** でリポジトリを探します。すでに一覧にある場合は、再度追加する必要はありません。
8. まだ接続されていない場合は、**Sessions** の横にある **+** を選択して新しいセッションを作成し、**Add GitHub repository** を選択します。前提条件のレッスンで控えた所有者名とリポジトリ名を使います。
9. リポジトリがサイドバーに表示されることを確認します。このレイアウトでは、`tailspin-toys` などのリポジトリフォルダーが **Sessions** の下にまとめられ、個々のセッションは各リポジトリの下に表示されます。

## ワークスペースの使い方を確認する

プロジェクトを接続したら、アプリの使い方を確認します。サイドバーには、いくつかの領域があります。

- **New** - 新しい Copilot チャットセッションを開始します。
- **Pull requests** - アプリの GitHub ネイティブ統合でプルリクエストを表示します。アプリを離れずに参照、絞り込み、CI 状態の確認、レビューができます。
- **Issues** - issue を参照したり、絞り込んだり、issue からセッションを開始したりできます。
- **Automations** — スケジュールまたはオンデマンドで実行する、保存済みのエージェントタスクです。やることリストの管理、定期的なプロジェクト保守、その他の面倒な作業を任せるのに役立ちます。振り返りで次のステップとして紹介し、ここでは演習しません。
- **Customize** - MCP サーバー、プラグイン、スキルなどのコンポーネントを追加して Copilot app の機能を拡張します。Playwright MCP の設定に使います。
- **Chats** — 独自のブランチやワークスペースを必要としない、質問やブレインストーミング向けの簡易的な会話です。最初のチャットを開始すると表示されます。表示されない場合は、**Sessions** の横にある **+** を選択してチャットを開始します。
- **Sessions** — リポジトリフォルダーと、その中のエージェントセッションを表示します。`tailspin-toys` などのリポジトリ名を選択するとリポジトリのビューが開き、その下のセッションを選択すると会話に戻ります。セッションはそれぞれ分離されたワークスペースで動くため、変更を競合させずに複数の作業を並行できます。星評価を追加する際に最初のセッションを開始します。

ワークショップを進めながら、ワークスペースの各機能を確認します。

> [!TIP]
> 迷ったときは Copilot に質問しましょう。操作方法が分からない場合や、何かが可能かどうかを知りたい場合は、Copilot に尋ねると案内してくれます。

### 用意されたバックログを確認する

バックログのないプロジェクトはほとんどありません。Tailspin Toys も例外ではありません。テンプレートからリポジトリを作成したときに用意されたバックログを確認します。

1. サイドバーの **Sessions** で、Tailspin Toys のリポジトリフォルダーを選択します。その下の個別セッションではなく、リポジトリを選んでください。次に、リポジトリの上部ナビゲーションで **Issues** を選択します。
2. issue 番号を決めつけず、次のタイトルを探します。

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. issue を選択して内容を読みます。各 issue からエージェントセッションを開始できます。まず小さな変更を行い、その後にフィルター issue から作業を始めます。

> [!NOTE]
> **Issues** と **Pull requests** の一覧には、Copilot app に追加したリポジトリの項目だけが自動的に表示されます。他のリポジトリの作業を表示する場合は、そのリポジトリもアプリに追加してください。

## クイックチャットを試す

アプリに慣れるには、アプリ自体について質問するのが効果的です。その用途には **quick chat** が適しています。クイックチャットでは、ブランチや worktree を作成せずに質問やブレインストーミングができます。セッションを必要としない、その場限りの質問に向いています。

1. サイドバーの **Chats** の横にある **+** を選択して、新しいチャットを開きます。**Chats** がまだ表示されていない場合は、**Sessions** の横にある **+** を選択して最初のチャットを開始します。開始後に **Chats** が表示されます。
2. アプリのセッションについて質問します。

   ```plaintext
   GitHub Copilot app は worktree をどのように使いますか。
   ```

3. 会話ビューで回答を読みます。各セッションが分離された Git worktree で動くため、変更を競合させずに複数のエージェントを並行して実行できます。会話は続けることも、いつでも新しいチャットを開始することもできます。

## まとめと次のステップ

GitHub Copilot app をインストールし、プロジェクトを接続して、ワークスペースを確認しました。次のことを学びました。

- アプリをインストールして GitHub にサインインする。
- GitHub リポジトリからプロジェクトを追加する。
- ワークスペースの使い方を確認し、**Issues** で用意されたバックログを見つける。
- クイックチャットで、その場限りの簡単な質問をする。

次は[最初のエージェントセッションを開始し][next-lesson]、ゲームカードに星評価を表示します。

## リソース

- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app の使用を開始する][getting-started]
- [GitHub Copilot app でエージェントセッションを操作する][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
[ghes-sign-in]: https://docs.github.com/enterprise-server@3.22/copilot/copilot-on-ghes/sign-in-on-copilot-app
