---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI の Playwright MCP を確認・構成し、ブラウザーでフィルター機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書くことだけが開発ではありません。データや外部サービスを使ったり、Copilot が利用できる自動化を追加したりする必要があります。MCP サーバーを使うと、Copilot CLI に組み込まれていないツールやサービスを利用できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) とは何か、Copilot CLI で使う方法を学ぶ。
- Playwright MCP サーバーがまだ利用できない場合は追加する。
- エージェントにブラウザーを操作させ、フィルター機能を試す。

## シナリオ

ユニットテストとエンドツーエンドテストは重要ですが、UI の変更を検証するには、実際に UI を操作する必要があります。Copilot がユーザーと同じように開発中の Web サイトを使えるようにし、期待どおりの変更を実現する手順をさらに自動化します。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] を使うと、AI エージェントが外部のツールやサービスと通信できます。MCP によって、AI エージェントは外部のツールやサービスとリアルタイムにやり取りし、最新情報にアクセスしたり、ユーザーに代わって操作したりできます。

ツールやリソースは、AI エージェントと外部のツールやサービスをつなぐ MCP サーバーを通じて利用します。各 MCP サーバーは、AI エージェントが利用できる異なるツールとリソースを提供します。

よく使われている MCP サーバーには、次のものがあります。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、issue、プルリクエストを管理する API を利用できます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を提供します。

他にも多くの MCP サーバーがあります。GitHub は、エコシステム内で MCP サーバーを見つけ、貢献できるように[MCP レジストリ][mcp-registry]をホストしています。

> [!CAUTION]
> MCP サーバーは他の依存関係と同様に扱ってください。利用前にソースコードを確認し、発行元を検証して、セキュリティ上の影響を検討します。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は、Copilot がユーザーと同じように Web サイトを操作できるように追加します。

> [!NOTE]
> Copilot CLI ですでに Playwright MCP を構成済みの場合は、このセットアップ手順を省略して次のセクションに進んでください。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを実行して、MCP サーバー追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力して <kbd>Tab</kbd> を押します。
4. サーバーの種類として **STDIO** を確認し、<kbd>Enter</kbd> と <kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright で機能を確認するよう Copilot に依頼する

前の手順では、機能が期待どおり動作することを手動で確認しました。次は、Copilot が利用できる Playwright MCP サーバーを使って同じ確認を行います。

1. 次のプロンプトで、Playwright MCP サーバーを使って機能を検証するよう Copilot に伝えます。

   ```plaintext
   アプリを起動し、Playwright MCP を使って issue と計画に照らしながらフィルター機能を確認してください。変更は加えずに、動作する点としない点を教えてください。確認が終わったら、起動したサーバーを停止してください。
   ```

> [!NOTE]
> Copilot は通常、自動的に MCP サーバーを選択するため、明示的に指定しなくてもかまいません。使うサーバーが分かっている場合は、指示すると結果が安定しやすく、トークンも節約できます。

2. Copilot がブラウザーで機能を確認する手順を実行する様子を見ます。
3. レポートを読み、すべて期待どおりに動作していることを確認します。

Copilot はサーバーを起動し、Playwright でサイトを操作し、作業後にサーバーを停止して結果を報告します。

## まとめと次のステップ

おめでとうございます。Copilot CLI から Playwright MCP サーバーを使って、実際のブラウザーで機能を確認できました。このレッスンでは、次のことを行いました。

- Model Context Protocol (MCP) とは何か、Copilot CLI で使う方法を学んだ。
- Playwright MCP サーバーが未設定の場合は追加した。
- エージェントにブラウザーを操作させ、フィルター機能を試した。

次は、スキルとブラウザーツールを専門家の役割にまとめる[QA カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
