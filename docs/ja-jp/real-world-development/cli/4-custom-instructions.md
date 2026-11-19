---
title: "レッスン 4 - カスタム指示で Copilot をガイドする"
description: "リポジトリの指示を確認し、ドキュメントの標準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使ううえで、コンテキストは重要です。特定の方法で作業する必要がある場合は、そのガイダンスを Copilot が参照できるようにします。[指示ファイル][instruction-files]では、コードの「何を」だけでなく、「どのように」作成するかも指定できます。フィルター機能を構築したので、Copilot が参照した指示を確認し、ドキュメントの標準を追加してコードに適用します。

このレッスンでは、次のことを行います。

- リポジトリレベルの指示とパス指定の指示がエージェントに伝わる仕組みを確認する。
- コーディング標準が守られるように指示ファイルを更新する。
- 指示ファイルがコードに与える影響を確認する。

## シナリオ

Tailspin Toys には、開発の進め方に関するガイドラインと要件があります。たとえば、次のとおりです。

- コメントでは、コードをそのまま言い換えるのではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` にあるエクスポートされた関数では、`db` の注入可能な引数がある場合はそれも含め、目的、パラメーター、戻り値を TSDoc/JSDoc で説明します。
- 再利用可能な Astro コンポーネントでは `Props` コントラクトを説明し、関連するコードの変更時にコメントを最新の状態に保ちます。
- 既存のフォーマットと lint のガイダンスを維持します。

指示ファイルを使い、これらの開発規則に沿って Copilot が作業できるようにします。

## 指示ファイル

カスタム指示を使うと、コーディングスタイルや要件を Copilot に伝え、より適切な提案やコードスニペットを得られます。希望するコーディング規約、ライブラリ、コードに含めるコメントの種類などを指定できます。リポジトリ全体、または特定のタスクやファイル形式に対する指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべて**のリクエストで Copilot に渡される単一の指示ファイルです。大半のリクエストに関係するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル形式、タスク向けのガイドラインを記載します。

> [!NOTE]
> 指示ファイルの形式と対応状況は、環境によって異なります。特定の形式を利用する前に、[カスタム指示の対応状況][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スターター プロジェクトには、あらかじめ指示ファイルが含まれています。変更を加える前に確認し、効果を比べられるようにします。

1. Codespace に戻ります。
2. Codespaces エディターで（ターミナルではなく）`.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディングのガイダンスを確認します。この指示は、リポジトリ内で Copilot とやり取りするときに適用されます。
4. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどに関する指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、指示を適用するファイルを glob で指定しています。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルや既存プロジェクトファイルへのリンクを確認します。これらを使うと、指示を小さく再利用可能な単位に分け、Copilot が参照できる例を示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存のファイルは出発点として適切ですが、まだ不足があります。共通の `copilot-instructions.md` を変更し、新しく生成される TypeScript に TSDoc コメントを追加するよう指示します。

1. `.github/copilot-instructions.md` で **Code formatting guidance** というセクションを探します。35 行目付近にあります。
2. 次の行を最後の箇条書きとして追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新されたガイダンスを使う

Copilot CLI は、会話の開始時にリポジトリの指示を読み込みます。指示を保存したら、機能のコンテキストを保ったまま Copilot CLI を終了して、フィルター機能の会話を再開します。

1. フィルター機能の会話を開いている場合は、`/exit` を入力して閉じます。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチのリポジトリルートから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示された場合は、フィルター機能の会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 更新された指示を適用するよう Copilot に依頼します。

   ```plaintext
   指示とコードのガイダンスを更新しました。生成されたコードをガイダンスに従うよう更新してください。
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、その説明がコードに沿っていることを確認します。

## まとめと次のステップ

このプロジェクトの指示ファイルを Copilot CLI が読み取る仕組みを確認し、新しい標準を機能に適用しました。このレッスンでは、次のことを行いました。

- リポジトリレベルの指示とパス指定の指示がエージェントに伝わる仕組みを確認した。
- コーディング標準が守られるように指示ファイルを更新した。
- 指示ファイルがコードに与える影響を確認した。

次は、再利用可能な `quality-checks` スキルを[カスタマイズして実行し][next-lesson]、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI のカスタム指示を追加する][instruction-files]
- [カスタム指示の対応状況][custom-instructions-support]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
