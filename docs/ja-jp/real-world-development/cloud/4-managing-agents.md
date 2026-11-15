---
title: "演習 4 - エージェントを監視して管理する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の 2 つの演習では、Copilot cloud agent にユーザー エクスペリエンスの改善と機能追加に関する 3 つのタスクを依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクを管理するためのツールはいくつかあります。その 1 つが、GitHub.com の[エージェント ページ][agents-page]です。この Mission control では、オープンなプル リクエスト (PR) があるすべてのエージェント タスクを確認できます。実行内容を調べ、進行中のセッションに指示を加えることもできます。

このレッスンでは、次の作業を行います。

- エージェント ページを使って cloud agent のタスクを監視する。
- 進行中のセッションに指示を加えて、機能を追加する。

## シナリオ

ハイ コントラスト モードの作成をエージェントに割り当てた後、チームはライト モードも追加するよい機会だと考えました。すでにサイトのスタイルを更新し、切り替え機能を追加しているため、一緒に実装するのが合理的です。エージェントに指示を追加し、ハイ コントラストに加えてライト モードも実装してもらいます。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メイン ペインと左ペインの両方に表示されるタスク一覧を確認します。次のようなタスクが割り当てられているはずです。
   - コードのドキュメントを更新する。
   - ゲーム変更用のアプリケーション プログラミング インターフェイス (API) を生成する。
   - Web サイトにハイ コントラスト モードを追加する。
3. 実行中のタスクを選択します。Copilot が実行した次のような操作を確認します。
   - リポジトリからコードをチェックアウトする。
   - Copilot の作業環境を作成する。
   - MCP サーバーを設定する。
   - 割り当てられたタスクを完了するための作業を行う。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や進め方によって異なります。

4. 右側に表示されるプル リクエスト (PR) ペインも確認します。PR と変更されたファイルを確認し、作業を監視できます。

## Cloud agent に指示する

アクティブなタスクを確認できたので、cloud agent がハイ コントラスト モードに取り組んでいる間にライト モードの切り替えも追加するよう依頼します。

1. ハイ コントラスト モードを追加するセッションを選択します。Copilot が付けた名前や作業状況によって、タイトルは異なります。

    ![Mission control に表示されたアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セッションを数分間確認し、セットアップが完了して作業が始まるまで待ちます。次のようなメッセージが表示されたら作業中です。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![エージェント ページの Copilot タスクで、作業中のセッションへの指示ダイアログが強調表示されたスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に反映することを確認します。

## Copilot に作業を続けてもらう

前回と同じように、Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で実行していた手順が終わると、新しい要望を作業に組み込みます。

前と同じく、完了まで数分かかります。これまでに学んだことを振り返りましょう。

## まとめと次のステップ

このレッスンでは、Copilot agents ページを確認しました。このページは、GitHub Copilot cloud agent のタスクを監視して指示するための中心的な場所です。アクティブなタスクと完了済みタスクを追跡し、実行中の作業を確認し、作業中のタスクに指示を加えて調整できます。

次の内容を確認しました。

- Mission control とエージェント ページを使って cloud agent のタスクを監視した。
- 進行中のセッションに指示を追加し、機能を依頼した。

Copilot がアクセシビリティ機能を完了したら、[Copilot が作成したプル リクエストを反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
