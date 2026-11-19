---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

agent mode を使ってフィルター機能を作成しました。プルリクエストを開く前に、ブラウザーで実際に動作することを確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーで機能をテストさせます。その後、ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) とは何か、MCP サーバーが Copilot にツールを追加する仕組みを学ぶ。
- Playwright MCP サーバーをワークスペースに追加する。
- Copilot にブラウザーでフィルター機能を手動テストさせる。
- ブランチを公開して、フィルター機能のプルリクエストを作成する。

## Model Context Protocol (MCP) とは

agent mode は、エディターの外にあるツールを利用できると、さらに便利になります。Copilot が外部ツールを使う仕組みが Model Context Protocol (MCP) です。エージェントが外部のツールやサービスと通信するための標準です。

![agent mode の内部構成とコンテキスト、LLM、MCP サーバーおよび VS Code 拡張機能が提供するツールとのやり取りを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントは外部のツールやサービスと通信できます。MCP を利用すると、リソースを使って最新情報にアクセスしたり、ツールを使ってユーザーに代わって操作したりできます。

ツールやリソースは、AI エージェントと外部のツールやサービスをつなぐ MCP サーバーを通じて利用します。MCP サーバーは、既存の API や NPM パッケージなどのローカルツールとの通信を管理します。各 MCP サーバーは、AI エージェントが利用できる異なるツールとリソースを提供します。

よく使われている MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリ、issue、プルリクエストを管理する API を利用できます。リポジトリの作成や更新、issue やプルリクエストの管理などを AI エージェントに依頼できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。Web ページへの移動、フォームへの入力、ボタンの選択などを AI エージェントに依頼できます。

他にも、さまざまなツールやリソースを利用できる MCP サーバーがあります。GitHub は、エコシステム内で MCP サーバーを見つけ、貢献できるように[MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーは他の依存関係と同様に扱ってください。利用前にソースコードをよく確認し、発行元を検証して、セキュリティ上の影響を検討します。信頼できる MCP サーバーだけを使い、機密リソースや操作へのアクセスを許可する際は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースから利用できる MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. 次のように、`github` サーバーがあらかじめ設定されていることを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` エントリはプロジェクトテンプレートに含まれています。そのため、前の演習では Copilot から issue のバックログを確認できました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使っているため、ローカルにインストールする必要はありません。VS Code は HTTP で接続し、Copilot が初めてツールを使うときに GitHub 認証を行います。

## Playwright MCP サーバーを追加する

次に、2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと Copilot が操作できるブラウザーを用意できるため、機能のテストに適しています。

1. `.vscode/mcp.json` で `github` の隣に `playwright` エントリを追加し、次のようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` は**ローカル**サーバーです。VS Code は `npx @playwright/mcp@latest` を実行してサーバーを起動します。`--headless` フラグを指定すると、ブラウザーを画面に表示せずに実行できます。Codespace には表示用デスクトップがないため、この設定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトでは、エンドツーエンドテストに Playwright をすでに使用しているため、通常はブラウザーがインストールされています。Copilot が後からブラウザーがないと報告した場合は、`npx playwright install chromium` を実行して再試行するよう依頼してください。

## サーバーを起動して信頼する

VS Code は、必要に応じて MCP サーバーを起動します。Copilot がサーバーのツールを初めて必要としたときに、VS Code がサーバーを起動し、信頼するかどうかを確認します。

1. `.vscode/mcp.json` の `playwright` エントリの上に **Start** 操作が表示されます。選択してサーバーを起動し、構成が正しいことを確認します。
2. VS Code からサーバーを信頼するか確認されたら、構成を確認して信頼することを選択し、サーバーを起動します。

手動での起動は省略できます。その場合は、次の手順で Copilot がツールを初めて使うときに VS Code がサーバーを起動し、信頼するか確認します。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。ユーザーがアプリを操作して確認する代わりに、エージェントがページを開き、移動し、フィルターを適用して、その結果を読み取って報告します。会話を続けながら機能を素早く確認できます。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティツリー][playwright-mcp-server]を利用します。エージェントは、ボタン、リンク、リスト項目などのラベル付き構造要素を使って判断します。支援技術と同じように情報を扱うため、簡単な機能確認をアクセシビリティの初歩的なチェックとしても活用できます。

サーバーを接続し、アプリを起動したら、次のように依頼して、作成したフィルター機能を試します。

```text
Playwright MCP サーバーを使って、実行中の http://localhost:4321 にブラウザーでアクセスし、新しいゲームフィルター機能を確認してください。

1. ゲームページに移動し、一覧に表示されているゲームの数を確認します。
2. カテゴリフィルターを適用し、そのカテゴリのゲームだけが表示されることを確認します。
3. フィルターを解除し、出版社フィルターを適用して、その出版社のゲームだけが表示されることを確認します。
4. カテゴリと出版社のフィルターを組み合わせ、両方の条件が結果に反映されることを確認します。

各手順で確認したことを報告し、期待どおりに動作しない点があれば指摘してください。
```

Copilot は Playwright MCP サーバーを通じてブラウザーを起動し、各手順を実行して結果を報告します。issue の受け入れ基準と照らして報告を確認してください。問題がある場合は追加で質問するか、コードを修正させてからプルリクエストを作成します。

> [!NOTE]
> テストするには、アプリが `http://localhost:4321` で実行されている必要があります。開発サーバーを停止した場合は、プロンプトを送信する前に再起動してください。Copilot が Playwright MCP サーバーを初めて使うときに、ブラウザーをダウンロードする場合があります。ブラウザーがないと報告されたら、`npx playwright install chromium` を実行して再試行するよう依頼します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認できたら、チームでレビューするためにプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミットメッセージを生成します。

   ![変更内容を表示する Source Control パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンドラインインターフェイス (CLI) を使うなど、プルリクエストを作成する方法はいくつかあります。GitHub Copilot を使っているので、PR 作成も依頼しましょう。Copilot は関連 issue を検索し、issue に関連付けて PR を作成できます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. GitHub ツールを使えるように、エージェント選択メニューで **Agent** が選択されていることを確認します。
3. 次のように依頼して、PR を作成します。

   ```plaintext
   リポジトリでカテゴリと出版社によるフィルターに関する issue を探してください。現在のブランチから新しいプルリクエストを作成し、正しい issue に関連付けてください。
   ```

4. Copilot が情報を集めて操作を進めるために確認を求めたら、必要に応じて **Continue** を選択します。GitHub ツールを初めて使うときは、GitHub への認証を求められることがあります。表示される手順に従ってください。
5. Copilot が issue を検索して該当するものを見つけ、PR を作成する様子を確認します。
6. Copilot が生成したリンクを選択して PR をレビューします。ただし、まだ**マージしないでください**。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) とは何か、MCP サーバーが Copilot にツールを追加する仕組みを学んだ。
- Playwright MCP サーバーをワークスペースに追加した。
- Playwright MCP サーバーを使い、機能を公開する前にブラウザー上でテストした。
- ブランチを公開して、フィルター機能のプルリクエストを作成した。

MCP サーバーを使って機能を「テスト」しましたが、MCP は Copilot を拡張する方法の 1 つです。次は[カスタムエージェント][next-lesson]を作成して、アクセシビリティレビューなど、対象を絞ったタスクを効率化しましょう。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
