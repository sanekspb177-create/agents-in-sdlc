---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使って、Tailspin Toys の一連のワークフローを進めました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトを確認し、用意されたフィルター issue を見つけた。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージした。
- フィルター issue を起点に、Plan モードでアプローチを決め、Autopilot モードで実装し、Interactive モードでレビューした。
- カスタム指示でエージェントをガイドし、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行した。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を確認した。
- quality assurance (QA) カスタムエージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーでの検証を確認した。
- フィルター機能全体をレビューし、フィルター PR で Agent Merge を許可した。
- コンテキスト、モデル、共有、省略可能な cloud agent への委任に使うスラッシュコマンドを確認した。

## 提出した内容

ワークショップでは、更新済みの `main` からそれぞれブランチを作成し、2 つの PR を作ります。

1. **星評価**: 既存の `starRating` と未評価状態の表示をゲームカードに追加する。
2. **フィルターと品質ワークフロー**: フィルター機能を実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルと関連テストを追加する。

フィルター機能の計画から PR の作成まで、同じ会話とブランチを使いました。ワークショップを簡潔に進めるため、この作業を 1 つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分でのブラウザー確認、MCP を通じた Copilot のブラウザー操作など、いくつかの方法でコードを確認しました。`quality-checks` スキルはユニットテスト、lint、型チェックを実行し、カスタマイズした形式で結果を報告しました。QA はこれらの結果と要件、テストカバレッジのレビューをまとめ、PR の前に確認しました。

追加するテストは実際の不足を補うものである必要があります。QA の実行時に新しいテストが不要と判断されることもあります。コードと根拠を確認してからマージを許可してください。変更があった場合は、影響を受ける検証結果も更新します。

## ベストプラクティス

Copilot に与えるコンテキストとツールが作業に影響します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを構成しました。これらのカスタマイズは会話をまたいで再利用し、チームのニーズに合わせて調整できます。指示は標準を設定し、スキルは繰り返し実行できるタスクをまとめ、カスタムエージェントは専門家の役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて**モードとモデル**を選びます。構築の前にアプローチを検討するには **Plan**、範囲を絞った変更を対話しながら進めるには **Interactive**、範囲が明確なタスクを自律的に進めるには **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業には高性能なモデルを選びます。

インフラストラクチャだけでなく、コンテキストも重要です。何を作るか、なぜ必要か、どのように作るかを明確に伝えることで、出力が大きく変わります。

## さらに確認する

主なワークフローを学びました。Copilot CLI の追加機能も確認してみましょう。

- `/review` でコードレビューエージェントに変更を分析させる。
- `/rubber-duck` で問題を整理し、別の視点を得る。
- `/fleet` で独立したサブタスクを並行して調整する。
- `/worktree` で別のタスクを分離する。
- `/delegate` で Copilot cloud agent にタスクを委任する。

## 次のステップ

どのツールも、使い続けることで上達します。製品コード、趣味のコード、長い間作りたかった小さなアプリなどで活用してください。チームで学んだことを共有し、互いの経験から学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys をさらに拡張したい場合は、省略可能な [Foundry Backer Concierge シリーズ][foundry]に進んでください。他の環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
