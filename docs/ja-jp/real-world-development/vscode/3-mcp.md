---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: カスタム エージェント →][next-lesson]

agent mode で絞り込み機能を作成しました。プル リクエストを開く前に、ブラウザーで実際に動作することを確認します。自分でアプリを操作する代わりに、**Playwright Model Context Protocol (MCP) サーバー**を接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開してプル リクエスト (PR) を開きます。

この演習では、次の作業を行います。

- Model Context Protocol (MCP) と MCP サーバーが Copilot にツールを追加する仕組みを学ぶ。
- Playwright MCP サーバーをワークスペースに追加する。
- Copilot に Playwright MCP を使わせ、ブラウザーで絞り込み機能を手動テストする。
- ブランチを公開し、絞り込み機能のプル リクエストを開く。

## Model Context Protocol (MCP) とは

agent mode はエディターの外部にアクセスできると、さらに便利になります。Copilot が外部のツールやサービスを利用するために使う標準的な仕組みが Model Context Protocol (MCP) です。

![agent mode の仕組みと、MCP サーバーや VS Code 拡張機能が提供するツールを含めたコンテキスト、大規模言語モデル (LLM)、ツールとの連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は、人工知能 (AI) エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、AI エージェントは外部のツールやサービスとリアルタイムで通信できます。リソースを使って最新情報にアクセスしたり、ツールを使って開発者に代わって操作したりできます。

これらのツールやリソースには、人工知能 (AI) エージェントと外部のツールやサービスをつなぐ MCP サーバーを介してアクセスします。MCP サーバーは、AI エージェントと外部ツール (既存のアプリケーション プログラミング インターフェイス (API) や NPM パッケージなどのローカル ツール) 間の通信を管理します。MCP サーバーごとに、AI エージェントが利用できるツールとリソースが異なります。

よく使われる MCP サーバーをいくつか紹介します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリの管理 API を利用できます。新しいリポジトリの作成、既存リポジトリの更新、Issue やプル リクエストの管理などを実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright によるブラウザー自動化機能を提供します。Web ページへの移動、フォームの入力、ボタンの選択などを実行できます。

さまざまなツールやリソースを提供する MCP サーバーがほかにもあります。GitHub は、エコシステム内の MCP サーバーを見つけやすくし、貢献を促進するために [MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーもプロジェクトのほかの依存関係と同様に扱ってください。使用前にソース コードを慎重にレビューし、公開元を確認して、セキュリティへの影響を検討してください。信頼できる MCP サーバーだけを使い、機密リソースや操作へのアクセスを許可する際は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースで利用する MCP サーバーを構成します。

1. codespace で `.vscode/mcp.json` を開きます。
2. すでに `github` サーバーが構成されていることを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` エントリはプロジェクト テンプレートに含まれているため、前の演習で Copilot はバックログの Issue を確認できました。これは[リモート GitHub MCP サーバー][remote-github-mcp-server]を使います。ローカルにインストールする必要はありません。VS Code が HTTP 経由で接続し、Copilot が初めてツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に 2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]は Copilot が操作できるブラウザーを提供するため、機能のテストに役立ちます。

1. `.vscode/mcp.json` で `github` と並べて `playwright` エントリを追加し、ファイルが次のようになるようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` は**ローカル** サーバーです。VS Code が `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグは Playwright を画面のないヘッドレス モードで実行します。デスクトップ環境がない codespace では、この設定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトはエンドツーエンド テストにすでに Playwright を使っているため、通常は必要なブラウザーもインストール済みです。Copilot がブラウザーが見つからないと報告した場合は、`npx playwright install chromium` を実行するよう依頼して、もう一度試してください。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて起動**します。Copilot がサーバーのツールを初めて必要としたときに VS Code が起動し、サーバーを信頼するか確認します。

1. `.vscode/mcp.json` で `playwright` エントリの上に **Start** が表示されます。選択してサーバーを起動し、構成が正しいことを確認します。
2. VS Code からサーバーを信頼するか確認された場合は、構成をレビューして信頼することを選択し、サーバーを起動します。

手動での起動は任意です。省略した場合は、次の手順で Copilot がツールを必要としたときに VS Code がサーバーを起動し、信頼の確認を表示します。

## 絞り込み機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。開発者がアプリを操作して確認する代わりに、エージェントがページを開き、移動し、フィルターを適用して、その結果を読み上げ、確認内容をまとめます。会話を離れずに機能を確認できるため、効率的です。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティ ツリー][playwright-mcp-server]を使います。エージェントはボタン、リンク、リスト項目などの構造化されラベル付けされた要素を基に判断します。支援技術と同じように動作するため、簡単な機能チェックがアクセシビリティの簡易確認にもなります。

サーバーが接続され、アプリが実行中であることを確認したら、次の手順で絞り込み機能を試すよう Copilot に依頼します。

1. 次のプロンプトを Copilot に送信します。

   ```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
   ```

Copilot は Playwright MCP サーバーを通じてブラウザーを起動し、各手順を実行して結果を報告します。報告内容を Issue の受け入れ条件に照らし合わせます。問題がある場合は、プル リクエストを開く前に追加の質問をするか、コードの修正を依頼します。

> [!NOTE]
> テストするには、アプリが `http://localhost:4321` で実行されている必要があります。開発サーバーを停止した場合は、プロンプトを送る前にもう一度起動してください。Copilot が Playwright MCP サーバーを初めて使うときにブラウザーのダウンロードが必要な場合があります。ブラウザーが見つからないと報告されたら、`npx playwright install chromium` を実行するよう依頼して、再試行してください。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプル リクエストを作成する

機能が動作することを確認できたら、チームのレビュー用にプル リクエスト (PR) を開きます。まず `filtering-vscode` ブランチを公開します。

1. codespace の **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを選択してコミット メッセージを生成します。

   ![変更内容が表示された Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して、ブランチをリポジトリにプッシュします。

## プル リクエストを作成する

github.com や GitHub コマンドライン インターフェイス (CLI) など、プル リクエストを作成する方法はいくつかあります。GitHub Copilot を使っているので、PR の作成も依頼しましょう。Copilot は関連する Issue を見つけ、その Issue に関連付けた PR を作成できます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. GitHub ツールを使えるよう、エージェント ドロップダウンで **Agent** が選択されていることを確認します。
3. 次のプロンプトを使って PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Copilot が情報を収集して操作を実行するために必要な場合は、**Continue** を選択して許可します。GitHub ツールを初めて使うときは認証を求められることがあります。画面の指示に従って許可してください。
5. Copilot が Issue を検索し、該当するものを見つけて PR を作成することを確認します。
6. Copilot が作成したリンクを選択して PR をレビューします。ただし、まだマージしないでください。

## まとめと次のステップ

おめでとうございます。この演習では、次の作業を行いました。

- Model Context Protocol (MCP) と MCP サーバーが Copilot にツールを追加する仕組みを学んだ。
- Playwright MCP サーバーをワークスペースに追加した。
- Copilot にブラウザーを操作させ、リリース前に絞り込み機能をテストした。
- ブランチを公開し、絞り込み機能のプル リクエストを開いた。

機能のテストに MCP サーバーを使いましたが、MCP は Copilot を拡張する方法の 1 つです。次は、アクセシビリティ レビューなどの作業を効率化する[カスタム エージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP レジストリ][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
