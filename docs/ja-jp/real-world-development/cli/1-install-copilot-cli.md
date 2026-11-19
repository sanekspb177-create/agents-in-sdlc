---
title: "レッスン 1 - GitHub Copilot CLI のインストール"
description: "Codespace に Copilot CLI をインストールして認証し、使い方を確認して、用意されたフィルター issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作する強力なエージェント型コーディングアシスタントです。コードベースの探索、コード生成、コマンドの実行、外部ツールとのやり取りを、すべてコマンドラインから行えます。タスクを任せたり、変更を依頼したりしながら、作業に集中できます。まずはツールをインストールしましょう。すでに使っているツールを使ってインストールできます。

このレッスンでは、次のことを行います。

- npm を使って GitHub Copilot CLI をインストールする。
- GitHub アカウントで認証する。
- ワークショップのリポジトリを信頼し、簡単な会話を試す。
- 組み込みの GitHub MCP サーバーからフィルター issue を見つける。

## シナリオ

チームは、増え続けるバックログに取り組むため、AI エージェントを使い始めています。Copilot CLI を使うと、多くの開発者が普段利用するターミナルでその機能を使えます。このレッスンでは、インストールと認証を済ませ、ワークショップの残りで利用できる状態にします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew でインストールできます。GitHub Codespaces には Node.js があらかじめインストールされているため、npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされ、必要なバージョンを満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されます。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

初回起動時に、Copilot CLI は GitHub アカウントでの認証を求めます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 認証を求められた場合は、デバイスコードの手順に従って Copilot CLI を認証し、許可します。
3. Copilot CLI に次のプロンプトが表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. パスが Tailspin Toys のリポジトリであることを確認し、**Yes, and remember this folder for future sessions** を選択して信頼します。

> [!NOTE]
> Codespace では、GitHub セッションを通じてすでに認証されている場合があります。Copilot CLI が認証を求めずに起動した場合は、そのまま進めて問題ありません。

## 使い方を確認する

通常のシェルプロンプトで入力したコマンドは、Codespace で直接実行されます。Copilot CLI の起動後は、自然言語をエージェントに送信し、スラッシュコマンドで会話を操作します。

1. `/model` と入力し、矢印キーで **Auto** を選択して <kbd>Enter</kbd> を押します。もう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` と入力して、インストール済みのバージョンで利用できるコマンドを確認し、<kbd>Esc</kbd> を押してヘルプ画面を閉じます。
3. Copilot が動作するか確認するため、簡単な質問をします。

   ```plaintext
   このプロジェクトの主なファイルを教えてください。
   ```

4. 回答を読み、Copilot が回答前にリポジトリを調べることを確認します。
5. `/mcp list` と入力し、組み込みの GitHub MCP サーバーが利用できることを確認します。
6. 次のように依頼して、フィルター issue を探します。

   ```plaintext
   GitHub MCP を使って、このリポジトリから「Allow users to filter games by category and publisher」というタイトルの issue を探してください。URL と短い概要を教えてください。何も変更しないでください。
   ```

7. URL を開いて issue を読みます。最初の変更に進む前に内容を確認しておきます。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトされているブランチを使います。自動的に worktree が作成されるわけではありません。各変更の前に機能ブランチを作成します。

## ワークショップ用のショートカットを使う

Copilot CLI は通常、許可されていないツールを使う前に確認を求めます。このワークショップでは、Codespace 内の承認プロンプトを省いて演習に集中できる、ユーザーが許可したショートカット `--yolo` を使って再起動します。

> [!CAUTION]
> `--yolo` は、すべての自動権限 (`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`) を有効にします。Codespace や VM など、分離された環境でのみ使い、日常の開発で既定にしたりエイリアスに設定したりしないでください。詳細は[ツールの使用を許可・拒否する][allow-all-warning]を参照してください。

このワークショップでは、後のレッスンで issue やプルリクエストを操作するために、`--enable-all-github-mcp-tools` を指定して GitHub MCP の読み取り・書き込みツールを有効にします。Codespace によってローカルコンピューターへのアクセスは制限されていますが、認証済みの GitHub リソースは実際のものです。公開やマージの前に変更を確認してください。

1. `/exit` で Copilot CLI を終了します。
2. リポジトリのルートから再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. 会話が動作するか確認するため、プロジェクトについて簡単な質問をします。その後、`/exit` で終了します。

Copilot は会話を自動的に保存します。後のレッスンで指示を変更したりエージェントを追加したりしたら、`copilot --resume` で同じ機能の会話とブランチに戻ります。

## まとめと次のステップ

おめでとうございます。このレッスンでは、次のことを行いました。

- npm を使って GitHub Copilot CLI をインストールした。
- GitHub アカウントで認証した。
- ワークショップのリポジトリを信頼し、簡単な会話を試した。
- 組み込みの GitHub MCP サーバーからフィルター issue を見つけた。

次は[最初の小さな変更を始め][next-lesson]、Copilot CLI でゲームカードに星評価を表示します。

## リソース

- [GitHub Copilot CLI のインストール][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
