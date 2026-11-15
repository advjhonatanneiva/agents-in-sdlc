---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュ コマンド"
description: "コンテキスト管理、モデルの選択、セッションの共有、任意のクラウドへの委任に使うスラッシュ コマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

優れた CLI ツールと同様、GitHub Copilot CLI には対話に使えるスラッシュ コマンドが数多くあります。高度な機能、内部情報、追加の構成オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などのコマンドを使いました。ここでは、ほかの便利なコマンドをいくつか確認します。

このレッスンでは、次の作業を行います。

- `/context` と `/compact` を使い、Copilot が会話のコンテキストを管理する仕組みを確認する。
- `/model` を使い、利用可能なモデルを確認する。
- `/share` でセッションをエクスポートまたは共有する方法を学ぶ。
- 並列作業、worktree、cloud agent への委任に使う任意のコマンドを確認する。

## シナリオ

CLI の主なワークフローを終えました。次は、コンテキスト管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent]への任意の委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクに取り組むと、モデルの最大コンテキスト ウィンドウに達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。また、スラッシュ コマンドを使って状態を確認したり、自分で圧縮したりできます。

1. Codespace に戻り、Copilot CLI がまだ開いていない場合はリポジトリのルートから起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域にコンテキストがどのように分かれているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して結果を比較します。会話が短い場合は、大きな違いがないこともあります。

> [!NOTE]
> コンテキスト ウィンドウが埋まると、Copilot CLI は自動的にコンテキストを圧縮します。自分のタイミングで圧縮する場合は `/compact` を使います。無関係なタスクに切り替えて新しい会話を始める場合は、`/clear` または `/new` を使います。

## モデルを選ぶ

モデルごとに得意分野が異なり、開発者によって好みも異なります。Copilot CLI では、利用可能なモデルを一覧表示して選択できます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況に関する情報を確認します。
3. 現在のモデルを使うか、別のモデルを選択します。または <kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力して学んだことを共有すると、人工知能 (AI) ツールの活用方法を互いに改善できます。`/share` コマンドを使うと、セッションを Markdown または HTML ファイルにエクスポートしたり、共有可能なリンクを作成したり、GitHub gist を公開したりできます。

1. `/help` を入力し、インストール済みバージョンの `/share` オプションを確認します。
2. セッションを共有する場合は、ローカルの Markdown 形式で出力する `/share file` など、目的に合った方法を選びます。
3. エクスポートした内容を、送信または公開する前に確認します。セッションのエクスポートには、プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有を意図していないリポジトリや会話の内容は公開しないでください。

## 任意: 作業の並列化や委任

ワークショップの主要な内容は完了です。Copilot CLI には、より大きなタスク向けのコマンドもあります。

- `/fleet` は、独立したサブタスクをサブエージェントに分割し、並行して実行できます。
- `/worktree` は、別のタスク用に独立した Git worktree を作成できます。
- `/delegate` は、タスクを Copilot cloud agent に送信できます。cloud agent は非同期に作業し、プル リクエストを開く場合があります。

これらのコマンドは、追加の worktree やリモート作業が発生する可能性があるため、任意です。試す場合は、範囲を明確にした新しいタスクから始め、通常のワークフローに沿って結果をレビューしてください。非同期エージェントの作業をさらに確認する場合は、[cloud agent ワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI のスラッシュ コマンドを使って、構成、セッションの共有、内部動作の確認を行いました。このレッスンでは、次の作業を行いました。

- `/context` と `/compact` を使い、Copilot が会話のコンテキストを管理する仕組みを確認した。
- `/model` を使い、利用可能なモデルを確認した。
- `/share` でセッションをエクスポートまたは共有する方法を学んだ。
- 並列作業、worktree、cloud agent への委任に使う任意のコマンドを確認した。

Copilot CLI には、ほかにも多くのスラッシュ コマンドがあります。これまでの内容を振り返り、[次のステップを確認して][next-lesson]、学習を続けましょう。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
