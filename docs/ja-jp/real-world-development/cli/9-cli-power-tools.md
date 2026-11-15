---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキスト、モデル、セッション共有、クラウドへの任意の委任に使うスラッシュコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI ツールと同様に、GitHub Copilot CLI には操作に使えるスラッシュコマンドが多数あります。高度な機能、内部情報、追加の設定オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などのコマンドを使いました。ほかの便利なコマンドも確認しましょう。

このレッスンでは、次の操作を行います。

- `/context` と `/compact` を使って、Copilot が会話のコンテキストを管理する方法を確認します。
- `/model` を使って、利用可能なモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並行作業、worktree、cloud agent への委任に使える任意のコマンドを確認します。

## シナリオ

CLI の中心的なワークフローは完了しました。次に、コンテキストの管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent] への任意の委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクでは、モデルの最大コンテキストウィンドウに達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。また、スラッシュコマンドを使って状態を確認したり、自分で圧縮したりできます。

1. Codespace に戻り、まだ開いていない場合はリポジトリのルートから Copilot CLI を起動します。
2. 次を入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域に分割されたコンテキストを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して、結果を比較します。会話が短い場合は、大きな違いが見られないことがあります。

> [!NOTE]
> Copilot CLI はコンテキストウィンドウが埋まると、自動的にコンテキストを圧縮します。自分でタイミングを選びたい場合は `/compact` を使います。無関係なタスクに切り替えて新しい会話を始めたい場合は `/clear` または `/new` を使います。

## モデルを選ぶ

モデルによって得意なことが異なり、開発者によって好みも異なります。Copilot CLI では、利用可能なモデルを一覧表示して選択できます。

1. 次を入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況に関する情報を確認します。
3. 現在のモデルを使い続けるか、別のモデルを選ぶか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで作業し、学んだことを共有すると、AI ツールの活用方法を互いに高められます。`/share` コマンドでは、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist を公開したりできます。

1. `/help` を入力して、利用中のバージョンにある `/share` のオプションを確認します。
2. セッションを共有する場合は、目的に合った方法を選びます。たとえば `/share file` は、ローカルの Markdown ファイルにエクスポートします。
3. エクスポートした内容を誰かに送ったり公開したりする前に確認します。エクスポートにはプロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有するつもりのないリポジトリや会話の内容は公開しないでください。

## オプション: 作業を拡張または委任する

ワークショップの中心的な内容は完了しました。Copilot CLI には、大きなタスク向けのコマンドもあります。

- `/fleet` は独立したサブタスクをサブエージェントに割り当て、並行して実行できます。
- `/worktree` は別のタスク用に独立した Git worktree を作成できます。
- `/delegate` は、非同期で作業してプルリクエストを作成することもある Copilot cloud agent にタスクを送信できます。

これらのコマンドは追加の worktree やリモート作業を作成する可能性があるため、任意の機能です。試すときは、範囲の明確な新しいタスクから始め、通常のワークフローで結果をレビューしてください。非同期エージェントの作業を詳しく確認するには、[cloud agent ワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI のスラッシュコマンドを使うと、設定を変更したり、セッションを共有したり、内部の動作を確認したりできます。このレッスンでは、次の操作を行いました。

- `/context` と `/compact` を使って、Copilot が会話のコンテキストを管理する方法を確認しました。
- `/model` を使って、利用可能なモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並行作業、worktree、cloud agent への委任に使える任意のコマンドを確認しました。

ほかにも Copilot CLI のスラッシュコマンドや機能があります。最後に、これまでに学んだことと、学習を続けるための方法を[振り返りましょう][next-lesson]。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
