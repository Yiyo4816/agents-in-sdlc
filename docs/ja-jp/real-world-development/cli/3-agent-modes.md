---
title: "レッスン 3 - Agent modes: Plan と Autopilot"
description: "Plan で方針を合意し、Autopilot で Issue に基づくフィルター機能を実装して、Interactive モードで結果を確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

プロジェクトに小さな機能を追加しました。より大きな変更には、さらにしっかりとしたプロセスが必要です。GitHub Copilot CLI は組織の既存のワークフローに沿って動作し、必要なものを正しい方法で構築できるように支援します。このレッスンから数回にわたり、Issue から新機能を計画し、コードの妥当性と機能の動作を確認し、プロジェクトへのマージを完了する、一般的なエージェント主導の開発プロセスを体験します。

> [!NOTE]
> 機能の開発を続ける間、同じ会話とブランチを使います。通常は作業対象ごとにブランチや PR を分けますが、中心的な概念に集中できるよう、このワークショップでは手順を簡略化しています。

このレッスンでは、次の操作を行います。

- GitHub Issue から Copilot の会話を開始します。
- Plan モードで要件を決めます。
- Autopilot モードで機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動で検証します。

この機能の開発を続けながら、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加して QA エージェントを作成し、機能の PR を作成します。

## シナリオ

Tailspin Toys のカタログが増え、利用者はカテゴリや発行者でゲームを絞り込みたいと考えています。バックログの Issue に機能の説明がありますが、カテゴリを組み合わせる方法など、実装前に合意しておくべき点があります。Plan モードで方針を決め、その後、範囲を明確にして Autopilot に実装を依頼します。

## 背景

AI コーディングエージェントを開発に加えても、基本は変わりません。むしろ、基本を守ることが一層大切になります。多くの開発者は、次のような流れで作業します。

1. 必要な作業を説明する Issue から始めます。
2. 作成するものを計画します。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新しい機能を手動で確認します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> 手順の詳細は、チームや組織によって異なります。ただし、多くの場合は上記の流れを調整したものになります。

標準的な方法を守ることで、AI が生成したコードが要件を満たし、手作業で書いたコードと同じレビューのプロセスを通るようにできます。

## 会話モード

**会話モード**は、エージェントにどの程度の自律性を与えるかを決めます。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すと、モードを切り替えられます。

- **Interactive**: エージェントと一緒に作業します。エージェントは変更を提案し、次に進む前に入力を待ちます。
- **Plan**: エージェントが最初に計画を作成します。プロジェクトファイルの編集はできません。
- **Autopilot**: エージェントがコードを作成し、テストを実行して、タスクが完了するまで自律的に改善します。

まず Plan モードで計画を確認し、その後 Autopilot で実装します。

## Issue から始める

フィルター機能の作業を始める前に、Codespace とリポジトリ、ターミナルを準備します。

1. Codespace に戻ります。停止している場合は、再起動してから続けます。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルター作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトが `#7` (または同様の番号) から始まることを確認します。`#` を使うと、GitHub 上の Issue やプルリクエスト (PR) をコンテキストに追加できます。

## フィルター機能を計画する

計画を立てると、Copilot に実装を任せる前に、機能の作成方法や作業内容を決められます。複雑な作業では、時間をかけて計画を立てると効果的です。Plan モードに切り替え、Copilot に計画を作成してもらいましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** であることを確認します。
2. 前の手順で追加した Issue の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べ、最適な方法を検討して計画を作成します。

3. 途中でフィルターの仕様について質問された場合は、希望に合わせて回答します。ここでは、唯一の正解はありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して展開します。
5. 計画を上から下まで確認します。
6. 自分の決定に合わない部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を確認できたら、実装を始めます。Autopilot を使って Copilot に作業を依頼しましょう。

Copilot は、タスクが完了したと判断するまで問題に取り組み、改善を続けます。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みのバージョンで同様の名前が付いたオプションを選びます。
2. プロンプトの下にあるモード表示が **Autopilot** であることを確認します。
3. Copilot が計画に沿ってコードを生成し、テストを実行して改善する様子を確認します。

> [!NOTE]
> 承認すると、すぐに実装が始まることがあります。先に計画を確認してください。依存関係が不足している、またはポートが競合していると報告された場合は、チェック完了とみなす前にセットアップの問題を解決してください。

## 実装をレビューして検証する

生成されたコードは、ほかのコードと同じようにマージ前にレビューします。コードを確認してサイトを実行し、問題がないか確認しましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示に **Plan** または **Autopilot** と表示されていないことを確認します。
2. `/diff` を入力し、フィルターの実装とテストを確認します。
3. 結果を Issue と計画中に決めた内容と比較します。
4. レビューが終わったら <kbd>Esc</kbd> を押して差分ビューを閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新しい機能を試す

コードに問題がないことを確認できました。次はアプリを起動して、Codespaces のポート転送を使ってサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら **Open in Browser** を選択します。
3. 計画で決めたとおりにカテゴリと発行者で絞り込み、組み合わせも試します。
4. リセットと結果が空の場合の動作が、Issue と自分の決定に合っていることを確認します。
5. Codespace に戻り、Copilot が起動した開発サーバーを停止するよう依頼します。

## まとめと次のステップ

会話モードを使い分けて、機能を作成し、レビューしました。このレッスンでは次の操作を行いました。

- GitHub Issue から Copilot の会話を開始しました。
- Plan モードで要件を決めました。
- Autopilot モードで機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動で検証しました。

次は[カスタム指示を使って][next-lesson]、コードが文書化された規約に従うようにしましょう。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
