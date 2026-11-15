---
slug: ja-jp/real-world-development/vscode
title: "Visual Studio Code"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

VS Code の **[GitHub Copilot Chat](https://code.visualstudio.com/docs/copilot/chat/copilot-chat)** を使うと、普段のコードエディターで GitHub Copilot を利用できます。Visual Studio Code と GitHub Codespaces で、Copilot Chat の agent mode を使い、MCP を通じて外部ツールに接続し、カスタムエージェントを活用します。すべて IDE を離れずに進められ、Copilot はファイル、ターミナル、問題の全体を把握できます。

まずカスタム指示を追加し、Copilot がその指示に従う様子を確認します。次に、エージェントモードを使って UI、データ層、テストにまたがるフィルター機能を構築します。その後、Playwright MCP サーバーを接続し、プルリクエストを作成する前に Copilot にブラウザーを操作させて機能をテストします。最後に、アクセシビリティ作業用のカスタムエージェントをレビューして使い、Copilot の変更を監視し、方向付けながら反復して改善します。すべてエディターを離れずに進められます。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと codespace を作成します |
| [1. カスタム指示][ex1] | コンテキスト | VS Code でカスタム指示を追加して検証します |
| [2. Agent Mode][ex2] | コード生成 | agent mode でフィルター機能を構築します |
| [3. Playwright による MCP][ex3] | 外部ツール | Playwright MCP サーバーを使ってブラウザーで機能をテストします |
| [4. カスタムエージェント][ex4] | 専門エージェント | カスタムエージェントをレビューして使います |
| [5. エージェントの管理][ex5] | 監視 | エージェントのセッションを監視し、方向付けします |
| [6. 繰り返し改善する][ex6] | レビュー | Copilot の作業をローカルで確認し、次の手順を選びます |
| [オプション: Foundry の導入][foundry-toolkit] | AI エージェント | VS Code と Foundry Toolkit でモデルを準備し、エージェントをデプロイしてサイトに接続します |

## 前提条件

このワークショップに参加する前に、次の項目を確認してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかのプランがある GitHub アカウント
- [ ] GitHub Codespaces へのアクセス

> [!TIP]
> 有料プランを利用していませんか？認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれているため、すべてのハーネスを完了できます。

[callout-student-plan-education]: https://github.com/education/students

## はじめる

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-agent-mode/
[ex3]: 3-mcp/
[ex4]: 4-custom-agents/
[ex5]: 5-managing-agents/
[ex6]: 6-iterating/
[foundry-toolkit]: 7-foundry-toolkit/
