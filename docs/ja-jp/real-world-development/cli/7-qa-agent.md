---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせた QA カスタムエージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルを使って自動チェックを実行し、Playwright MCP でブラウザーからフィルター機能を確認しました。今度は、これらの機能をカスタムエージェントにまとめて、明確な QA プロセスを定義します。

このレッスンでは、次の操作を行います。

- カスタムエージェントが指示、スキル、MCP ツールと連携する方法を確認します。
- 再利用可能な品質保証プロファイルを作成して確認します。
- QA エージェントを選択し、フィルター機能の Issue と照らして結果を確認します。

## シナリオ

Tailspin Toys では、プルリクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テストカバレッジ、ブラウザー上の動作を一貫した方法でレビューしたいと考えています。カスタムエージェントを使うと QA プロセスをまとめ、再利用できるレポートを作成できます。

## カスタムエージェントとは

カスタムエージェントは、Markdown のプロファイルで定義する、特定の役割に特化した Copilot です。プロファイルには、エージェントの目的、指示、利用できるツールを記載します。このワークショップでは `.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまで使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの指示はチームの標準を説明し、`quality-checks` スキルは繰り返し実行するチェックをまとめ、Playwright MCP はブラウザーツールを提供します。QA プロファイルは、これらを使って要件を評価し、結果を報告する方法を Copilot に伝えます。これらを置き換えるものではなく、別の会話を開始する必要もありません。

## QA プロファイルを作成する

機能の PR を作成する前に、再利用可能な QA プロファイルを Copilot に作成してもらいます。プロファイルでは、QA が行うチェックと守るべき境界の両方を定義します。

1. Codespace に戻り、フィルターの会話が Interactive モードであることを確認します。
2. 既定のエージェントに、次のカスタムエージェントを作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、Copilot が意図した QA ワークフローと境界を反映したか確認します。これにより、検証だけを依頼したのにエージェントが機能を変更する、といった事態を防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. frontmatter を読みます。`description` は必須です。`name` は省略できますが、設定すると一覧でわかりやすい表示名になります。
3. プロファイルの指示を読み、QA が要件を確認し、リポジトリの指示に従い、`quality-checks` スキルを実行し、Playwright MCP を使うことを確認します。
4. QA が根拠を示して報告し、実装コードを変更する前に確認を求め、変更をコミットしたりプルリクエストを作成したりしないことを確認します。
5. いずれかの役割や制約が抜けている場合は、先に進む前に既定のエージェントに修正を依頼します。

## Issue に対して QA を実行する

Copilot CLI は会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルを作成した後、同じフィルターの会話を再開して QA を選択します。そうすると、会話に含まれる Issue と計画の内容を利用できます。

1. 次のコマンドを入力して、エージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成したばかりのエージェントは、使用可能なエージェントの一覧にまだ表示されない場合があります。上のコマンドを実行すると有効になります。

2. 次のプロンプトを使い、QA エージェントに機能をレビューしてもらいます。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を開始します。
4. 作業が完了したら、エージェントが作成したレポートを読みます。

## まとめと次のステップ

再利用可能な専門的役割をワークフローに加え、その結果を確認しました。次の操作を行いました。

- カスタムエージェントが指示、スキル、MCP ツールと連携する方法を確認しました。
- 再利用可能な品質保証プロファイルを作成して確認しました。
- QA エージェントを選択し、フィルター機能の Issue と照らして結果を確認しました。

実装、スキルの更新、QA プロファイル、テスト、検証レポートが揃いました。次は内容をまとめて[機能の PR を作成し、Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用のカスタムエージェントを作成する][create-agents]
- [カスタムエージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
