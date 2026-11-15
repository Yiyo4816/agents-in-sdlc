---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。作業の方法や Copilot が知っておくべき背景情報がある場合、その情報にアクセスできるようにしましょう。[指示ファイル][instruction-files]を使うと、Copilot に「何を」するかだけでなく、「どのように」作業するかも伝えられます。

この演習では、次の操作を行います。

- リポジトリのカスタム指示とパス別の指示ファイルを使って、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- リポジトリ全体に適用する標準を `.github/copilot-instructions.md` に追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、この演習ではプロンプトの「変更前と変更後」を比較しません。Copilot cloud agent は GitHub Issue に対して非同期で作業するため、その影響をリアルタイムで並べて示すのが難しいためです。後の演習で cloud agent が作成したプルリクエストをレビューするときに、指示ファイルの効果を確認できます。

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

## リポジトリ標準を追加する

この演習で行う編集は、コード内のドキュメントに TSDoc ドキュメントコメントとファイル先頭のコメントを使うという、プロジェクト全体の規則の追加です。後の演習で Issue を割り当てると、cloud agent がこの規則を参照します。

編集の前に作業ブランチを作成します（cloud agent は Issue の対象ブランチにある指示を読みますが、ここでは変更をきちんとコミットします）。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を提供するファイルです。コード提案の質を上げるため、リポジトリのコーディング標準を明記しましょう。

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

## 指示の変更をコミット、プッシュ、マージする

cloud agent は、Issue の対象ブランチにある指示ファイルを読み込みます。次の演習では Copilot を `main` に割り当てるため、cloud agent が新しい指示を使えるように、変更を `main` にマージする必要があります。

1. ステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成し、マージします。次の演習で Issue を割り当てると、cloud agent がこの指示を読み取ります。

> [!TIP]
> このワークショップで `main` に直接作業したい場合は、ブランチを作成せず、`main` に直接コミットしてもかまいません。ここでは実際のプロジェクトでの作業に近づけるためにブランチを使っています。

## まとめと次のステップ

このプロジェクトの指示ファイルが Copilot にコンテキストを伝える方法を確認し、リポジトリ全体に適用する標準を `.github/copilot-instructions.md` に追加しました。残りのハーネスで cloud agent が作成するプルリクエストを確認すると、この標準が反映されていることがわかります。

次は、最初の Issue を [Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot の指示ファイルによるカスタマイズ][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 向けのより良いカスタム指示を書く 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
