---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。作業の方法を Copilot に伝える必要がある場合、その指針を参照できるようにしましょう。[指示ファイル][instruction-files]では、必要なコードの「内容」だけでなく、コードの「構成方法」も伝えられます。フィルター機能を実装したので、Copilot が参照した指示を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次の操作を行います。

- リポジトリの指示とパス別の指示ファイルがエージェントに適用される方法を確認します。
- コーディング標準が守られるように指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発プロセスに関するガイドラインと要件があります。たとえば、次のとおりです。

- コメントにはコードの内容を言い換えるのではなく、目的や、ひと目でわからない判断の理由を記載します。
- `db/` と `src/lib/` のエクスポート関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc コメントを記載します。`db` 引数を注入できる場合は、その引数についても記載します。
- 再利用可能な Astro コンポーネントには `Props` の契約を記載し、関連するコードを変更した場合はコメントも最新の状態にします。
- 既存のフォーマットと lint の指針を維持します。

指示ファイルを使って、Copilot がこれらの開発方針に沿って作業できるようにします。

## 指示ファイル

カスタム指示では、コーディングスタイルや要件に関するコンテキストと設定を Copilot に伝えます。コーディング規約、ライブラリ、コメントの書き方などを指定して、より適切な提案やコードスニペットを得るために利用できます。リポジトリ全体、または特定の種類のファイルに対する指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に渡される単一の指示ファイルです。ほとんどのリクエストに関係するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種類、タスクに関する指針を記載するファイルです。

> [!NOTE]
> ほかの形式の指示ファイルへの対応状況は環境によって異なります。特定の形式を使用する前に、[カスタム指示の対応状況][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スターター プロジェクトには、あらかじめ指示ファイルが用意されています。変更前に内容を確認して、指示がコードに与える影響を見てみましょう。

1. Codespace に戻ります。
2. Codespaces のエディターで（ターミナルではなく）`.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディング指針を確認します。この指示は、このリポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データレイヤー、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象のファイルを決める glob を指定しています。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルや既存のプロジェクトファイルへのリンクを確認します。大きな指示を再利用できる小さなファイルに分け、コード生成の例を Copilot に示す方法がわかります。

## チームの指針に合わせて指示ファイルを更新する

既存の指示はよい出発点ですが、追加できる項目があります。中心となる `copilot-instructions.md` を変更して、新しく生成する TypeScript に TSDoc コメントを追加するようにします。

1. `.github/copilot-instructions.md` で、35 行目付近にある **Code formatting guidance** セクションを探します。
2. 最後の箇条書きとして、次の行を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新した指針を使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。フィルター機能のコンテキストを保ったまま新しい指示を読み込むため、会話を終了して再開します。

1. フィルターの会話がまだ開いている場合は、`/exit` を入力して閉じます。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートにある `game-filters-cli` ブランチから、保存済みの会話を再開します。

   ```bash
   copilot --resume
   ```

   表示された場合はフィルターの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 更新した指針を適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードの説明として正確であることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを取得する方法を確認し、機能に新しい標準を適用しました。具体的には、次の操作を行いました。

- リポジトリの指示とパス別の指示ファイルがエージェントに適用される方法を確認しました。
- コーディング標準が守られるように指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は再利用可能な `quality-checks` スキルを[カスタマイズして実行し][next-lesson]、lint とテストを一貫して実行できるようにします。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示の対応状況][custom-instructions-support]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
