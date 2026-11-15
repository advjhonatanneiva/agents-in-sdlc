---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成人工知能 (AI) を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、Copilot がその情報を利用できるようにします。Copilot とコンテキストを共有する方法はいくつかあります。なかでも重要なのは、コード生成の指針を伝える[指示ファイル][instruction-files]です。

この演習では、次の作業を行います。

- リポジトリのカスタム指示とパスで対象を限定した指示ファイルを使い、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認する。
- 現在の指示を使って絞り込み用の最初のデータ スライス (発行元ヘルパー) を生成する。
- `.github/copilot-instructions.md` に新しいリポジトリ全体の標準を追加する。
- 同じプロンプトを再実行し、生成コードが新しい標準に従う様子を確認する。
- 次の演習で Copilot が更新されたガイダンスを使えるよう、指示の更新と絞り込みのスライスを `main` にコミットする。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot は非決定論的です。この演習では、指示を更新した後の動作の「傾向」を確認します。出力が一字一句同じになることを期待しないでください。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発の進め方に関するガイドラインと要件があります。具体的には、次のとおりです。

- データ レイヤーには、常にユニット テストが必要です。
- ユーザー インターフェイス (UI) はダーク モードで、現代的な見た目にします。
- TypeScript 用のドキュメント コメント (TSDoc) をコードに追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使って、Copilot がこれらの慣行に沿ってタスクを実行するための情報を提供します。

### カスタム指示

カスタム指示を使うと、コーディング スタイルや要件を Copilot に伝え、より適切な提案やコード スニペットを生成できるようになります。希望するコーディング規約、ライブラリ、コメントの形式などを指定できます。リポジトリ全体に適用する指示も、特定の種類のファイルに適用する指示も作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストで Copilot に送信される単一の指示ファイルです。Copilot へのチャットや CLI リクエストの多くに関係する、技術スタック、構築中の内容、ベスト プラクティス、全体的なガイダンスなどの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル形式に作成できます。TypeScript や Astro などの言語、UI コンポーネントの作成やユニット テストの追加などのタスクに関するガイドラインを記載できます。

> [!NOTE]
> 統合開発環境 (IDE) で作業する場合、指示ファイルがコード生成に使われるのは Copilot Chat のみです。コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの指示と、`applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方をコード生成時に使います。
>
> また Copilot は、AGENTS.md や CLAUDE.md など、別の標準の指示ファイルにも対応しています。[カスタム指示のサポート][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベスト プラクティス

指示ファイルの作成に関する詳しい説明はこのワークショップの範囲外です。ただし、サンプル プロジェクトには代表的な方法が含まれています。基本的には、次のようにします。

- `copilot-instructions.md` は、構築中の内容、プロジェクトの構成、全体的なコーディング標準など、プロジェクト全体のガイダンスに集中させます。
- `*.instructions.md` は、ユニット テスト、Astro コンポーネント、データ レイヤーなどのファイル形式や特定のタスクに合わせて作成します。
- 自然な言葉で、明確なガイダンスを記載します。コードの良い例と避けるべき例を示します。

指示ファイルの作成方法は 1 つではありません。AI の使い方と同様、プロジェクトに合う方法を試しながら見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトでは、指示ファイルを充実させることをお勧めします。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルがあります。
>
> Copilot に指示ファイルを生成してもらうこともできます。利用する画面によって操作は異なります (VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init` など)。利用する画面に応じたレッスンで手順を説明します。
>
> テンプレートやひな形を探している場合は、指示ファイル、カスタム エージェント、その他のリソースを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを読みましょう。中心となる `copilot-instructions.md` が 1 つあり、さまざまなタスク向けの `*.instructions.md` ファイルもあります。エディターまたは GitHub Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には、**GitHub Actions Workflows** に関するガイダンスがあります。これらの指示は Copilot とのあらゆるやりとりに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ レイヤー、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象となるファイルを決める glob を指定しています。ここでは、`**/*.test.ts` に一致するファイルなど、TypeScript のテスト ファイルに適用されます。
5. このプロジェクトのユニット テスト作成に関する指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、最後までスクロールします。ほかの指示ファイル (`unit-tests.instructions.md` など) やプロジェクト内の既存ファイルへのリンクを確認します。これにより、大きな指示を再利用できる小さなファイルに分け、Copilot がコード生成時に参考にする例を示せます。ここにあるパスはリポジトリのルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc のドキュメント コメントとファイル コメント ヘッダーのルールを追加します。

## 変更用のブランチを作成する

ベスト プラクティスに沿って、変更用のブランチを作成します。

1. 前の演習で使用した codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めてください。モードとモデルの選択は演習によって異なります。必要な場合は各手順に記載しています。

カスタム指示の影響を確認するため、まず現在の指示ファイルを使ってプロンプトを送信します。その後、指示を更新し、同じプロンプトを再送信して違いを確認します。

1. Copilot が必要なコンテキストだけを使えるよう、前の演習で開いたエディター タブを閉じます。
2. Copilot にヘルパーの配置場所を知らせるため、`src/lib/publishers.ts` を開きます。
3. Chat ビューのエージェント ドロップダウンから **Agent** を選択し、ファイルを変更できるようにします。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調査してコードを適用します。通常、ヘルパー ファイルとテストの両方に変更を加えます。
6. 提案されたヘルパーは、最初の引数として `db` クライアントを受け取る型付き関数で、型付きの発行元配列を返します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ レイヤー規約に基づくものです。
7. 提案されたコードに TSDoc のドキュメント コメントとファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメント コメントを追加する場合があります。その場合も問題ありません。指示を更新した後に動作の一貫性が改善されることが、この演習の目的です。

## 新しいリポジトリ標準を追加する

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクト全体の情報を提供します。コードの提案を改善するため、リポジトリのコーディング標準を記載します。

1. `.github/copilot-instructions.md` を再度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準は記載されていますが、コード内ドキュメントのルールはありません。そのため、生成されたヘルパーにドキュメント コメントがありませんでした。
3. 既存の標準のすぐ下に次の Markdown を追加し、ファイル コメント ヘッダーと TSDoc のドキュメント コメントを Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで見たように、リポジトリ全体に適用する指示 (`.github/copilot-instructions.md`) と、言語、ファイル形式、タスクに限定した指示 (`*.instructions.md`) を作成できます。ドキュメント コメントのルールなど、プロジェクト全体の標準にはリポジトリ レベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

新しいチャットを開始し、更新した指示を使って同じプロンプトを送信します。

1. Copilot Chat に戻り、**New Chat** を選択して会話をクリアします。
2. Copilot の対象を指定するため、`src/lib/publishers.ts` をもう一度開きます。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックが追加されていることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが追加されていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準に従うよう Copilot を導き、次の演習で引き続き使う実際のコードに適用しました。

## 指示をコミットしてブランチをプッシュする

指示ファイルもほかのリポジトリ内ファイルと同じように、ソース管理で管理します。変更をコミットしてリポジトリにプッシュします。

1. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
2. ターミナルで次のコマンドを実行し、指示とヘルパーが更新されていることを確認します。

   ```bash
   git status
   ```

3. ターミナルで指示の更新と、ヘルパーの基盤のために Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ターミナルからブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プル リクエストを作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント標準の Issue に関連付けてプル リクエストを作成します。手動で行う代わりに、すでにプロジェクトに接続されている GitHub ツールを使って Copilot に依頼できます。Copilot に Issue を見つけさせ、それを完了するプル リクエスト (PR) を作成してからマージします。

> [!NOTE]
> この演習では、Copilot に GitHub 上の操作を初めて依頼します。プロジェクト テンプレートでは GitHub ツールがすでにワークスペースに接続されています。Copilot が初めてツールを使うときに、VS Code から GitHub へのサインインを求められることがあります。画面の指示に従ってください。この接続の仕組み (Model Context Protocol) は、後の演習で学びます。

1. codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント ピッカーで **Agent** を選択します。
3. 指示ファイルの更新に関する Issue を見つけ、現在のブランチから PR を作成するよう、指示の更新と新しい発行元ヘルパーの基盤の両方を説明するプロンプトを Copilot に送ります。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が代わりに GitHub 操作を行うことを求めたら、コマンドを確認して、適切な場合は **Allow** を選択します。
5. PR が作成されたら、次のプロンプトを使ってマージし、ローカル ブランチを `main` に戻すよう Copilot に依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. Copilot が代わりに GitHub 操作とシェル コマンドを実行することを求めたら、コマンドを確認して、適切な場合は **Allow** を選択します。

GitHub Copilot の支援でプル リクエストを作成し、マージできました。

## まとめと次のステップ

このプロジェクトで指示ファイルを使って Copilot にコンテキストを伝える仕組みを確認しました。VS Code の Copilot Chat を使って、次の作業を行いました。

- リポジトリのカスタム指示とパスで対象を限定した指示ファイルを調べ、コンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認した。
- 現在の指示を使ってプロンプトを送信し、絞り込み用の発行元ヘルパーを生成した。
- `.github/copilot-instructions.md` に新しいリポジトリ全体の標準を追加した。
- 同じプロンプトを再実行し、提案されたコードが新しい標準に従う様子を確認した。
- 次の演習で作業を引き継げるよう、指示の更新と絞り込みのスライスを `main` にコミットした。

次は、指示を[agent mode で実際に使い][next-lesson]、Copilot にコードベース全体の新機能を追加してもらいます。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善するための 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルやその他のリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
