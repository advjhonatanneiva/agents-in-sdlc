---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成系人工知能 (AI) を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、Copilot がその情報を利用できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に実行内容だけでなく方法も伝えられます。

この演習では、次の作業を行います。

- リポジトリのカスタム指示とパスで対象を限定した指示ファイルを使い、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認する。
- `.github/copilot-instructions.md` に新しいリポジトリ全体の標準を追加する。

> [!NOTE]
> VS Code と CLI のハーネスとは異なり、この演習では更新前後のプロンプトを比較しません。Copilot cloud agent は GitHub Issue に対して非同期で作業するため、影響をリアルタイムで並べて示すのは難しいためです。このハーネスの後半で cloud agent が作成したプル リクエストをレビューし、指示ファイルの影響を確認します。

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

## 新しいリポジトリ標準を追加する

ここで編集するのは、コード内の TSDoc コメントとファイル レベルのコメント ヘッダーをドキュメント化するプロジェクト全体のルールです。cloud agent は、このハーネスの後半で割り当てる Issue に取り組むときに、このルールを使います。

編集の前に、作業用のブランチを用意します。cloud agent は Issue の対象ブランチから指示を読み込みますが、変更をきれいにコミットしておきます。

1. codespace のターミナルで新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

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

## 指示の更新をコミット、プッシュ、マージする

Cloud agent は、Issue の対象ブランチにある指示ファイルを読み込みます。次の演習で Issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。そのため、cloud agent が新しい指示を読み込めるよう、変更を `main` にマージする必要があります。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプル リクエストを開き、マージします。次の演習で Issue を割り当てると、cloud agent がこの指示を読み込みます。

> [!TIP]
> ワークショップでは `main` に直接作業してもかまいません。その場合はブランチ作成を省略して、`main` に直接コミットします。ブランチの手順は、実際のプロジェクトでの作業方法を体験できるようにしています。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する仕組みを確認し、`.github/copilot-instructions.md` に新しいリポジトリ全体の標準を追加しました。このハーネスの残りの演習では、cloud agent が作成するプル リクエストで、この標準が使われている様子を確認します。

次は、[最初の Issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善するための 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルやその他のリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://awesome-copilot.github.com/
