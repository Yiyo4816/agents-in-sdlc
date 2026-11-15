---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、参考リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys の一連のワークフローで GitHub Copilot CLI を使いました。次の操作を行いました。

- Codespace を準備し、Copilot CLI をインストールし、プロジェクトを確認して、登録済みのフィルター Issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルターの Issue から始め、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を試しました。
- 品質保証 (QA) 用のカスタムエージェントを作成して選択し、要件、テストカバレッジ、スキルの結果、ブラウザーでの確認内容を評価しました。
- フィルター機能の変更全体を確認し、Agent Merge によるフィルター PR のマージを承認しました。
- コンテキスト、モデル、共有、cloud agent への任意の委任に使うスラッシュコマンドを確認しました。

## リリースしたもの

このワークショップには、更新済みの `main` からそれぞれ別のブランチを作成して進める、2 つの PR があります。

1. **星評価:** 既存の `starRating` と未評価の状態をゲームカードに表示します。
2. **フィルターと品質ワークフロー:** フィルターを実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して、関連するテストを追加します。

フィルターの計画から PR の作成まで、同じ会話とブランチを使いました。ワークショップを簡潔にするため、これらの作業は 1 つの PR にまとめています。

## さまざまな検証方法

自動テスト、自分で行うブラウザー確認、MCP を使った Copilot のブラウザー操作など、複数の方法でコードを確認しました。`quality-checks` スキルはユニットテスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA エージェントはそれらの結果と、要件やテストカバレッジのレビューをまとめました。

追加したテストで、実際の不足を補うようにします。QA の実行結果としてテストの追加が不要な場合もあります。エージェントの要約だけでなく、変更と根拠をレビューしてください。変更後は、影響を受ける検証を再実行します。

## ベストプラクティス

Copilot に与えるコンテキストとツールが、作業結果に影響します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを設定しました。会話をまたいでこれらのカスタマイズを再利用し、チームのニーズに合わせて調整しましょう。指示は標準を設定し、スキルは繰り返し実行するタスクを説明し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。構築の前に方針を検討するには **Plan**、変更を絞って自分も作業に参加するには **Interactive**、範囲の明確なタスクを任せるには **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

インフラストラクチャと同じように、コンテキストも重要です。*何を*作るか、*なぜ*必要か、*どのように*作るかを明確に伝えると、出力に大きな違いが生まれます。

## さらに確認する機能

中心的なワークフローを完了しました。Copilot CLI には、次のような機能もあります。

- `/review` でコードレビューエージェントに変更を分析してもらう
- `/rubber-duck` で問題を整理し、別の視点を得る
- `/fleet` で独立したサブタスクを並行して調整する
- `/worktree` で別のタスクを分離する
- `/delegate` で Copilot cloud agent にタスクを送信する

## 次のステップ

どのツールも、使い続けることが上達への近道です。プロダクションコード、趣味のコード、いつか作ろうと思っていた小さなアプリなどで活用してください。チームと学んだことを共有し、互いの経験から学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張するには、オプションの [Foundry Backer Concierge シリーズ][foundry]に進んでください。別の環境での作業を比べるには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
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
