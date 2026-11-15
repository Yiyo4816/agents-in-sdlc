---
title: "演習 5: GitHub Copilot の作業を繰り返し改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、GitHub Copilot と一緒にユーザー体験の向上や機能追加に取り組みました。コードにドキュメントを追加し、デザインチームが試せる関連ゲーム機能を作成し、ハイコントラストとライトモードの切り替えなど、アクセシビリティ機能を実装しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送って改善しましょう。

### シナリオ

生成 AI を加えても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスを進める必要があります。チーム全体にレビューを依頼する前に、Copilot が作成したドキュメント、関連ゲーム機能、アクセシビリティ機能を確認しましょう。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は非同期かつ人の監視なしで作業するため、安全性を保つための制約があります。

- Copilot はリポジトリへの読み取りアクセスのみを持ち、書き込みできるのは作業に使うブランチ**のみ**です。
- cloud agent は GitHub Actions 内に作成された、分離された一時的な環境で実行されます。
- GitHub Actions のワークフローを実行するには、人による承認が必要です。
- [外部リソースへのアクセスは、既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。MCP サーバーへのアクセスも制限されます。

## 生成されたドキュメントをレビューする

GitHub Copilot cloud agent が最初に作成した PR、つまりコードへのドキュメント追加を確認しましょう。github.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認すると、GitHub Copilot がファイアウォールでブロックされたという警告が表示されることがあります。Copilot は MCP サーバーへのアクセスを含め、外部リソースへのアクセスが既定で制限されているため、これは**想定された動作**です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、プルリクエストの一覧を開きます。
3. **Add missing documentation** などのタイトルが付いた PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルには **[WIP]** が含まれます。その場合は、Copilot が作業を完了するまで待ちます。数分かかることがあるため、休憩したり、これまでに学んだことを振り返ったりしてください。

4. PR の準備ができたら **Files changed** タブを選び、変更を確認します。

    ![「Files changed」タブ](../../../_images/shared-pr-files-changed.png)

5. 新たに追加された TSDoc ドキュメントコメントや、その他のドキュメントを確認します。実際の変更内容は状況によって異なります。

   変更を確認しながら、TSDoc ドキュメントコメント、TypeScript 規約、ファイル先頭のコメントを探します。これらは演習 2 で確認したカスタム指示ファイルから反映されています。

6. 更新内容を確認して問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. 承認待ちのワークフローがあることを示す表示を探します。
8. ワークフローの承認が必要な場合は **Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄にワークフローが登録されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot と作業する方法は、一方向だけではありません。チームメンバーと同じように、PR のコメントやコードへのインラインコメントで Copilot にメンションできます。Copilot はコメントを確認し、対応するセッションを開始します。結果は確定的ではないため、必ず同じ結果になる依頼文はありません。次のような変更を依頼できます。

- 各コードファイルの先頭に、ファイルの役割を簡潔に説明するコメントを追加する
- TypeScript と Astro のファイルに TSDoc ドキュメントコメントを追加する
- Astro アプリの構成を説明する README を作成する

1. 上記の例などを使い、コードベースのドキュメントに関する変更を依頼するコメントを追加し、**@copilot** をメンションします。
2. **View Session** を選択し、Copilot の作業を確認します。Copilot が更新用の新しいセッションを開始します。
3. **Back to pull request** を選択すると、PR に戻れます。

    ![プルリクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、PR に新しいコミットが追加されます。
5. **Files changed** タブを選び、変更を確認します。

納得できるまで改善を繰り返してください。準備ができたら、ドラフト PR をレビュー可能な状態に変更して `main` ブランチにマージします。

![レビュー可能な状態に変更する](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue を解決するため、Copilot が作成した PR に戻ります。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルが付いた PR を選択します。
4. **Files changed** タブを選び、生成されたコードをレビューします。
5. 更新内容を確認して問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. 承認待ちのワークフローがあることを示す表示を探します。
7. ワークフローの承認が必要な場合は **Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

8. PR のチェック欄にワークフローが登録されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。
9. **オプション:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストできます。Codespace に移動してターミナルを開き、Copilot が作成したブランチ名（例: **copilot/fix-8**）に置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。前と同じように、Copilot cloud agent にコメントを追加して更新を依頼できます。たとえば、表示する関連ゲームの数を変更したり、コメントヘッダーと TSDoc ドキュメントコメントを追加したりできます（この作業は、カスタム指示を更新する前に割り当てたことを思い出してください）。**Conversation** タブにコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用のカスタムエージェントを使って実装した機能を確認します。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 でミッションコントロールから追加を依頼したライトモードの両方が含まれます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルには **[WIP]** が含まれます。その場合は、Copilot が作業を完了するまで数分待ちます。

4. **Files changed** タブを選び、生成されたコードを確認します。
5. 実装をレビューし、特に次の点を確認します。
   - モードを切り替える UI コンポーネント
   - ユーザー設定を保存する local storage の使い方
   - ハイコントラストモードとライトモードの CSS またはスタイルの変更
   - アクセシビリティ属性（ARIA ラベル、キーボード操作など）
   - モード切り替えを管理する JavaScript/TypeScript コード
6. 更新内容を確認して問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. 承認待ちのワークフローがあることを示す表示を探します。
8. ワークフローの承認が必要な場合は **Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄にワークフローが登録されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。
10. **オプション:** Codespace でこのブランチに切り替えて、アクセシビリティ機能を手動でテストできます。Codespace に移動し、ターミナルで Copilot が作成したブランチ名に置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動してブラウザーでハイコントラストモードとライトモードをテストし、ページを再読み込みしても設定が維持されることを確認します。

アクセシビリティ用のカスタムエージェントが、ベストプラクティスに沿った機能実装をどのように支援したか確認してください。アクセシビリティ上の懸念や改善点があれば、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## オプションの演習 — 委任を続ける

cloud agent に実際のバックログ項目を任せ、自分は別の作業に取り組む方法を身に付けましょう。リポジトリに Issue を追加して Copilot に割り当ててみてください。たとえば、次のようなタスクがあります。

- ゲーム詳細ページに支援者向け問い合わせフォームを作成する
- ゲーム一覧ページにページネーションを実装する
- データアクセスヘルパーに入力の検証とエラー処理を追加する

## まとめ

Cloud agent ハーネスを完了しました。各レッスンでは、次の操作を行いました。

- **このリポジトリに含まれるカスタム指示ファイルを確認**し、後で cloud agent の出力に反映される様子を確認しました。
- **Issue を Copilot cloud agent に割り当て**、環境のセットアップ、計画、非同期の実行を確認しました。
- **カスタムエージェントを作成して使用**し、アクセシビリティ用のハイコントラストモードとライトモードを追加しました。
- **Copilot agents のページをミッションコントロールとして使用**し、作業中のアクセシビリティセッションを監視して指示を出しました。
- **cloud agent のプルリクエストをレビューして改善**し、変更を依頼するために `@copilot` をメンションし、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent ハーネスを完了しました。関連するシナリオを別の環境で試すには、次のハーネスに進みます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から Copilot Chat の agent mode と MCP 統合を体験します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、エージェントスキル、QA カスタムエージェント、Playwright MCP、Agent Merge、cloud agent への任意の `/delegate` を使い、ターミナルから 2 つの変更をレビューしてリリースします。

自分のリポジトリでは、次のアイデアを試してみてください。

- リファクタリングやテストカバレッジの Issue を cloud agent に割り当てる
- 別の分野向けのカスタムエージェントを作成する
- 異なる技術スタック向けに `copilot-setup-steps.yml` を設定する

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
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
