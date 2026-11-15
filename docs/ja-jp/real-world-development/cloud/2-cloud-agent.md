---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能要望など、さまざまな課題があります。GitHub Copilot cloud agent は、コードの更新や機能の追加などを自律的に実行できます。作業が完了すると、人がレビューできるドラフトのプルリクエスト (PR) を作成します。面倒な作業を任せて開発プロセスを加速し、開発者がより大きな課題に集中できるようにします。

Copilot cloud agent を使って、次のことを学びます。

- コード生成環境をカスタマイズする方法
- 安全に操作を実行する方法
- Issue の範囲を明確に定めることの重要性
- Issue を Copilot に割り当てる方法

## シナリオ

Tailspin Toys には解決したい技術的負債があります。サイトの初期版を作成した委託先は、ドキュメントを十分に整備していませんでした。まず、アプリケーションのエクスポート関数すべてに TSDoc ドキュメントコメントを追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。ゲーム詳細ページに同じカテゴリの別のゲームを表示すれば、利用者は引き続きゲームを探せます。今の段階では完成度の高い実装は不要で、UX の受け入れテストに使える状態にしたいと考えています。この機能は現在ブロッカーですが、より優先度の高い Issue がほかにあります。

どちらも、優先度が下がりやすいタスクです。Copilot cloud agent に割り当てれば、開発者がほかの作業に取り組んでいる間に非同期で処理され、後から結果をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドで作業できます。たとえば、[GitHub Issue を Copilot に割り当て][assign-issue]る方法があります。割り当てると、Copilot は進捗を記録するドラフトのプルリクエストを作成し、環境をセットアップしてタスクに取りかかります。作業中も完了後もセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を明確に定めた指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。簡単そうに見える操作でも、詳しく調べると複雑なことがあります。[Copilot cloud agent に依頼するタスクの範囲を慎重に決めてください][cloud-agent-best-practices]。AI ペアプログラマーとして扱い、段階を踏んで学び、試し、状況に合わせて進め方を調整します。生成 AI を加えても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と Copilot のガイドに使う方法を学びました。すでにカスタム指示の演習を完了している場合は、`.github/copilot-instructions.md` が機能することを確認しています。まだの場合は、ここでファイルを確認しましょう。Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認し、後からその効果を見られるようにします。

リポジトリの GitHub Web UI、または起動済みの Codespace で次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクション、特に TypeScript 規約と TSDoc ドキュメントコメントの要件を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` frontmatter を確認します。この指示は `**/*.test.ts` ファイルに適用されます。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てるとき、プルリクエストに TSDoc コメント、TypeScript 規約、ファイル先頭のコメントが反映されるか確認します。後の演習で PR をレビューするときにも取り上げます。

## Copilot cloud agent の開発環境を設定する

開発者が誰であっても、コードの作成には適切な環境と、状態を整えるためのセットアップスクリプトが必要です。ソフトウェアエンジニアのように作業する Copilot にタスクを割り当てる場合も同じです。

cloud agent は作業環境として [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発ツールと依存関係を利用できるように環境をカスタマイズできます。このラボの流れを簡単にし、学習に集中できるよう、あらかじめ設定されています。単一の Astro アプリ用に Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、マイグレーションとシードを済ませた SQLite データベースを利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの「Actions」タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Copilot は自分の操作用トークンを受け取ります。ここでは、手順の実行に必要な最小限の権限を設定します。
    permissions:
      # 依存関係のインストールなど、セットアップ手順の中でリポジトリをクローンする場合は、`contents: read` 権限が必要です。セットアップ手順でクローンしない場合は、完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro と Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ローカル SQLite データベースをマイグレーションし、ビルドとテスト用のデータを投入します。
      - name: Set up the database
        run: npm run db:setup
```

ほかの GitHub ワークフローと同じ形式ですが、いくつか重要な点があります。

- `copilot-setup-steps` という 1 つのジョブがあります。このジョブは、Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot が実行するのを待つ代わりに、正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く理解されていますが、古い情報しかない、またはまったく整備されていないプロジェクトも多くあります。こうした技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しいメンバーの受け入れを難しくします。Copilot はドキュメント作成を得意とするため、この作業は Copilot cloud agent に割り当てるのに適した Issue です。バックグラウンドでドキュメントを作成してもらい、後の演習で結果を確認します。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当て用ダイアログを開きます。

    ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のような名前になります。PR が一覧に表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が todo リストを作成したことを確認します。

> [!NOTE]
> Copilot が todo リストを PR に表示するまで数分かかる場合があります。Copilot は前述のワークフローを実行して環境を準備し、プロジェクトを分析し、問題への最適な取り組み方を検討しています。

13. todo リストと、Copilot が完了する予定のタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

    ![Copilot のセッションビュー](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示を確認するには、ウィンドウを更新する必要がある場合があります。

16. ライブセッションをスクロールし、Copilot の作業を確認します。コードを調べて状態を理解し、計画を考えて判断を下し、コードを作成する様子を確認できます。

この処理には数分かかることがあります。Copilot cloud agent の主な目的は、作業を非同期で実行して、ほかのタスクに取り組めるようにすることです。この利点を活用するため、もう 1 つのタスクを cloud agent に割り当て、その間にアプリの機能を追加しましょう。

## ゲーム詳細ページに関連ゲームのセクションを追加する

Copilot cloud agent の大きな利点の 1 つは、作業を分けられることです。デザインチーム向けに関連ゲームのセクションを追加するのに時間がかからないとしても、その時間をほかの作業に使えます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択します。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

    - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
    - Show a "Related games" section on the game details page that uses the helper
    - Handle the case where a game has no related games
    - There should be unit tests created for the new helper
    - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当て用ダイアログを開きます。

    ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

間もなく、Issue の最初のコメントに 👀 が表示され、Copilot が作業中であることがわかります。

![Copilot が作業中であることを示す目の絵文字が付いた Issue のコメント](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。Copilot cloud agent は開発者と同じように作業するため、完了するまで常時監視する必要はありません。完了後にレビューしましょう。次はカスタムエージェントの作成と使用を確認します。

## まとめと次のステップ

このレッスンでは [GitHub Copilot cloud agent][copilot-agents] を確認しました。cloud agent に Issue を割り当てると、非同期で作業を依頼できます。技術的負債への対応、新機能の作成、フレームワーク間のコード移行などに活用できます。

次の操作を行いました。

- コード生成環境をカスタマイズしました。
- 安全に操作を実行することを確認しました。
- Issue の範囲を明確に定めることの重要性を学びました。
- Issue を Copilot に割り当てました。

cloud agent がバックグラウンドで作業している間に、次はカスタムエージェントを作成して使います。[Copilot cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム指示も使用します。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
