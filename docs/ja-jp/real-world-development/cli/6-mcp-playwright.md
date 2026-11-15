---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または構成し、ブラウザーで絞り込み機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

すでに説明したように、コードを書くことだけが開発ではありません。データや外部サービスを扱い、Copilot が利用できる自動化機能を追加することもあります。そこで Model Context Protocol (MCP) サーバーが役立ちます。MCP サーバーを使うと、Copilot は CLI に組み込まれている機能を超えて、さらに多くのツールやサービスを利用できます。

このレッスンでは、次の作業を行います。

- Model Context Protocol (MCP) と Copilot CLI での利用方法を理解する。
- 利用できない場合は Playwright MCP サーバーを追加する。
- エージェントにブラウザーを操作させ、絞り込み機能を試す。

## シナリオ

ユニット テストやエンドツーエンド テストは重要ですが、ユーザー インターフェイス (UI) の更新を検証するには、実際に UI を操作する必要があります。ユーザーと同じように作業中の Web サイトを Copilot が使えるようにすると、変更の自動化をさらに進め、期待どおりの動作に対する信頼を高められます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、人工知能 (AI) エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、AI エージェントは外部のツールやサービスとリアルタイムで通信し、最新情報にアクセスしたり、開発者に代わって操作を実行したりできます。

これらのツールやリソースには、人工知能 (AI) エージェントと外部のツールやサービスをつなぐ MCP サーバーを介してアクセスします。MCP サーバーごとに、AI エージェントが利用できるツールとリソースが異なります。

よく使われる MCP サーバーをいくつか紹介します。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、Issue、プル リクエストを管理するアプリケーション プログラミング インターフェイス (API) にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を提供します。

ほかにも多くの MCP サーバーがあります。GitHub は、エコシステム内の MCP サーバーを見つけやすくし、貢献を促進するために [MCP レジストリ][mcp-registry]をホストしています。

> [!CAUTION]
> MCP サーバーも、プロジェクトのほかの依存関係と同様に扱ってください。使用前にソース コードをレビューし、公開元を確認して、セキュリティへの影響を検討してください。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は、追加して Copilot がユーザーと同じように Web サイトを操作できるようにします。

> [!NOTE]
> Copilot CLI で Playwright MCP がすでに構成されている場合は、セットアップ手順を省略して次のセクションに進んでください。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバーの追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前として `playwright` を入力し、<kbd>Tab</kbd> を押します。
4. サーバーの種類が **STDIO** であることを確認し、<kbd>Enter</kbd> を押してから <kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright で機能を試すよう Copilot に依頼する

これまでは機能が期待どおりに動作することを手動で確認しました。今度は Copilot が利用できる Playwright MCP サーバーを使って、同じ確認を行います。

1. 次のプロンプトを使い、Playwright MCP サーバーで機能を検証するよう Copilot に依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常、Copilot は指示しなくても MCP サーバーを使う方法を判断できます。ただし、使用するツールが分かっている場合は、明示すると結果の一貫性を高め、トークンを節約できます。

2. Copilot がブラウザー上で機能の動作を確認する手順を表示する様子を確認します。
3. レポートを読み、すべてが期待どおりに動作することを確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作してからサーバーを停止し、結果を報告します。

## まとめと次のステップ

おめでとうございます。Playwright MCP サーバーを使い、Copilot CLI から実際のブラウザーで機能を確認しました。このレッスンでは、次の作業を行いました。

- Model Context Protocol (MCP) と Copilot CLI での利用方法を学んだ。
- 利用できない場合は Playwright MCP サーバーを追加した。
- エージェントにブラウザーを操作させ、絞り込み機能を試した。

次は、スキルとブラウザー ツールをまとめた[品質保証 (QA) カスタム エージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
