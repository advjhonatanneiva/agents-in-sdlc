---
title: "レッスン 8 - 機能のプル リクエスト (PR) を作成してマージする"
description: "絞り込み機能、指示、スキルの更新、品質保証 (QA) プロファイル、テストをまとめてレビューし、プル リクエストを作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

絞り込み機能の実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストはすべて 1 つのブランチに保存されています。まとめてレビューし、プル リクエストを開く段階です。星評価のプル リクエスト (PR) は自分でマージしましたが、今回は **Agent Merge** にプロセスを管理させます。

> [!NOTE]
> 通常は、機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。このワークショップでは手順を簡潔にするため、絞り込み機能と品質ワークフローを 1 つの会話とブランチにまとめ、この PR に含めています。

このレッスンでは、次の作業を行います。

- Agent Merge と、マージ ライフサイクルを自動化する仕組みを学ぶ。
- 機能全体の変更と検証の根拠を確認する。
- 絞り込み機能の PR を作成する。
- レビュー後にのみ Agent Merge を有効にして、PR がマージされたことを確認する。

## シナリオ

絞り込み機能の開発では、Copilot を使って計画、実装、検証を行いました。Tailspin Toys では残りの PR 作業を自動化しつつ、マージの許可は開発者が管理したいと考えています。

## Agent Merge について

**Agent Merge** は、プル リクエストをマージするために必要な残りの作業を自動化します。有効にすると、Copilot は PR を妨げている問題 (失敗した継続的インテグレーション (CI) チェックの修正、レビュー コメントへの対応、必要に応じた rebase) に取り組み、リポジトリで許可されていれば GitHub の自動マージを有効にします。

これまでは自分で **Merge pull request** を選択していました。Agent Merge にこの作業を任せることもできます。Agent Merge を有効にする前に作業をレビューし、準備ができていることを確認してください。

## Agent Merge で PR を管理する

作成したコードをまとめてレビューし、PR を作成して、残りのプロセスを Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェント ダイアログを開きます。
3. 選択肢から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` を使って Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーション (CI) を監視します。すべて成功すると、マージが実行されます。
7. Copilot から `"PR #14 was squash-merged successfully."` のようなメッセージが届いたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージ キュー、リポジトリ設定、権限の不足を回避しません。処理がブロックされた場合は報告された理由を確認し、リポジトリで許可されていればレビュー済みの変更を手動でマージしてください。

## まとめと次のステップ

コードの生成、テストと検証、プル リクエストのプロセスなど、開発作業の一部を自動化しました。このレッスンでは、次の作業を行いました。

- Agent Merge と、マージ ライフサイクルを自動化する仕組みを学んだ。
- 機能全体の変更と検証の根拠を確認した。
- 絞り込み機能の PR を作成した。
- レビュー後にのみ Agent Merge を有効にして、PR がマージされたことを確認した。

次は、コンテキスト、モデル、共有、任意のクラウドへの委任に使える[便利な Copilot CLI スラッシュ コマンドを確認します][next-lesson]。

## リソース

- [Copilot CLI でプル リクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
