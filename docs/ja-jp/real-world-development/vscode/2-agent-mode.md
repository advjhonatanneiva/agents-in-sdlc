---
title: "演習 2 - Copilot agent mode で新機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションに小さな変更を加えるだけでも、通常は複数ファイルの更新やテスト実行などが必要です。開発者は、関連ファイルを探し、変更を加え、テストを実行し、デバッグし、変更し忘れたファイルを見つけて、さらに更新することになります。作業は次々に続きます。

こうした場面で役立つのが Copilot Agent Mode です。

Copilot Agent Mode は、統合開発環境 (IDE) 内でより自律的に作業するよう設計されています。開発者と同じように既存のプロジェクト構成を調査し、必要な更新を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。Agent Mode を使ってサイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる用途向けに作られています。agent mode は IDE 内で動作し、すばやいフィードバックと対話を可能にします。cloud agent はチームの一員のように非同期で作業し、Issue とプル リクエストを通じて開発者と連携します。

この演習では、次の方法を学びます。

- Copilot Agent Mode でプロジェクトを調査し、関連ファイルを特定して、複数のファイルを連携して変更する。
- GitHub Copilot Agent Mode でユーザー インターフェイス (UI) とデータ レイヤーにまたがる機能を実装する。
- Copilot Agent Mode が生成した変更とテストをレビューしてからコードベースにマージする。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリと発行元で絞り込めるようにします。前の演習で発行元ヘルパーを追加しました。ここでは Copilot Agent Mode を使って、残りのデータ レイヤー、ユーザー インターフェイス (UI)、テストを完成させます。

## Tailspin Toys Web サイトを実行する

変更を加える前に、Tailspin Toys Web サイトを確認し、現在の機能を把握します。

この Web サイトは、開発者向けテーマのボードゲームのクラウドファンディング プラットフォームです。ゲームを一覧表示し、詳細を表示できます。単一の Astro アプリで、ビルド時にページを静的なハイパーテキスト マークアップ言語 (HTML) としてレンダリングします。ページは frontmatter で Drizzle のオブジェクト リレーショナル マッピング (ORM) を使ってローカル SQLite データベースを直接参照します。独立したバックエンドのアプリケーション プログラミング インターフェイス (API) やクライアント側 UI フレームワークはありません。再利用可能なデータ アクセス ヘルパーは `src/lib/` にあり、必要な操作性は標準のドキュメント オブジェクト モデル (DOM) API を使った小規模な Astro `<script>` で追加します。

### Web サイトを起動する

Web サイトを簡単に実行できるように、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習後に `main` に戻っているはずです。
2. Codespace 内で新しいターミナルを開くには、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
3. 絞り込み機能用の新しいブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような Uniform Resource Locator (URL) を示すメッセージが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して Web サイトを開きます。

> [!NOTE]
> codespace でターミナルの localhost URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` にリダイレクトされます。これは、Web サーバーを実行している codespace へのプライベート トンネルです。

### Web サイトを確認する

Web サイトを起動したら、機能を確認します。主な機能は次のとおりです。

- **ホーム ページ**: タイトル、画像、説明とともにボードゲームを一覧表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、発行元、カテゴリなどの情報を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めてください。モードとモデルの選択は演習によって異なります。必要な場合は各手順に記載しています。

Web サイトの初期実装は機能しますが、新しい機能を追加して改善します。まずバックログを確認します。テンプレートからプロジェクトを設定すると Issue のバックログが自動的に作成されます。GitHub Copilot に一覧を表示してもらいましょう。

1. Chat ビューのエージェント ドロップダウンから **Agent** を選択します。**Agent** エージェントは、自律的に計画を立て、複数のファイルを変更し、ターミナル コマンドを実行してツールを呼び出します。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者は、特定のモデルを推奨するものではありません。ワークショップの作成時に Claude Sonnet 4.5 を使用したため、指示にはそのモデルを記載しています。提案されるコードに一貫性を持たせ、スムーズに体験できることを期待しています。ただし、大規模言語モデル (LLM) は確率的に動作するため、ワークショップの例とは異なる提案が表示されることがあります。これは通常の想定された動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が異なる Model Context Protocol (MCP) コマンドを使う場合がありますが、タスクは完了できるはずです。

3. 次のプロンプトを Copilot に送信して、バックログの Issue を確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot はユーザーに最も役立つものを判断し、Issue の優先順位も提示します。

## 指示ファイルを確認する

コードの生成をエージェントに依頼する前に、Copilot にコンテキストを伝えるために使う指示ファイルを確認します。Web サイトに機能を追加する方法を説明する [ユーザー インターフェイス (UI)][ui-instructions] ファイルを使います。

1. codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加に関する全体的なガイダンスを確認します。次の内容が含まれています。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト容易性、アクセシビリティの原則。
   - さまざまなファイル形式に関する指示へのリンク。対象には Astro、Tailwind CSS、Drizzle があります。

> [!TIP]
> 指示ファイルから、他の指示ファイルやプロジェクトのファイルを参照できます。パスは指示ファイルからの相対パスです。指示を再利用し、大きな内容を管理しやすい単位に分け、コード生成時に Copilot が参考にする例を示せます。

## 絞り込み機能を実装する

絞り込み機能を完成させるには、少なくとも次の 3 つを更新する必要があります。

- データ レイヤー (`src/lib/`) の絞り込みロジックを追加または調整する。
- 絞り込み動作のテストを追加または更新する。
- ゲーム一覧ページに絞り込み UI を追加する。

さらに、コードベースにマージする前にテストを実行し、合格することを確認します。Copilot Agent Mode にこれらの作業を依頼しましょう。

1. Copilot との現在の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択して、指示ファイルを追加します。

   ![UI 指示ファイルを選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント ドロップダウンで、引き続き **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. モデルが引き続き **Claude Sonnet 4.5** であることを確認します。
5. バックログの関連 Issue に基づいて機能を実装するよう、次のプロンプトで Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調査し、機能に関連するファイルを特定する様子を確認します。データ レイヤーのヘルパー、UI、テストを見つけてから、ファイルを変更しテストを実行します。

   ![Copilot がプロジェクト ファイルを調査している様子を示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行います。タスクやコードベースの複雑さによっては数分かかることがあります。作業中、コード エディターに **Keep** と **Undo** が表示される場合があります。Copilot が完了すると、すべての変更に対して **Keep** または **Undo** を選べるようになるため、作業中に選択する必要はありません。

7. Copilot から求められたら、**Continue** を選択してテストを実行します。

   ![Copilot Chat ペインにテスト実行の確認ダイアログが表示されたスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりすることがあります。問題ありません。Copilot はコード生成とテストを繰り返し、エラーが見つからなくなるまで作業します。

   ![Copilot Agent Mode の完了済みチャット セッションを示すスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードをレビューすることは常に重要です。

10. Web サイトを実行しているブラウザーに戻り、新機能を確認します。
11. すべての動作を確認してコードのレビューが終わったら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

おめでとうございます。この演習では、GitHub Copilot Agent Mode を使って Tailspin Toys Web サイトに新機能を追加しました。次の方法を学びました。

- GitHub Copilot Agent Mode で UI とデータ レイヤーにまたがる機能を実装する。
- Copilot Agent Mode でプロジェクトを調査し、関連ファイルを特定して、複数のファイルを連携して変更する。
- Copilot Agent Mode が生成した変更とテストをレビューしてからコードベースにマージする。

次は、[Playwright MCP サーバーで機能をテストし][next-lesson]、プル リクエストを作成します。

### 追加演習 - ページネーションを実装する

ゲームの一覧が増えると、ページネーションが必要になります。この演習で学んだことを使い、Copilot にページネーションを追加するよう依頼します。コードを作成するときは、次の点を検討してください。

- 既存の指示ファイルを使うなど、既存のベスト プラクティスに従う。
- ページサイズをユーザーが選べるようにするか、固定値にするかなど、ページネーションの実装方法を検討する。
- 希望する実装に必要なガイダンスを含むプロンプトを作成する。
- Copilot に変更を依頼してコンテキストを追加しながら、何度かやりとりする場合がある。これは Copilot を使う通常のワークフローです。

## リソース

- [Copilot の ask、edit、agent mode: 機能と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
