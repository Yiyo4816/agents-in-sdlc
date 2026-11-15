---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。作業の方法や Copilot が知っておくべき背景情報がある場合、その情報にアクセスできるようにしましょう。Copilot に特定のコンテキストを伝える方法はいくつかあります。その中でも重要なのが[指示ファイル][instruction-files]です。指示ファイルを使うと、コード生成の指針を伝えられます。

この演習では、次の操作を行います。

- リポジトリのカスタム指示とパス別の指示ファイルを使って、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- 既存の指示に従ってフィルター用の最初のデータスライス（発行者ヘルパー）を生成します。
- リポジトリ全体に適用する標準を `.github/copilot-instructions.md` に追加します。
- 同じプロンプトを再び実行し、生成コードが新しい標準に従う様子を確認します。
- 指示の更新とフィルタリング用データスライスを `main` にコミットし、次の演習で Copilot が新しい指針を使えるようにします。

> [!CAUTION]
> 生成コードが、指定した標準の一部に従わないことがあります。Copilot は決定論的ではありません。この演習では、出力の一字一句を一致させるのではなく、指示を更新した後の動作の**傾向**を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発プロセスに関するガイドラインと要件があります。たとえば、次のとおりです。

- データレイヤーには必ずユニットテストを用意します。
- UI はダークモードにし、現代的な印象にします。
- コードのドキュメントには TSDoc ドキュメントコメントを使用します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使って、Copilot がこれらの開発方針に沿って作業できるようにします。

### カスタム指示

カスタム指示では、コーディングスタイルや要件に関するコンテキストと設定を Copilot に伝えます。コーディング規約、ライブラリ、コメントの書き方などを指定して、より適切な提案やコードスニペットを得るために利用できます。リポジトリ全体、または特定の種類のファイルに対する指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に渡される単一の指示ファイルです。技術スタック、開発対象の概要、ベストプラクティス、全体に適用する指針など、ほとんどのチャットや CLI リクエストに関係する情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類に使う指示ファイルです。TypeScript や Astro などの言語、UI コンポーネントの作成、ユニットテストの作成といった作業向けに作成できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示ファイルと、`applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方をコード生成に使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの形式の指示ファイルにも対応しています。[対応する指示ファイル][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成方法をすべて説明することはこのワークショップの範囲外ですが、サンプルプロジェクトには代表的な構成例があります。基本的には、次の点を意識してください。

- `copilot-instructions.md` には、開発対象やプロジェクトの構造、全体的なコーディング標準など、プロジェクトレベルの指針を記載します。
- `*.instructions.md` には、ユニットテスト、Astro コンポーネント、データレイヤーなどのファイル種類や、特定のタスクに関する指針を記載します。
- 自然な言葉を使い、明確に記述します。コードの望ましい例と避けるべき例も示します。

指示ファイルの作成や AI の使い方に唯一の正解はありません。プロジェクトで試しながら、効果的な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な指示ファイルを用意しましょう。このプロジェクトのファイルを確認すると、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けの指示があります。
>
> Copilot に指示ファイルを作成してもらうこともできます。各環境で方法は異なります（たとえば VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init` など）。必要な場合は、各ハーネスのレッスンで説明します。
>
> テンプレートや出発点を探していますか？指示ファイル、カスタムエージェント、その他のリソースをまとめた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリには、中心となる `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いて確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明や **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には **GitHub Actions Workflows** の指針もあります。これらは Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて、ファイルを確認します。Astro ファイル、Drizzle データレイヤー、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、リポジトリルートを基準とする glob を指定します。この例では `**/*.test.ts` に一致する TypeScript テストファイルが対象です。
5. このプロジェクトのユニットテスト作成に関する指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト内のファイルへのリンクがあります。大きな指示を再利用できる小さなファイルに分け、コード生成の例を Copilot に示す方法を確認してください（ここにあるパスはリポジトリルートではなく指示ファイルからの相対パスです）。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメント作成はまだ必須ではありません。次の手順で TSDoc ドキュメントコメントとファイル先頭のコメントの規則を追加します。

## 変更用ブランチを作成する

ここではベストプラクティスに従って、変更用ブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選び、新しい会話を始めます。モードとモデルの選択は演習によって異なるため、必要な箇所で説明します。

カスタム指示の効果を確認するため、まず現在の指示ファイルを使ってプロンプトを送信します。後で指示を更新した後、同じプロンプトをもう一度送信して違いを確認します。

1. 前の演習で開いたエディタータブを閉じ、Copilot が必要なコンテキストだけを取得するようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置先を Copilot に示します。
3. Chat ビューのエージェント選択メニューで **Agent** を選び、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント選択メニューを示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べ、ヘルパーファイルとテストを含むコードを更新します。
6. 提案されたヘルパーは、最初の引数に `db` クライアントを受け取り、発行者の型付き配列を返す関数です。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約に基づいています。
7. 提案されたコードに TSDoc ドキュメントコメントとファイル先頭のコメントが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示されていなくてもドキュメントコメントを追加する場合があります。その場合も問題ありません。指示を更新した後の動作が一貫することが重要です。

## リポジトリ標準を追加する

`.github/copilot-instructions.md` は、Copilot にプロジェクトレベルの情報を提供するファイルです。コード提案の質を上げるため、リポジトリのコーディング標準を明記しましょう。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。ここにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関する規則はまだありません。そのため、先ほど生成されたヘルパーにはドキュメントコメントがありませんでした。
3. 既存の標準の直後に、次の Markdown 行を追加して、ファイル先頭のコメントと TSDoc ドキュメントコメントを Copilot に求めます。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリレベルの `.github/copilot-instructions.md` は全体に適用する指針に、`*.instructions.md` は特定の言語、ファイル種類、タスクに適用する指針に使えます。今回追加したドキュメントコメントの規則のようなプロジェクト全体の標準には、リポジトリレベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選んで会話を新しくします。
2. `src/lib/publishers.ts` を選択して、Copilot が正しいファイルに注目するようにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントがあることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準を Copilot に伝え、次の演習で使うコードに適用できました。

## 指示とブランチをコミットしてプッシュする

指示ファイルも、ほかのアセットと同じようにソース管理で管理します。指示の更新とヘルパーの変更をリポジトリにコミットしてプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで次を実行し、指示とヘルパーの変更が含まれていることを確認します。

   ```bash
   git status
   ```

3. 指示の更新と、ヘルパーの基礎として Copilot が変更したファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加ファイルが表示された場合は、それらも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、指示の標準に関する Issue に関連付けたプルリクエストを作成します。手動で実行することもできますが、GitHub ツールに接続済みの Copilot に Issue を探し、Issue をクローズする PR を作成して、マージしてもらいましょう。

> [!NOTE]
> Copilot に GitHub での操作を依頼するのは、これが初めてです。プロジェクトのテンプレートでは GitHub ツールがすでにワークスペースに接続されています。Copilot が初めてツールを使うとき、VS Code で GitHub へのサインインを求められることがあります。画面の指示に従ってください。後の演習で、この接続の仕組みである Model Context Protocol (MCP) を学びます。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択メニューで **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関する Issue を探し、現在のブランチから PR を作成するよう Copilot に依頼します。指示の更新と、フィルター機能の準備となる発行者ヘルパーの両方を記載します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を探して PR を作成します。
5. Copilot が GitHub 操作の実行許可を求めたら、コマンドを確認して必要に応じて **Allow** を選択します。
6. PR が作成されたら、次のプロンプトでマージしてローカルブランチを `main` に戻すよう依頼します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot が GitHub 操作とシェルコマンドの実行許可を求めたら、コマンドを確認して必要に応じて **Allow** を選択します。

これで GitHub Copilot の支援を受けてプルリクエストを作成し、マージできました。

## まとめと次のステップ

このプロジェクトの指示ファイルから Copilot がコンテキストを取得する方法を確認し、VS Code の Copilot Chat を使って次の操作を行いました。

- 既存の指示を使ってコード生成を依頼し、Copilot の提案を確認しました。
- `.github/copilot-instructions.md` に新しい標準を追加しました。
- 同じプロンプトを再実行し、提案されたコードが新しい標準に従うことを確認しました。
- 指示とヘルパーの基礎をコミットし、`main` にマージして、次の演習で使えるようにしました。

次は[agent mode][next-lesson]を使い、Copilot にコードベース全体へ新機能を追加してもらいます。

## リソース

- [GitHub Copilot の指示ファイルによるカスタマイズ][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 向けのより良いカスタム指示を書く 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
