---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使ううえで、コンテキストは重要です。特定の方法で作業する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストにアクセスできるようにします。[指示ファイル][instruction-files]を使うと、Copilot に「何を」してほしいかだけでなく、「どのように」行うかも伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示ファイルとパス指定の指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot にどう伝わるかを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code と CLI のハーネスとは異なり、この演習では変更前後のプロンプトを実行しません。Copilot cloud agent は GitHub issue に対して非同期に作業するため、影響をリアルタイムで並べて示すのが難しいためです。このハーネスの後半で、cloud agent が作成したプルリクエストをレビューするときに、指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発の進め方に関するガイドラインと要件があります。たとえば、次のとおりです。

- データ層には必ずユニットテストを用意します。
- UI はダークモードにし、現代的な見た目にします。
- コードには TSDoc のドキュメントコメントを追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使い、これらの開発規則に沿って Copilot が作業できるようにします。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件を Copilot に伝え、より適切な提案やコードスニペットを得られます。希望するコーディング規約、ライブラリ、コードに含めるコメントの種類などを指定できます。リポジトリ全体、または特定のタスクやファイル形式に対する指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべて**のリクエストで Copilot に渡される単一の指示ファイルです。Copilot への一般的なチャットや CLI リクエストの大半に関係する、プロジェクトレベルの情報を記載します。技術スタック、開発対象の概要、ベストプラクティス、全体に適用するガイダンスなどが該当します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル形式に対する指示ファイルです。TypeScript や Astro などの言語、UI コンポーネントの作成、ユニットテストの追加といったタスク向けのガイドラインを記載できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使用され、コード補完や次の編集候補には使用されません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示ファイルと `applyTo` フロントマターを持つ `*.instructions.md` ファイルの両方を使ってコードを生成します。
>
> また Copilot は、AGENTS.md や CLAUDE.md など、他の標準形式の指示ファイルにも対応しています。[対応する指示ファイルの形式][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成に関する詳細はこのワークショップの範囲外ですが、サンプルプロジェクトには代表的な構成例があります。基本的には、次の点を意識してください。

- `copilot-instructions.md` には、開発対象の説明、プロジェクト構成、全体に適用するコーディング標準など、プロジェクトレベルのガイダンスを記載します。
- `*.instructions.md` には、ユニットテスト、Astro コンポーネント、データ層など、特定のファイル形式やタスク向けの指示を記載します。
- 自然な言葉で、明確なガイダンスとコードの良い例・悪い例を示します。

指示ファイルに唯一の正解はありません。AI の使い方と同様に、プロジェクトで試しながら最適な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な指示ファイルを用意することをお勧めします。このプロジェクトには、[UI の変更][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスク向けの指示ファイルがあります。
>
> Copilot に指示ファイルを作成させることもできます。VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` など、利用する環境によって方法が異なります。該当する機能は、それぞれのレッスンで説明します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタムエージェント、その他のリソースをまとめたリポジトリ [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを確認します。共通の `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明や **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には **GitHub Actions Workflows** のガイダンスもあります。これらは、Copilot とやり取りするときに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、指示を適用するファイルをリポジトリルートからの glob で指定しています。たとえば `**/*.test.ts` に一致する TypeScript のテストファイルに適用されます。
5. このプロジェクトでユニットテストを作成するための指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。他の指示ファイル（`unit-tests.instructions.md` など）やプロジェクト内の既存ファイルへのリンクがあります。大きな指示を再利用可能な小さなファイルに分け、Copilot が参照できる例を示す方法を確認してください。ここに記載されたパスはリポジトリルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションではプロジェクトのコーディング標準を説明していますが、コード内のドキュメント化はまだ要件になっていません。次の手順では、TSDoc コメントとファイルヘッダーコメントの規則を追加します。

## 新しいリポジトリ標準を追加する

次に行う編集は 1 か所だけです。`.github/copilot-instructions.md` にプロジェクト全体の規則を追加し、コード内のドキュメントを TSDoc コメントとファイルヘッダーコメントで記述するよう Copilot に伝えます。cloud agent は、このハーネスの後半で issue に取り組むときにこの規則を参照します。

ブランチを作成してから編集します。cloud agent は issue の対象ブランチにある指示を読み取りますが、変更を適切にコミットするためにもブランチを使います。

1. 前の演習で使った Codespace に戻り、ターミナルで次のコマンドを実行して新しいブランチを作成し、切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前の説明のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を提供するファイルです。コーディング標準を追加して、コードの提案を改善しましょう。

1. `.github/copilot-instructions.md` を再度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準は記載されていますが、コード内のドキュメント化はまだ要件ではありません。そのため、生成されたヘルパーにドキュメントコメントがありませんでした。
3. 既存の標準の直後に、次の Markdown 行を追加します。ファイルヘッダーコメントと TSDoc コメントを追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで説明したように、リポジトリ全体への指示は `.github/copilot-instructions.md` に、言語やファイル形式、特定のタスクへの指示は `*.instructions.md` ファイルに記載できます。TSDoc コメントの規則など、プロジェクト全体の標準にはリポジトリレベルのファイルが適しています。

## 指示の変更をコミット、プッシュ、マージする

Copilot cloud agent は、issue の対象ブランチから指示ファイルを読み取ります。次の演習で issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。そのため、cloud agent が指示を利用できるように、変更を `main` に取り込む必要があります。

1. ステージングしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成し、マージします。次の演習で issue を割り当てる際、cloud agent はこの指示を読み取ります。

> [!TIP]
> このワークショップで `main` に直接作業したい場合は、ブランチ作成を省略して `main` にコミットできます。ブランチを使う手順は、実際のプロジェクトでの作業方法を再現するために含まれています。

## まとめと次のステップ

このプロジェクトの指示ファイルが Copilot にコンテキストを伝える仕組みを確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。このハーネスの後半では、cloud agent が作成するプルリクエストでこの標準を確認します。

次に、[最初の issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示をより良く書くための 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
