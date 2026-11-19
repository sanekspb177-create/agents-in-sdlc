---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの小さな更新でも、通常は複数のファイルを変更し、テストを実行する必要があります。開発者は、必要なファイルを探し、変更を加え、テストを実行して、デバッグし、変更漏れがあればさらに修正する、といった作業を行います。こうした作業は何度も続くことがあります。

Copilot Agent Mode は、こうした状況で役立ちます。

Copilot Agent Mode は IDE 内でより自律的に作業します。開発者と同じように、まず既存のプロジェクト構成を調べ、必要な更新を行い、テストなどのタスクを実行して、見つかった問題を自動的に修正します。Agent Mode を使ってサイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる用途の機能です。Agent mode は IDE 内で作業し、素早くフィードバックを得て対話するためのものです。Cloud agent はチームメンバーのように非同期で作業し、issue とプルリクエストを通じてやり取りします。

この演習では、次の方法を学びます。

- Copilot Agent Mode でプロジェクトを調べ、関連ファイルを特定して、複数のファイルを連携して変更する。
- GitHub Copilot Agent Mode で UI とデータ層にまたがる新機能を実装する。
- Copilot Agent Mode が生成した変更とテストをレビューしてから、コードベースにマージする。

## シナリオ

ゲームの一覧が増えてきたため、ユーザーがカテゴリや出版社で絞り込めるようにします。前の演習では出版社ヘルパーを追加しました。ここでは、Copilot Agent Mode を使って残りのデータ層、UI、テストを完成させます。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを見て、現在の機能を確認しましょう。

この Web サイトは、開発者向けテーマのボードゲームを扱うクラウドファンディングプラットフォームです。ゲームの一覧表示と詳細表示ができます。このサイトは単一の Astro アプリで、ビルド時に静的 HTML としてページを生成します。ページはフロントマター内で Drizzle ORM を使ってローカルの SQLite データベースを直接参照します。別のバックエンド API やクライアントサイドの UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` に置き、インタラクティブな機能は標準 DOM API を使う小さな Astro `<script>` で追加します。

### Web サイトを起動する

Web サイトを簡単に実行できるように、単一 Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習を完了していれば、`main` ブランチにいるはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルウィンドウを開きます。
3. フィルター機能用の新しいブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を選択します。Mac では <kbd>Command</kbd>、Windows/Linux では <kbd>Ctrl</kbd> を押しながら選択します。

> [!NOTE]
> Codespace のターミナルで localhost URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的に転送されます。これは Codespace の Web サーバーをホストするプライベートなトンネルです。

### Web サイトを確認する

サイトを起動したら、機能を確認しましょう。主な機能は次のとおりです。

- **ホームページ**: ボードゲームの一覧と、タイトル、画像、説明を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、出版社、カテゴリなどの詳細ページが表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

Web サイトの初期実装は動作しますが、新しい機能を追加したいと考えています。まずバックログを確認しましょう。テンプレートからプロジェクトを作成すると、issue のバックログも自動的に作成されます。GitHub Copilot に項目を表示するよう依頼します。

1. Chat ビューのエージェント選択メニューから **Agent** を選択します。**Agent** はファイルを横断して変更を計画・実装し、ターミナルコマンドを実行して、ツールを呼び出します。

   ![Chat ビューのエージェント選択メニューの画面](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者は、特定のモデルを推奨しているわけではありません。ワークショップの作成時に Claude Sonnet 4.5 を使用したため、手順にも記載しています。提案されるコードにある程度一貫性を持たせ、スムーズに進めるためです。ただし、LLM の動作は確定的ではないため、ワークショップの記載と異なる提案が表示される場合があります。これは正常な動作です。

> [!NOTE]
> LLM の動作は確定的ではないため、Copilot が別の MCP コマンドを使う場合がありますが、タスクは同様に完了できるはずです。

3. 次のプロンプトを Copilot に送信して、issue のバックログを確認します。

   ```plaintext
   GitHub リポジトリのバックログを表示してください。ユーザーに最も役立つ項目を優先して整理してください。
   ```

4. **Continue** を選択して、すべての issue を一覧表示するコマンドを実行します。
5. 生成された issue の一覧を確認します。

Copilot が、ユーザーにとって最も役立つと思われる項目を優先して整理しています。

## 指示ファイルを確認する

Copilot にコード生成を依頼する前に、作業のコンテキストを伝える指示ファイルを確認します。この Web サイトに機能を追加する方法を説明する [ユーザーインターフェイス (UI) 指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加の進め方に関する全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要
   - コンポーネントの設計、テスト可能性、アクセシビリティの原則
   - Astro、Tailwind CSS、Drizzle など、ファイル形式ごとの指示ファイルへのリンク

> [!TIP]
> 指示ファイルから、他の指示ファイルとプロジェクト内のファイルの両方を参照できます。パスは指示ファイルを基準とした相対パスです。指示を再利用したり、大きな指示を小さく管理しやすい単位に分けたり、Copilot が参照できる例やテンプレートを用意したりできます。

## フィルター機能を実装する

フィルター機能を完成させるには、少なくとも次の 3 か所を更新します。

- データ層 (`src/lib/`) に残りのフィルターロジックを追加または改善する。
- フィルター動作のテストを追加または更新する。
- ゲーム一覧ページにフィルター UI を追加する。

さらに、コードベースにマージする前にテストを実行し、すべて成功する必要があります。Copilot Agent Mode にこれらの作業を依頼しましょう。

1. 現在の Copilot 会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択して、指示ファイルを追加します。

   ![UI 指示ファイルを選択する画面](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント選択メニューで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択メニューの画面](../../../_images/shared-chat-mode-selector.png)

4. モデルが **Claude Sonnet 4.5** のままであることを確認します。
5. バックログにある関連 issue の要件に基づいて機能を実装するよう、次のプロンプトで Copilot に依頼します。

   ```plaintext
   バックログの関連 GitHub issue の要件に基づき、出版社とカテゴリによるフィルターをサイトに追加してください。前の演習で追加した出版社ヘルパーは保持し、必要に応じて改善しながら、残りのデータ層、UI、テストを完成させてください。完了前にすべてのテストが成功することを確認してください。サーバーはすでに起動しているので、起動する必要はありません。
   ```

6. Copilot がプロジェクトを調べ、機能に関連するファイルを探す様子を確認します。データ層のヘルパー、UI、テストを見つけた後、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクトのファイルを調べる画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行うため、完了まで数分かかる場合があります。その間、エディターに **Keep** と **Undo** ボタンが表示されることがあります。Copilot が完了すると、すべての変更に **Keep** または **Undo** が表示されます。作業中に選択する必要はありません。

7. Copilot に確認されたら **Continue** を選択してテストを実行します。

   ![Copilot Chat ペインでテストの実行を確認するダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりする場合があります。問題ありません。Copilot は、タスクを完了し、エラーを検出しなくなるまでコード生成とテストを繰り返します。

   ![Copilot Agent Mode の完了したチャットセッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを起動しているブラウザーに戻り、新しい機能を確認します。
11. 動作とコードを確認したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot Agent Mode を使って Tailspin Toys の Web サイトに機能を追加しました。次のことを学びました。

- GitHub Copilot Agent Mode で UI とデータ層にまたがる新機能を実装する。
- Copilot Agent Mode でプロジェクトを調べ、関連ファイルを特定して、複数のファイルを連携して変更する。
- Copilot Agent Mode が生成した変更とテストをレビューしてから、コードベースにマージする。

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成しましょう。

### 追加の探索演習 — ページネーションを実装する

ゲーム一覧が増えるとページネーションが必要になります。この演習で学んだことを使い、Copilot にページネーションを追加するよう依頼します。次の点を検討してください。

- 既存の指示ファイルを使うなど、現在のベストプラクティスに従う。
- ページサイズをユーザーが選べるようにするか、値を固定するかなど、ページネーションの動作を決める。
- 望む実装に必要なガイダンスをプロンプトで Copilot に伝える。
- GitHub Copilot に変更とコンテキストを伝え、反復して調整する。これは通常の Copilot との作業方法です。

## リソース

- [Copilot の ask、edit、agent mode: それぞれの役割と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
