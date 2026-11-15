---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせる QA カスタム エージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルを使って自動チェックを実行し、Playwright Model Context Protocol (MCP) でブラウザー上の絞り込み機能を確認しました。ここでは、明確な品質保証 (QA) プロセスを持つカスタム エージェントに、それらの機能をまとめます。

このレッスンでは、次の作業を行います。

- カスタム エージェントが指示、スキル、MCP ツールと連携する仕組みを確認する。
- 再利用可能な品質保証プロファイルを作成して確認する。
- QA エージェントを選択し、絞り込みの Issue に照らして報告内容をレビューする。

## シナリオ

Tailspin Toys では、プル リクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テスト カバレッジ、ブラウザー上の動作を一貫した方法でレビューしたいと考えています。カスタム エージェントを使うと、その QA プロセスを調整し、再利用できるレポートを作成できます。

## カスタム エージェントとは

カスタム エージェントは、Markdown プロファイルで定義する、Copilot の特化バージョンです。プロファイルにはエージェントの目的、指示、利用可能なツールを記述します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの指示はチームの標準を示し、`quality-checks` スキルは繰り返し実行できるチェックをまとめます。Playwright MCP はブラウザー ツールを提供します。QA プロファイルは、それらを使って要件を評価し、結果を報告するよう Copilot に指示します。これらの機能を置き換えるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能のプル リクエスト (PR) を開く前に、再利用できる QA プロファイルの作成を Copilot に依頼します。プロファイルには、QA が実施するチェックと、その作業範囲の両方を定義します。

1. Codespace に戻り、絞り込み機能の会話が Interactive モードであることを確認します。
2. 既定のエージェントに、次のカスタム エージェントを作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、Copilot が意図した QA の手順と作業範囲をプロファイルに反映したことを確認します。これにより、検証だけを行うつもりなのにエージェントが機能を変更してしまうことを防げます。

1. `/diff` を入力して `.github/agents/qa.agent.md` を開きます。
2. frontmatter を読みます。`description` は必須で、`name` は任意ですが、含めるとエージェント名が分かりやすくなります。
3. プロファイルの指示を読み、QA が要件を起点にし、リポジトリの指示に従い、`quality-checks` スキルと Playwright MCP を使うことを確認します。
4. QA が根拠を示して報告し、実装コードを変更する前に確認を求め、変更をコミットしたりプル リクエストを開いたりしないことを確認します。
5. 必要な役割や制約が抜けている場合は、続行する前に既定のエージェントに修正を依頼します。

## Issue を使って QA を実行する

Copilot CLI は会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルの作成後、同じ絞り込み機能の会話を再開して QA を選択すると、Issue と計画の判断を引き継げます。

1. 次のプロンプトを使ってエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成したばかりのエージェントは、利用可能なエージェントの一覧にまだ表示されない場合があります。次のコマンドで有効にできます。

2. 次のプロンプトを使って、QA エージェントに機能をレビューするよう依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントの作業を確認します。
4. 作業が完了したら、レポートを読みます。

## まとめと次のステップ

再利用可能な専門的役割をワークフローに追加し、その成果をレビューしました。このレッスンでは、次の作業を行いました。

- カスタム エージェントが指示、スキル、MCP ツールと連携する仕組みを確認した。
- 再利用可能な品質保証プロファイルを作成して確認した。
- QA エージェントを選択し、絞り込みの Issue に照らして報告内容をレビューした。

実装、スキルの更新、QA プロファイル、テスト、検証レポートが揃いました。次は、[それらを機能の PR にまとめ、Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用のカスタム エージェントを作成する][create-agents]
- [カスタム エージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
