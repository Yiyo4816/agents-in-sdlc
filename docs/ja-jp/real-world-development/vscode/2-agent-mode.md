---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの最も簡単な更新でも、複数のファイルを変更したりテストを実行したりする必要があります。開発者は通常、必要なファイルを見つけ、変更を加え、テストを実行し、デバッグし、変更し忘れたファイルを探して、さらに更新する、という作業を繰り返します。

こうした作業に役立つのが Copilot Agent Mode です。

Copilot Agent Mode は IDE 内で、より自律的に作業するように設計されています。開発者と同様に、プロジェクトの構造を調べ、必要な変更を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。Agent Mode を使ってサイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる用途向けに設計されています。agent mode は IDE 内で作業し、短いフィードバックサイクルでやり取りできます。cloud agent はチームメンバーのように非同期で作業し、Issue とプルリクエストを通じてやり取りします。

この演習では、次のことを学びます。

- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを特定し、複数の変更を連携して行う方法
- Copilot Agent Mode が UI とデータレイヤーを横断して新機能を実装する方法
- コードベースにマージする前に、Copilot が作成した変更とテストをレビューする方法

## シナリオ

ゲームの一覧が増えてきたため、利用者がカテゴリと発行者で絞り込めるようにします。前の演習では発行者ヘルパーを追加しました。今回は Copilot Agent Mode を使い、データレイヤー、UI、テストの残りの作業を完了します。

## Tailspin Toys の Web サイトを起動する

変更を加える前に、Tailspin Toys の Web サイトを確認し、現在の機能を把握しましょう。

この Web サイトは、開発者向けのテーマを持つボードゲームのクラウドファンディングプラットフォームです。ゲーム一覧と詳細を表示します。サイトは Astro アプリ 1 つで構成され、ビルド時に静的 HTML としてページを生成します。ページは frontmatter 内で Drizzle ORM を使ってローカル SQLite データベースを直接参照します。別のバックエンド API やクライアントサイド UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` に置き、インタラクティブな機能は標準の DOM API を使う小さな Astro `<script>` で追加します。

### Web サイトを起動する

単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. 前の演習で使った `main` 上の Codespace に戻ります。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押し、新しいターミナルを開きます。
3. フィルター機能用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro の開発サーバーが起動すると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くように求めるダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルの `http://localhost:4321` を Mac では <kbd>Command</kbd>、Windows/Linux では <kbd>Ctrl</kbd> を押しながら選択して Web サイトを開きます。

> [!NOTE]
> Codespace でターミナルの localhost URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動転送されます。これは Codespace の Web サーバーをホストするためのプライベートトンネルです。

### Web サイトを確認する

Web サイトが起動したら、機能を確認します。主な機能は次のとおりです。

- **ホームページ:** ボードゲームのタイトル、画像、説明を一覧表示します。
- **ゲーム詳細ページ:** ゲームを選択すると、タイトル、説明、発行者、カテゴリなどの詳細を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選び、新しい会話を始めます。モードとモデルの選択は演習によって異なるため、必要な箇所で説明します。

Web サイトの最初の実装は動作しますが、新しい機能を追加して改善します。まずバックログを確認しましょう。テンプレートからプロジェクトをセットアップすると、Issue のバックログが自動的に作成されます。GitHub Copilot に一覧を表示してもらいます。

1. Chat ビューのエージェント選択メニューで **Agent** を選びます。Agent は複数のファイルにまたがる変更を計画して実装し、ターミナルコマンドを実行して、ツールを呼び出します。

   ![Chat ビューのエージェント選択メニューを示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. 使用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が、特定のモデルを推奨しているわけではありません。作成時に Claude Sonnet 4.5 を使用したため、手順にも記載しています。提案されるコードに一貫性を持たせ、学習しやすくするためです。ただし、LLM は確率的に動作するため、提案内容が例と異なることがあります。これは通常の動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が別の MCP コマンドを使用する場合があります。それでもタスクを完了できるはずです。

3. 次のプロンプトを Copilot に送信して、Issue のバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選び、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot が利用者にとって役立つと考えた順に、項目を優先順位付けしたことを確認します。

## 指示ファイルを確認する

Copilot にコードを生成してもらう前に、作業のコンテキストを伝えるために使う指示ファイルを確認します。この Web サイトに機能を追加する方法が記載された [ユーザーインターフェイス (UI) 指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能を追加するときの全体的な指針を確認します。次の内容が含まれます。
   - アーキテクチャの概要
   - コンポーネント設計、テスト容易性、アクセシビリティの原則
   - Astro、Tailwind CSS、Drizzle などのファイル向け指示ファイルへのリンク

> [!TIP]
> 指示ファイルから、ほかの指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルの場所からの相対パスです。指示を再利用可能な小さな単位に分けたり、Copilot に例やテンプレートを示したりできます。

## フィルター機能を実装する

フィルター機能を完成させるには、アプリケーションに少なくとも 3 つの変更が必要です。

- データレイヤー (`src/lib/`) にフィルター用ロジックを追加または調整する
- フィルターの動作を確認するテストを追加または更新する
- ゲーム一覧ページにフィルター UI を追加する

さらに、コードベースにマージする前にテストを実行し、成功することを確認します。Copilot Agent Mode にこれらの作業を依頼しましょう。

1. Copilot の現在の会話を続けるか、**New Chat** を選んで新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選んで指示ファイルを追加します。

   ![UI 指示ファイルを選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント選択メニューで **Agent** が選ばれていることを確認します。

   ![Chat ビューのエージェント選択メニューを示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. モデルが引き続き **Claude Sonnet 4.5** であることを確認します。
5. バックログにある関連 Issue の要件に沿って機能を実装するよう、次のプロンプトで Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、必要なファイルを見つける様子を確認します。データレイヤーのヘルパー、UI、テストを探して変更し、テストを実行します。

   ![Copilot がプロジェクトのファイルを調べている様子を示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行います。タスクやコードベースの複雑さによっては数分かかることがあります。作業中、コードエディターに **Keep** と **Undo** ボタンが表示される場合があります。Copilot の作業中に選ぶ必要はありません。完了すると、すべての変更に対して **Keep** または **Undo** を選べるようになります。

7. Copilot が確認を求めたら **Continue** を選び、テストを実行します。

   ![Copilot Chat でテスト実行の確認を求めるダイアログのスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりすることがあります。問題ありません。エラーが見つからなくなるまで、Copilot はコード生成とテストを繰り返します。

   ![Copilot Agent Mode の Chat セッションが完了した画面](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いたブラウザーに戻り、新しい機能を試します。
11. 動作を確認し、コードをレビューしたら Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot Agent Mode を使って Tailspin Toys の Web サイトに機能を追加しました。次のことを学びました。

- GitHub Copilot Agent Mode で UI とデータレイヤーを横断する新機能を実装する方法
- プロジェクトを調べ、関連ファイルを特定し、複数の変更を連携して行う方法
- コードベースにマージする前に、Copilot が作成した変更とテストをレビューする方法

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### 追加の演習 — ページネーションを実装する

ゲームの一覧が増えるとページネーションが必要になります。この演習で学んだことを使い、Copilot にページネーションを追加してもらいましょう。次の点を検討してください。

- 既存の指示ファイルを使うなど、ベストプラクティスに従う
- ページサイズを利用者が選べるようにするか、固定にするかなど、ページネーションの方法を検討する
- 希望どおりの実装に必要なガイダンスをプロンプトに含める
- Copilot に追加の変更を依頼したりコンテキストを伝えたりしながら、何度か改善する場合がある。これは通常の作業です。

## リソース

- [Copilot の ask、edit、agent mode: それぞれの役割と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
