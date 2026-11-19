---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使ううえで、コンテキストは重要です。特定の方法で作業する必要がある場合は、そのガイダンスを Copilot が参照できるようにします。[指示ファイル][instruction-files]を使うと、コード生成の「何を」だけでなく、「どのように」も伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示ファイルとパス指定の指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot にどう伝わるかを確認する。
- 現在の指示を使って、フィルター機能の最初のデータ部分（出版社ヘルパー）を生成する。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加する。
- 同じプロンプトを再実行し、生成されるコードに新しい標準が反映されることを確認する。
- 指示の更新とフィルター機能の一部を `main` にコミットし、次の演習で Copilot が更新されたガイダンスを使えるようにする。

> [!CAUTION]
> 生成されたコードが、設定した標準の一部に従わない場合があります。Copilot の動作は確定的ではありません。この演習の目的は、指示の更新後に動作がどのように変化するかを見ることであり、出力を一字一句一致させることではありません。

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

- `.github/copilot-instructions.md` は、リポジトリに対する**すべて**のリクエストで Copilot に渡される単一の指示ファイルです。Copilot への一般的なリクエストの大半に関係する、プロジェクトレベルの情報を記載します。技術スタック、開発対象の概要、ベストプラクティス、全体に適用するガイダンスなどが該当します。
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

## 変更用のブランチを作成する

ここでは、ベストプラクティスに従って変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

カスタム指示の効果を確かめるため、まず現在の指示ファイルを使ってプロンプトを送信します。その後、指示を更新し、同じプロンプトをもう一度送信して違いを確認します。

1. 前の演習で開いたエディターのタブを閉じ、Copilot が必要なコンテキストだけを取得するようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーを配置する場所を Copilot に示します。
3. Chat ビューのエージェント選択メニューから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント選択メニューの画面](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   src/lib/publishers.ts に、各出版社の名前と ID を返すデータアクセスヘルパーを作成または更新してください。ファイルの変更を適用してください。
   ```

5. Copilot はプロジェクトを調べてコードを更新します。ヘルパーファイルとテストに変更を加えることがあります。
6. 提案されたヘルパーが、最初の引数に `db` クライアントを受け取り、出版社の型付き配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ層の規約に基づいています。
7. 提案されたコードに TSDoc のドキュメントコメントとファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot の動作は確定的ではないため、指示がなくてもドキュメントコメントが追加される場合があります。その場合も問題ありません。指示を更新した後の一貫性の向上が、この演習の目的です。

## 新しいリポジトリ標準を追加する

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を提供するファイルです。コードの提案を改善するために、リポジトリのコーディング標準を記載します。

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

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を選択し、Copilot が対象ファイルに集中できるようにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   src/lib/publishers.ts に、各出版社の名前と ID を返すデータアクセスヘルパーを作成または更新してください。ファイルの変更を適用してください。
   ```

4. 今度は、提案されたファイルの先頭に次のようなコメントブロックがあることを確認します。

   ```typescript
   /**
    * Tailspin Toys クラウドファンディングプラットフォームの出版社データアクセスヘルパーです。
    * データベースから出版社情報を取得する関数を提供します。
    */
   ```

5. 提案された関数に次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * ID と名前を含むすべての出版社の一覧を返します。
    *
    * @param db - Drizzle データベースクライアント。
    * @returns 出版社オブジェクトの配列で解決される Promise。
    */
   ```

新しいプロジェクト標準を Copilot に伝え、次の演習で作業を続けるコードに適用できました。

## 指示とブランチをコミットしてプッシュする

指示ファイルも他のリソースと同様に、ソース管理で管理します。変更をコミットしてリポジトリにプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルウィンドウを開きます。
2. ターミナルで次のコマンドを実行し、指示の更新とヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. ターミナルで次のコマンドを実行し、指示の更新とヘルパー基盤に関する Copilot の変更ファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status に追加の変更（テストなど）が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 次のコマンドでブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント標準の issue に関連付けてプルリクエストを作成します。手動で行う代わりに、すでにプロジェクトに接続されている GitHub ツールを使って Copilot に依頼します。issue を検索して、issue をクローズする PR を作成し、マージするよう指示します。

> [!NOTE]
> Copilot が GitHub に対して操作を行うのは、この演習が初めてです。プロジェクトのテンプレートには GitHub のツールがあらかじめ設定されていますが、Copilot が初めてツールを使うときに、VS Code で GitHub へのサインインを求められることがあります。表示される手順に従ってください。後の演習で、この接続に使われる Model Context Protocol について説明します。

1. Codespace 内で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択メニューで **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関する issue を検索し、現在のブランチから PR を作成するよう Copilot に依頼します。指示の変更と、新しい出版社ヘルパー基盤の両方を説明に含めます。

   ```
   指示ファイルの更新に関する issue を探してください。現在のブランチから新しい PR を作成し、その issue をクローズすることを明記してください。また、今後のフィルター機能に向けて出版社ヘルパーの基盤も追加したことを説明に含めてください。
   ```

4. Copilot が issue の検索や操作を進めるために確認を求めたら、**Allow** を選択します。
5. PR が作成されたら、次のプロンプトで Copilot にマージを依頼し、ブランチを `main` に戻します。

   ```
   PR を main にマージしてください。その後、ローカルで main に戻り、最新のコードをプルして最新状態にしてください。
   ```

6. GitHub の操作やシェルコマンドを実行する許可を Copilot が求めたら、コマンドを確認したうえで適切な場合に **Allow** を選択します。

GitHub Copilot の支援でプルリクエストを作成し、マージできました。

## まとめと次のステップ

このプロジェクトの指示ファイルが Copilot にコンテキストを伝える仕組みを確認し、VS Code の Copilot Chat を使って次のことを行いました。

- 既存の指示を使ってコード生成プロンプトを送り、Copilot の出力を確認した。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加した。
- 同じプロンプトを再実行し、提案されたコードに新しい標準が反映されることを確認した。
- 指示の更新とヘルパー基盤を `main` にコミットし、次の演習で利用できるようにした。

次は、指示を [agent mode][next-lesson]で活用し、Copilot にコードベース全体で新機能を追加させます。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示をより良く書くための 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
