---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここまでの演習では、ユーザー体験の改善と機能追加を目的に、GitHub Copilot cloud agent に 3 つのタスクを依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクは、GitHub.com の[エージェントページ][agents-page]など、さまざまなツールで管理できます。このミッションコントロールでは、オープンなプルリクエスト (PR) があるすべてのエージェントタスクを確認できます。実行内容を調べ、進行中のセッションを誘導することもできます。

このレッスンでは、次のことを行います。

- エージェントページで cloud agent のタスクを監視する。
- 実行中のセッションを誘導して、機能を追加するよう依頼する。

## シナリオ

エージェントにハイコントラストモードの作成を依頼した後、チームはライトモードも同時に追加したいと考えました。サイトのスタイルと切り替え機能を更新しているところなので、ハイコントラストに加えてライトモードも実装するようエージェントを誘導します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状況を確認します。

1. [Copilot エージェントページ](https://github.com/copilot/agents)を開きます。
2. メインペインと左ペインの両方にタスクの一覧が表示されます。Copilot に割り当てたタスクとして、次の項目があることを確認します。
    - コードベースのドキュメントを更新する。
    - ゲームを変更する API を生成する。
    - Web サイトにハイコントラストモードを追加する。
3. 実行中のタスクを 1 つ選択し、Copilot が実行した作業を確認します。たとえば、次の手順が含まれます。
    - リポジトリからコードをチェックアウトする。
    - Copilot の作業環境を作成する。
    - MCP サーバーをセットアップする。
    - 割り当てられたタスクを完了するためのさまざまな作業を行う。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や採用した方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。PR と変更されたファイルが表示されるため、さらに詳しく監視できます。

## Cloud agent を誘導する

実行中のタスクを確認できたので、ハイコントラストモードに取り組んでいる Copilot に、ライトモードの切り替えも追加するよう依頼します。

1. ハイコントラストモードを追加するセッションを選択します。正確なタイトルは Copilot が付けた名前や作業状況によって異なります。

    ![ミッションコントロールのアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まったことを示すメッセージが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot が作業中のエージェントページで、Steer active session while copilot is working ダイアログが強調表示された画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に反映したことを確認します。

## Copilot に作業を任せる

前回と同じように、Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で実行中の手順が終わると、新しい依頼が作業に組み込まれます。

完了まで数分かかるため、これまでに学んだことを振り返りましょう。

## まとめと次のステップ

このレッスンでは、Copilot cloud agent のタスクを監視して誘導するための中心的な場所である Copilot エージェントページを確認しました。ミッションコントロールで、実行中・完了済みのタスクを追跡し、作業内容をレビューして、進行中のタスクに追加の指示を出せます。

次の概念を学びました。

- ミッションコントロールとエージェントページで cloud agent のタスクを監視する。
- 実行中のセッションを誘導して、機能の追加を依頼する。

Copilot がアクセシビリティ機能を実装している間に、[Copilot が作成したプルリクエストを改善します][next-lesson]。

## リソース

- [Copilot エージェントページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
