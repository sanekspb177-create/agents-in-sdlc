---
title: "レッスン 8 - 機能 PR の作成とマージ"
description: "フィルター機能、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルター機能の実装、指示の更新、スキルの更新、quality assurance (QA) プロファイル、テストはすべて 1 つのブランチに保存されています。変更をまとめてレビューし、プルリクエストを作成します。星評価のプルリクエスト (PR) は手動でマージしました。今回は **Agent Merge** にマージの作業を任せます。

> [!NOTE]
> 通常は、機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。ワークショップを簡略化するため、フィルター機能と品質ワークフロー全体を 1 つの会話とブランチにまとめ、この PR に含めました。

このレッスンでは、次のことを行います。

- Agent Merge と、マージのライフサイクルを自動化する方法を学ぶ。
- 機能の変更全体と検証の証拠を確認する。
- フィルター機能の PR を作成する。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認する。

## シナリオ

フィルター機能のワークフローを通じて、Copilot で機能の計画、実装、検証を行いました。Tailspin Toys は、マージの許可を開発者が管理しながら、PR の残りの作業を自動化したいと考えています。

## Agent Merge について

**Agent Merge** は、プルリクエストのマージに必要な残りの作業を自動化します。有効にすると、Copilot は PR をブロックしている項目（失敗した継続的インテグレーション (CI) チェックの修正、レビューコメントへの対応、必要な場合のリベースなど）に取り組みます。その後、リポジトリで許可されていれば、GitHub の自動マージを有効にします。

これまでは、自分で **Merge pull request** を選択しました。今度は Agent Merge にその作業を任せます。Agent Merge を有効にする前に、変更をレビューして問題がないことを確認してください。

## Agent Merge を使って PR を管理する

コードの作成が完了したら、変更をレビューして PR を作成し、残りの処理を Agent Merge に任せましょう。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントのダイアログを開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドを実行して PR を作成します。
5. `/pr agentmerge` を実行して Agent Merge を有効にします。
6. Copilot は PR の継続的インテグレーションを監視します。すべてのチェックが成功すると、マージを実行します。
7. Copilot から `PR #14 was squash-merged successfully.` のようなメッセージが届いたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリの設定、権限の不足を回避しません。ブロックされた場合は報告された理由を確認し、リポジトリで許可されている場合はレビュー済みの PR を手動でマージしてください。

## まとめと次のステップ

コードの生成とテスト・検証に続き、プルリクエストの作業も自動化しました。このレッスンでは、次のことを行いました。

- Agent Merge と、マージのライフサイクルを自動化する方法を学んだ。
- 機能の変更全体と検証の証拠を確認した。
- フィルター機能の PR を作成した。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認した。

次は、コンテキスト、モデル、共有、省略可能なクラウドへの委任に使う[便利な Copilot CLI のスラッシュコマンド][next-lesson]を確認します。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
