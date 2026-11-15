---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の演習では、Copilot cloud agent にユーザー体験の改善や機能追加につながる 3 つのタスクを依頼しました。cloud agent は非同期かつ自律的に作業しますが、進行中のタスクを監視することも重要です。

cloud agent に割り当てたタスクを管理する方法はいくつかあります。その 1 つが GitHub.com の[エージェントのページ][agents-page]です。ここでは、オープンなプルリクエスト (PR) があるエージェントのタスクを確認できます。実行内容を調べたり、進行中のセッションに指示を出したりできます。

このレッスンでは、次の操作を行います。

- エージェントのページで cloud agent のタスクを監視します。
- 進行中のセッションに追加機能を依頼します。

## シナリオ

ハイコントラストモードの作成をエージェントに依頼した後、チームはライトモードも追加するとよいと気付きました。サイトのスタイルと切り替え機能を更新しているため、同じ作業に含めるのが自然です。エージェントに指示を出して、ハイコントラストに加えてライトモードも追加してもらいましょう。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの現在の状態を確認します。

1. [Copilot agents のページ](https://github.com/copilot/agents)に移動します。
2. メイン領域と左側の領域にタスクの一覧が表示されます。次のような、Copilot に割り当てたタスクがあることを確認します。
   - コードベースのドキュメントを更新する
   - ゲームを変更する API を生成する
   - Web サイトにハイコントラストモードを追加する
3. 実行中のタスクを 1 つ選択し、Copilot が実行した作業を確認します。たとえば、次のような操作が含まれます。
   - リポジトリからコードをチェックアウトする
   - Copilot の作業環境を作成する
   - MCP サーバーを設定する
   - タスクを完了するための各種操作を実行する

> [!NOTE]
> 表示される手順は、Copilot の作業状況や実行方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) の領域も確認します。ここから PR と変更ファイルを確認し、進行状況をさらに監視できます。

## cloud agent に追加の指示を出す

進行中のタスクを確認したので、ハイコントラストモードの作業中にライトモードの切り替えも追加するよう Copilot に依頼します。

1. ハイコントラストモードの追加に関連するセッションを選択します。タイトルは Copilot が付けた名前や作業状況によって異なります。

    ![ミッションコントロールのアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して作業が始まったことを示すメッセージが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot のエージェントタスク画面で、作業中のセッションに指示を出すダイアログが強調表示されています](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に反映することを確認します。

## Copilot に作業を続けてもらう

前回と同様に、Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で進行中だった手順を終えた後、新しい要望を作業に取り込みます。

今回も数分かかります。その間に、これまでに学んだことを振り返りましょう。

## まとめと次のステップ

このレッスンでは、GitHub Copilot cloud agent の監視と指示を行う中心的な場所である Copilot agents のページを確認しました。ミッションコントロールで、実行中と完了済みのタスクを追跡し、進行中の作業を確認し、タスクの範囲を調整する指示を出せます。

次の操作を行いました。

- ミッションコントロールとエージェントのページを使って、cloud agent のタスクを監視しました。
- 進行中のセッションに追加機能を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、Copilot が作成したプルリクエストを[繰り返し改善する][next-lesson]方法を確認しましょう。

## リソース

- [Copilot agents のページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を繰り返し改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
