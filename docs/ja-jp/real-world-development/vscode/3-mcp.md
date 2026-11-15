---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

agent mode でフィルター機能を作成しました。プルリクエストを開く前に、ブラウザーで実際に動作することを確認しましょう。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作してテストしてもらいます。その後、ブランチを公開して PR を作成します。

この演習では、次の操作を行います。

- Model Context Protocol (MCP) とは何か、MCP サーバーが Copilot にツールを追加する仕組みを学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- ブラウザーでフィルター機能を手動テストするよう Copilot に依頼します。
- ブランチを公開し、フィルター機能のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

agent mode は、エディターの外部にアクセスできると、さらに便利になります。Copilot が外部にアクセスする方法の 1 つが Model Context Protocol (MCP) です。エージェントが外部のツールやサービスと通信するための標準的な方法です。

![agent mode の内部構造と、コンテキスト、LLM、ツール (MCP サーバーと VS Code 拡張機能が提供するツールを含む) との連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は、AI エージェントが外部のツールやサービスと通信するための方法を提供します。MCP を使うと、AI エージェントは外部サービスとリアルタイムにやり取りし、最新情報にアクセスしたり、ユーザーに代わって操作を実行したりできます。

ツールやリソースへのアクセスは MCP サーバーを介して行います。MCP サーバーは AI エージェントと外部ツールやサービス（既存の API や npm パッケージなどのローカルツール）の橋渡しをし、エージェントが利用できるツールやリソースを管理します。

広く使われている MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API を利用できます。新しいリポジトリの作成、既存リポジトリの更新、Issue やプルリクエストの管理などを実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright によるブラウザー自動化機能を利用できます。Web ページへの移動、フォームへの入力、ボタンの選択などを実行できます。

ほかにも多くの MCP サーバーが公開されています。GitHub がホストする [MCP レジストリ](https://github.com/mcp)で、エコシステム内のサーバーを探したり、登録に貢献したりできます。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーはプロジェクトのほかの依存関係と同様に扱ってください。利用する前にソースコードを確認し、発行者を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーのみを使い、機密リソースや操作へのアクセスを許可する際は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースから利用できる MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーがすでに設定されています。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

`github` の設定はプロジェクトのテンプレートに含まれています。そのため、前の演習で Copilot が Issue のバックログを読み取れました。この設定は[リモート GitHub MCP サーバー][remote-github-mcp-server]を使います。ローカルへのインストールは不要です。VS Code は HTTP で接続し、Copilot が初めてツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次は 2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]は Copilot が操作できるブラウザーを提供します。機能をテストするために使います。

1. `.vscode/mcp.json` の `github` と並べて `playwright` の設定を追加し、ファイルを次のようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートサーバーの `github` とは異なり、`playwright` はローカルサーバーです。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグは、画面を表示せずにブラウザーを実行する設定です。Codespace には表示用のデスクトップがないため必要です。

> [!NOTE]
> Tailspin Toys ではエンドツーエンドテストに Playwright を使っているため、必要なブラウザーがすでにインストールされていることが多くあります。Copilot がブラウザーの不足を報告した場合は、`npx playwright install chromium` を実行してから再試行します。

## サーバーを起動して信頼する

VS Code は、Copilot が初めてサーバーのツールを必要としたときに MCP サーバーを起動し、サーバーを信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` の上に **Start** 操作が表示されます。選択してサーバーを起動し、設定が正しいことを確認します。
2. VS Code でサーバーを信頼するか尋ねられた場合は、設定を確認し、サーバーを起動できるように信頼します。

手動で起動する必要はありません。省略した場合は、次の手順で Copilot が初めて Playwright のツールを使うときに、VS Code がサーバーを起動して信頼の確認を表示します。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。自分でアプリを操作する代わりに、エージェントにページを開いて移動し、フィルターを適用して結果を読み取らせ、その内容を要約してもらえます。会話を離れずに機能の動作を確認できます。

Playwright MCP サーバーはスクリーンショットではなく、ページの[アクセシビリティツリー][playwright-mcp-server]を使います。ボタン、リンク、リスト項目などのラベル付き要素を基にエージェントが判断します。支援技術と同じように構造化された情報を使うため、機能の簡易テストとアクセシビリティの確認を兼ねられます。

サーバーが接続され、アプリが起動している状態で、次のプロンプトを Copilot に送信してフィルター機能を試します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーからブラウザーを起動し、各手順を実行して結果を報告します。報告内容を Issue の受け入れ基準と照らし合わせます。問題があれば、プルリクエストを作成する前に追加の質問をしたり、コードを修正するよう依頼したりします。

> [!NOTE]
> テストするには、アプリが `http://localhost:4321` で起動している必要があります。開発サーバーを停止した場合は、プロンプトを送る前に再起動します。Playwright MCP サーバーを初めて使うときは、ブラウザーをダウンロードする場合があります。ブラウザーが見つからないと報告された場合は `npx playwright install chromium` を実行して再試行します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認できたら、チームのレビュー用にプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace で **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミットメッセージを作成します。

   ![Copilot が行った変更を示す Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して、ブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンドライン インターフェイス (CLI) など、プルリクエストを作成する方法はいくつかあります。GitHub Copilot を使って、関連する Issue を探し、Issue に関連付けた PR を作成してもらいましょう。

1. Copilot Chat パネルに移動し、**New Chat** を選んで新しいセッションを開始します。
2. エージェント選択メニューで **Agent** が選択され、Copilot が GitHub ツールを使えることを確認します。
3. 次のように依頼して、PR を作成します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選び、情報収集や操作の実行を許可します。GitHub ツールを初めて使うときに GitHub への認証を求められた場合は、画面の指示に従います。
5. Copilot が Issue を検索し、該当するものを見つけて PR を作成することを確認します。
6. Copilot が作成したリンクを選び、プルリクエストを確認します。まだ**マージしないでください**。

## まとめと次のステップ

この演習では、次の操作を行いました。

- Model Context Protocol (MCP) とは何か、MCP サーバーが Copilot にツールを追加する仕組みを学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- ブラウザーでフィルター機能を手動テストするために Playwright MCP サーバーを使いました。
- ブランチを公開し、フィルター機能のプルリクエストを作成しました。

機能のテストに MCP サーバーを使いました。Copilot を拡張する方法は MCP だけではありません。次は[カスタムエージェントを作成し][next-lesson]、アクセシビリティレビューなど、対象を絞ったタスクを効率化します。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
