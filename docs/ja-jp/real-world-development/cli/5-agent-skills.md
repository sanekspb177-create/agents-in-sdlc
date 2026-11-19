---
title: "レッスン 5 - quality-checks スキルのカスタマイズと使用"
description: "既存の quality-checks スキルを確認し、レポート形式をカスタマイズして、フィルター機能を検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くことだけが開発ではありません。手動でコードの動作を確認し、指示ファイルでコーディング標準に従うようにしました。では、テストや lint、その他の継続的インテグレーション (CI) の作業はどうでしょうか。

このようなタスクには、**エージェントスキル**が適しています。スキルを使うと、こうした操作を正しい方法で実行できるよう Copilot を支援できます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` スキルを確認する。
- 結果の形式をカスタマイズする。
- スキルを再読み込みして実行する。

## シナリオ

Tailspin Toys では、ユニットテスト、lint、型チェックに `quality-checks` スキルを使っています。チームは、結果を読みやすくするため、レポートを改善したいと考えています。

## 指示、スクリプト、リソース

エージェントスキルは、エージェントが必要なときに読み込む、再利用可能なタスクの指示、実行可能なスクリプト、補助リソースをまとめたものです。基本的には、スキル名のフォルダーと、その中にある `SKILL.md` で構成されます。Markdown ファイルには、スキルの目的を定義する `name` と `description` のフロントマター、機能の概要、呼び出すべき状況のガイダンスを記載します。スキルが呼び出されたときに使用するスクリプトなどのリソースをサブフォルダーに含めることもできます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルはプロジェクトにあるコマンドを使うため、`SKILL.md` だけで構成されています。

スキルは、チームで共有・再利用するリポジトリ資産としてプロジェクトの `.github/skills` に配置するか、ユーザー用の `~/.copilot/skills` に配置できます。

## スキルを確認する

Tailspin Toys のチームがユニットテスト、lint、型チェックを実行するために作成した `quality-checks` スキルを確認しましょう。

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭にある `name` と `description` を読みます。説明は Copilot がスキルをいつ呼び出すかを判断するのに役立ちます。
3. 指示を読み、テストと lint の手順を Copilot にどう案内するか確認します。
4. スキルに **Results output formatting** セクションがまだないことを確認します。

## 変更を加える前にスキルを実行する

Copilot CLI から直接、または自然言語を使ってスキルを呼び出せます。Copilot に 3 種類のチェックを実行するよう依頼します。

1. Interactive モードでフィルター機能の会話に戻ります。
2. 次のプロンプトを送信します。

   ```plaintext
   quality-checks スキルを実行して、ユニットテスト、lint、型チェックを行ってください。
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

何を実行したか、成功したか、ツールが実際に何を報告したかが分かるレポートに改善しましょう。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの最後に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

変更を加えたので、スキルを実際に使ってみましょう。Copilot CLI は、会話を再起動せずに編集済みのスキルを再読み込みできます。

1. 次のコマンドを入力します。

   ```plaintext
   /skills reload
   ```

2. 先ほどと同じプロンプトを送信します。

   ```plaintext
   quality-checks スキルを実行して、ユニットテスト、lint、型チェックを行ってください。
   ```

3. 最後に表示されるレポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

既存のエージェントスキルをカスタマイズして使いました。このレッスンでは、次のことを行いました。

- ユニットテスト、lint、型チェックを行う `quality-checks` スキルを確認した。
- 結果の形式をカスタマイズした。
- スキルを再読み込みして実行した。

この変更は、機能 PR でフィルター機能と一緒に扱います。次は、[Playwright MCP サーバー][next-lesson]を使って Copilot がサイトを直接操作できるようにします。

## スキルの参考例

次のコミュニティの例は参考資料であり、追加のタスクではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューションのワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とエクスポートスクリプト: `drawio`][drawio-example]
- [ブラウザーテスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
