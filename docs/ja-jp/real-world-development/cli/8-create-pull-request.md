---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "フィルター機能、指示、スキルの更新、QA プロファイル、テストをまとめて確認し、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルター機能の実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストは、すべて 1 つのブランチに保存されています。まとめて確認し、プルリクエストを作成しましょう。星評価のプルリクエスト (PR) は自分でマージしましたが、今回は **Agent Merge** にマージの作業を任せます。

> [!NOTE]
> 通常は機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。このワークショップでは手順を簡潔にするため、フィルター機能と品質向上の作業を 1 つの会話とブランチにまとめ、この PR に含めました。

このレッスンでは、次の操作を行います。

- Agent Merge と、マージのライフサイクルを自動化する方法を学びます。
- 機能の変更全体と検証の根拠を確認します。
- フィルター機能の PR を作成します。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルター機能のワークフローを通じて、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys では、マージの承認を開発者が管理しながら、PR の残りの作業を自動化したいと考えています。

## Agent Merge とは

**Agent Merge** は、プルリクエストをマージするために必要な残りの作業を自動化します。有効にすると、Copilot は PR を妨げている問題（失敗した継続的インテグレーション (CI) チェックの修正、レビューコメントへの対応、必要な場合のリベースなど）に取り組みます。その後、リポジトリで許可されていれば GitHub の自動マージを有効にします。

これまでは **Merge pull request** を自分で選択していました。Agent Merge にその作業を任せることもできます。Agent Merge を有効にする前に、変更をレビューして準備ができていることを確認してください。

## Agent Merge で PR を管理する

コードの作成が完了したので、まとめてレビューし、PR を作成して、残りの処理を Agent Merge に任せましょう。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントのダイアログを開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドを使って新しい PR を作成します。
5. `/pr agentmerge` を使って Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションを監視します。すべて成功すると、マージを実行します。
7. Copilot から「PR #14 was squash-merged successfully.」のようなメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリ設定、権限の不足を回避しません。処理がブロックされた場合は理由を確認し、リポジトリで許可されていれば、レビュー済みのマージを手動で完了してください。

## まとめと次のステップ

コード生成、テストと検証、プルリクエストの処理など、開発プロセスのいくつかの部分を自動化しました。次の操作を行いました。

- Agent Merge と、マージのライフサイクルを自動化する方法を学びました。
- 機能の変更全体と検証の根拠を確認しました。
- フィルター機能の PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、任意のクラウドへの委任に役立つ Copilot CLI のスラッシュコマンドを[確認します][next-lesson]。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
