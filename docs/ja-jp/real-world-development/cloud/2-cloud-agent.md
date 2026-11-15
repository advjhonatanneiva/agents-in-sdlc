---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

多くの組織は、技術的負債の問題を抱えています。解決されていないセキュリティの問題、更新が必要なレガシー コード、実装する時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot の cloud agent は、コードの更新や機能の追加などのタスクを自律的に実行します。作業が完了すると、人間の開発者がレビューできるドラフト プル リクエスト (PR) を作成します。これにより、手間のかかるタスクを委任して開発プロセスを加速し、開発者はより大きな課題に集中できます。

Copilot cloud agent を使って、次の内容を確認します。

- コード生成環境をカスタマイズした。
- 操作が安全に行われるようにした。
- Issue の範囲を明確に設定することの重要性を理解した。
- Issue を Copilot に割り当てた。

## シナリオ

Tailspin Toys には解決したい技術的負債があります。サイトの初期バージョンを作った請負業者が、ドキュメントを適切に整備しませんでした。実際、ドキュメントがまったくありません。最初の作業として、アプリケーション内のすべてのエクスポート関数に TypeScript 用のドキュメント コメント (TSDoc) を追加したいと考えています。

さらに、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属するほかのタイトル、つまり関連ゲームを表示します。完成度の高い実装はまだ必要なく、ユーザー エクスペリエンス (UX) の受け入れテストに使えるものを求めています。この作業は現在ブロッカーですが、優先度の高い Issue がほかにあります。

これらは、優先度が下がりやすいタスクの例です。Copilot cloud agent に割り当てると、非同期に作業を進められます。開発者は別の作業に集中し、後から Copilot の作業をレビューして期待どおりか確認できます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドでタスクを実行できます。たとえば、[GitHub Issue を Copilot に割り当てる][assign-issue]方法があります。割り当てると、Copilot は進捗を追跡するドラフト プル リクエスト (PR) を作成し、環境を設定してタスクに取り組みます。進行中でも完了後でも、Copilot のセッションを確認できます。提案された解決策のレビュー準備ができると、プル リクエストで開発者に通知します。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な操作でも、詳しく見ると複雑なことがあります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。人工知能 (AI) のペア プログラマーとして扱い、段階的に作業し、学び、試し、必要に応じて調整します。生成 AI を利用しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習ではカスタム指示と、その指示が Copilot を導く仕組みを確認しました。カスタム指示の演習をすでに行っていれば、`.github/copilot-instructions.md` の動作も見たことがあるでしょう。まだの場合は、今のうちに確認してください。Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認して、後でその影響を判断できるようにします。

リポジトリの指示ファイルが後の Copilot の作業にどう反映されるか確認するため、GitHub の Web ユーザー インターフェイス (UI) または起動済みの codespace で次のファイルを開きます。

1. `.github/copilot-instructions.md` を開き、**Code standards** セクション、特に TypeScript の規約と TypeScript 用のドキュメント コメント (TSDoc) に関する要件を確認します。
2. `.github/instructions/unit-tests.instructions.md` を開き、`applyTo` frontmatter を確認します。この指示は `**/*.test.ts` ファイルに適用されます。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てるとき、作成されたプル リクエストに指示ファイルが反映され、TSDoc のドキュメント コメント、TypeScript の規約、コメント ヘッダーが追加されているか確認してください。後の演習で PR をレビューするときにも取り上げます。

## Copilot cloud agent 用の開発環境を設定する

コードの作成には、通常、適切な環境と、状態を整えるためのセットアップ スクリプトが必要です。ソフトウェア エンジニア (SWE) と同じようにタスクを実行する Copilot に作業を割り当てる場合も同様です。

Cloud agent は作業環境に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップ ワークフロー][setup-workflow]を作成して環境をカスタマイズすると、必要な開発ツールや依存関係を使えるようになります。ラボをスムーズに進められるよう、事前に構成済みです。Copilot が Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー バイナリ、単一 Astro アプリ用にマイグレーションと初期データを設定した SQLite データベースを利用できるようにします。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 手順に必要な最小限の権限を設定します。Copilot には操作用の独自トークンが付与されます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順の一部としてリポジトリをクローンする場合は、`contents: read` 権限が必要です。手順でクローンしない場合は、完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストでデータを利用できるように、ローカルの SQLite データベースをマイグレーションして初期データを投入します。
      - name: Set up the database
        run: npm run db:setup
```

ほかの GitHub ワークフローと同じ形式ですが、いくつか重要な点があります。

- `copilot-setup-steps` という単一のジョブが含まれています。このジョブは、Copilot がプル リクエストに取り組む前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーが設定されています。このトリガーを使うと、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot が実行するのを待たずに、ワークフローが成功することを確認するのに便利です。

## ドキュメントを追加する

ドキュメントの重要性は広く認識されていますが、多くのプロジェクトでは情報が古かったり、ドキュメントそのものがなかったりします。この種の技術的負債は、開発の効率を下げ、コードベースの保守や新しい開発者の受け入れを難しくします。Copilot はドキュメント作成が得意なため、cloud agent に割り当てるのに適した Issue です。バックグラウンドで必要なドキュメントを作成します。後の演習で作業をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して Issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

   ![Issue への Copilot の割り当て](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく生成されたプル リクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のような内容です。一覧に新しい PR が表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が To Do リストを作成したことを確認します。

> [!NOTE]
> Copilot の To Do リストが PR に表示されるまで、数分かかることがあります。Copilot が環境を作成し、前述のワークフローを実行して、プロジェクトを分析し、問題に取り組む方法を決めています。

13. リストを確認し、Copilot が実行するタスクを把握します。
14. プル リクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot のセッション ビュー](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 表示が更新されない場合は、ウィンドウを更新してください。

16. セッションをスクロールし、Copilot が問題に取り組む様子を確認します。コードを調査して状態を把握し、計画を考えて判断し、コードを作成します。

作業には数分かかる場合があります。Copilot cloud agent の主な目的は、非同期にタスクを実行して、開発者が別の作業に集中できるようにすることです。この機能を活用し、もう 1 つのタスクも Copilot cloud agent に割り当ててから、アプリの機能を追加するコード作成に取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

前述のとおり、GitHub Copilot cloud agent の利点の 1 つは作業を分担できることです。デザイン チーム向けに関連ゲームのセクションを作成するのに長い時間がかからないとしても、その時間をほかの作業に充てられます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して、空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

   ![Issue への Copilot の割り当て](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Copilot が作業を開始したことを示す 👀 が Issue の最初のコメントに表示されます。

![Copilot が Issue に取り組んでいることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組んでいます。Copilot cloud agent はソフトウェア エンジニアと同じように作業するため、常に監視する必要はなく、完了後にレビューできます。次は、カスタム エージェントの作成と使用を確認します。

## まとめと次のステップ

このレッスンでは [GitHub Copilot cloud agent][copilot-agents]を確認しました。cloud agent に Issue を割り当てると、非同期で作業を進められます。技術的負債への対応、新機能の作成、コードの別フレームワークへの移行などを Copilot に任せられます。

次の内容を確認しました。

- コード生成環境をカスタマイズする。
- 操作が安全に行われるようにする。
- Issue の範囲を明確に設定することの重要性を理解する。
- Issue を Copilot に割り当てる。

Copilot cloud agent がバックグラウンドで作業している間に、次はカスタム エージェントの作成と使用を確認します。[Copilot cloud agent は Model Context Protocol (MCP) サーバーも利用でき][cloud-agent-mcp]、前のモジュールで扱ったカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
