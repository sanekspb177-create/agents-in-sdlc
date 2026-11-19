---
title: "レッスン 0 - 前提条件"
description: "GitHub Copilot app のレッスンに向けて、Tailspin Toys プロジェクト用の Node.js をインストールし、テンプレートからリポジトリの自分用コピーを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

GitHub Copilot app は、Copilot と GitHub を一元的に利用するためのデスクトップアプリです。issue やプルリクエストにすばやくアクセスし、GitHub Copilot を使って開発できます。このワークショップでは、GitHub Copilot app を使い、Astro で構築された Tailspin Toys アプリをローカル環境で更新します。始める前に、Node.js がローカルにインストールされていることを確認し、Copilot app をインストールしましょう。

このレッスンでは、次のことを行います。

- プロジェクトのテストを自分のコンピューターで実行できるように Node.js をインストールする。
- テンプレートから Tailspin Toys プロジェクトの自分用コピーを作成する。

## Node.js をインストールする

いくつかのレッスンでは、エージェントに機能を作成させ、Tailspin Toys のテストスイートをローカルで実行します。そのためには、プロジェクトで必要な唯一のランタイムである **[Node.js][nodejs]** が必要です。現在の **LTS** リリースをインストールしてください。

どのプラットフォームでも、公式インストーラーを使うのが最も簡単です。

1. Windows Terminal、macOS のターミナルなど、普段使っているターミナルを開きます。
2. 次のコマンドを実行して、インストールされている Node.js のバージョンを確認します。

    ```shell
    node --version
    npm --version
    ```

3. どちらのコマンドも成功し、Node.js がプロジェクトの README と `package.json` に記載された要件を満たしている場合は、次のセクションに進めます。

> [!TIP]
> Node.js がインストールされていない場合、または更新が必要な場合にのみ、次の手順を実行してください。

4. [Node.js のダウンロードページ][node-download]を開きます。
5. 使用しているオペレーティングシステム向けの **LTS** ビルドをダウンロードします。
6. インストーラーを実行し、既定の設定を選択します。Windows では **Add to PATH** を選択したままにします。
7. インストール後、新しいターミナルウィンドウを開きます。
8. 新しいターミナルで次のコマンドを実行し、インストールを確認します。

    ```bash
    node --version
    npm --version
    ```

9. インストールされた Node.js と npm のバージョンが表示されます。

> [!IMPORTANT]
> Copilot app のエージェントからも `node --version` と `npm --version` を実行できる必要があります。普段使うターミナルで成功しても、アプリから利用できるとは限りません。Node.js のインストールや PATH の変更後は、アプリを完全に終了して再起動してください。セッションでどちらかのコマンドが失敗する場合は、続行する前に[セッション環境のトラブルシューティング手順][session-environment]に従ってください。

> [!IMPORTANT]
> 各 worktree には、プロジェクトの依存関係と E2E チェック用の Playwright Chromium も必要です。worktree を準備する際は Tailspin Toys リポジトリの README に従い、インストールの要求は内容を確認してから承認してください。

## 演習用リポジトリをセットアップする

Tailspin Toys プロジェクトの自分用コピーで作業します。[テンプレートリポジトリ][template-repository]から今すぐ作成してください。新しいリポジトリには演習に必要なファイルがすべて含まれています。次のレッスンでそのリポジトリをアプリに接続します。

1. 新しいブラウザーウィンドウで、このラボの GitHub リポジトリ `https://github.com/github-samples/tailspin-toys` を開きます。
2. ラボ用リポジトリのページで **Use this template** ボタンを選択し、**Create a new repository** を選択して自分用のコピーを作成します。

    ![ドロップダウンで Create a new repository が選択された Use this template ボタン](../../../_images/app-0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターの案内に従ってください。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成します。

    ![github-samples/tailspin-toys がテンプレートに設定され、リポジトリ名が入力された Create a new repository フォーム](../../../_images/app-0-create-repository.png)

4. 作成したリポジトリのパス (**organization-or-user-name/repository-name**) を控えてください。この後の演習で使用します。

> [!NOTE]
> テンプレートからリポジトリを作成すると、GitHub issue のバックログも自動的に作成されます。ワークショップではこれらの issue を使うため、自分で起票する必要はありません。

ワークショップ用テンプレートの新しいコピーを使ってください。リポジトリの指示、アプリケーションのコード、テスト、`quality-checks` スキル、既存のキャンバス拡張機能が含まれています。ワークショップではスキルをカスタマイズし、QA エージェントを作成します。古いコピーを使う場合は、必要なファイルがあるか進行役に確認してください。

## まとめと次のステップ

準備ができました。このレッスンでは、次のことを行いました。

- プロジェクトを自分のコンピューターでビルドしてテストできるように Node.js をインストールした。
- テンプレートから Tailspin Toys リポジトリの自分用コピーを作成した。

次は、[GitHub Copilot app をインストールし][next-lesson]、作成したリポジトリを接続して、ワークスペースを確認します。

## リソース

- [Node.js のダウンロード][node-download]
- [テンプレートからリポジトリを作成する][template-repository]
- [GitHub Copilot app について][about-copilot-app]

[next-lesson]: ../1-install-copilot-app/
[nodejs]: https://nodejs.org/
[node-download]: https://nodejs.org/en/download
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[session-environment]: ../3-agent-modes/#issue-からセッションを開始する
