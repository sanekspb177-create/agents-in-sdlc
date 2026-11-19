---
title: "演習 5: GitHub Copilot の作業を改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験の向上と機能追加を目的に、いくつかのタスクで GitHub Copilot と作業してきました。コードへのドキュメント追加、デザインチームが改善できる関連ゲーム機能の作成、ハイコントラストとライトモードを含むアクセシビリティ機能の実装を依頼しました。コードの変更内容を確認し、必要に応じて Copilot にフィードバックを送って改善しましょう。

### シナリオ

これまでに何度も説明したように、生成 AI を導入しても、ソフトウェア設計や DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。チーム全体にレビューを依頼する前に、GitHub Copilot が作成したドキュメント、関連ゲーム機能、アクセシビリティ機能を確認しましょう。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期に、監督を受けずに作業するため、安全性を保つためのセキュリティ制約があります。

- Copilot はリポジトリへの読み取りアクセス権を持ち、書き込みは作業に使うブランチ**のみ**に制限されます。
- Cloud agent は GitHub Actions 内で実行され、作業用の独立した一時環境を作成します。
- GitHub Actions ワークフローを実行するには、人間による承認が必要です。
- MCP サーバーを含む[外部リソースへのアクセスは、既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が作成した最初のプルリクエスト (PR)、つまりコードへのドキュメント追加を確認します。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認するとき、GitHub Copilot がファイアウォールでブロックされているという警告が表示される場合があります。Copilot は、外部 MCP サーバーへの呼び出しなどを含む外部リソースへのアクセスが既定で制限されているため、これは**想定される動作**です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、プルリクエストの一覧を開きます。
3. **Add missing documentation** など、コードのドキュメント追加を示すタイトルの PR を開きます。

> [!NOTE]
> Copilot が作業中の場合、PR のタイトルに **[WIP]** が付いています。その場合は、Copilot が作業を完了するまで待ちます。数分かかることがあるため、休憩したり、これまでに学んだことを振り返ったりしてください。

4. PR の準備ができたら **Files changed** タブを選択し、変更を確認します。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく追加された TSDoc のドキュメントコメントなど、更新されたコードを確認します。変更内容は状況によって異なります。

    変更を確認するときは、TSDoc のドキュメントコメント、TypeScript の規約、コメントヘッダーに注目してください。これらは演習 2 の最初に確認したカスタム指示ファイルに基づいています。

6. 更新内容に問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示があるか確認します。
8. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
9. プルリクエストのチェック欄にワークフローがキューに登録されたことが表示されます。問題がなければ、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot と一緒に作業する方法は、レビューだけではありません。チームメンバーと同じように、PR やコードへのインラインコメントで Copilot にメンションできます。Copilot はコメントを確認し、対応するための新しいセッションを開始します。結果は確定的ではないため、必ずしも同じ内容になるような具体的な文章は提示できません。たとえば、次のような変更を依頼できます。

- 各コードファイルの先頭に、ファイルの役割を簡潔に説明するコメントヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc のドキュメントコメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. 前述の案や、コードベースに追加したいドキュメントに関する別の案を使って、変更を依頼するコメントを追加します。通常のユーザーと同じように **@copilot** をメンションします。
2. **View Session** を選択し、Copilot が作業する様子を確認します。Copilot は更新のために新しいセッションを開始します。
3. **Back to pull request** を選択すると、プルリクエストに戻れます。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了すると、PR に新しいコミットが表示されます。
5. **Files changed** タブを選択して、変更をレビューします。

満足するまで改善を繰り返してください。問題がなければ、ドラフト PR をレビュー可能な状態に変更し、`main` ブランチにマージできます。

![PR をレビュー可能な状態に変更する画面](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

次に、ゲーム詳細ページに関連ゲームを表示する issue に対して Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** など、関連ゲームの表示を示すタイトルの PR を選択します。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 更新内容に問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローの承認待ちを示す表示があるか確認します。
7. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
8. プルリクエストのチェック欄にワークフローがキューに登録されたことが表示されます。問題がなければ、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
9. **省略可能:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します（`<branch-name>` は Copilot が作成したブランチ名、たとえば **copilot/fix-8** に置き換えます）。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。前の手順と同じように、Copilot cloud agent にコメントして更新を依頼できます。たとえば、表示する関連ゲームの数を調整したり、コメントヘッダーや TSDoc のドキュメントコメントを追加したりできます（この issue は、カスタム指示を更新する**前**に割り当てたことを思い出してください）。前と同じように、**Conversation** タブのコメントから依頼を追加すると、Copilot が新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用カスタムエージェントで実装した機能を確認します。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 のミッションコントロールで追加を依頼したライトモードの両方が含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** など、ハイコントラストモードの追加を示すタイトルの PR を選択します。

> [!NOTE]
> Copilot が作業中の場合、PR のタイトルに **[WIP]** が付いています。その場合は、Copilot が作業を完了するまで待ちます。数分かかることがあります。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 実装を確認するときは、特に次の点に注目してください。
   - モード切り替え用の UI コンポーネント
   - ユーザー設定を保存するためのローカルストレージの使用
   - ハイコントラストモードとライトモードに関する CSS またはスタイルの変更
   - アクセシビリティ属性（ARIA ラベル、キーボード操作など）
   - モード切り替えを管理する JavaScript/TypeScript コード

6. 更新内容に問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示があるか確認します。
8. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
9. プルリクエストのチェック欄にワークフローがキューに登録されたことが表示されます。問題がなければ、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
10. **省略可能:** Codespace でこのブランチに切り替えて、アクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します（`<branch-name>` は Copilot が作成したブランチ名に置き換えます）。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動してブラウザーでハイコントラストモードとライトモードを切り替え、ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティ用カスタムエージェントが、アクセシビリティのベストプラクティスに従って Copilot の実装を支援したことを確認してください。アクセシビリティに関する懸念や改善点があれば、前の PR と同様にコメントで **@copilot** をメンションし、修正を依頼できます。

## 省略可能な演習 — 作業の委任を続ける

Cloud agent は、実際のバックログ項目を渡して別の作業に集中するときに効果を発揮します。習慣として身に付けるために、リポジトリに issue をいくつか追加し、Copilot に割り当ててみてください。たとえば、次のような作業があります。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent のハーネスを完了しました。各レッスンでは、次のことを行いました。

- **このリポジトリに含まれるカスタム指示ファイルを確認し**、cloud agent の出力に影響することを学びました。
- **issue を Copilot cloud agent に割り当て**、環境のセットアップ、計画、非同期の実行を確認しました。
- アクセシビリティのための**カスタムエージェントを作成して利用し**、ハイコントラストとライトモードの切り替えを追加しました。
- Copilot エージェントページをミッションコントロールとして利用し、進行中のアクセシビリティセッションを監視して誘導しました。
- **cloud agent のプルリクエストをレビューして改善し**、変更を依頼するために `@copilot` をメンションし、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent のハーネスを完了しました。さらに学びたい場合は、ここで取り組んだ内容を補完する次のハーネスを確認してください。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE で Copilot Chat の agent mode と MCP 統合を試します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan モードと Autopilot モード、エージェントスキル、QA カスタムエージェント、Playwright MCP、Agent Merge、cloud agent に戻るための省略可能な `/delegate` ブリッジを使い、ターミナルからレビュー済みの変更を 2 つ届けます。

自分のリポジトリでは、次の案を試してみてください。

- リファクタリングやテストカバレッジの issue を cloud agent に割り当てる。
- 別の分野向けのカスタムエージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
