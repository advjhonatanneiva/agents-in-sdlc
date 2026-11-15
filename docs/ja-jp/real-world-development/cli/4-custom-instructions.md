---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、絞り込み機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成系人工知能 (AI) を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合、そのガイダンスを Copilot が利用できるようにします。[指示ファイル][instruction-files]では、必要なコードだけでなく、コードの構成方法も説明できます。絞り込み機能を作成したので、Copilot が使った指示を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次の作業を行います。

- リポジトリの指示とパスで対象を限定した指示ファイルがエージェントに伝わる仕組みを確認する。
- コーディング標準を適用するため、指示ファイルを更新する。
- 指示ファイルがコードに与える影響を確認する。

## シナリオ

Tailspin Toys には、開発の進め方に関するガイドラインと要件があります。具体的には、次のとおりです。

- コメントには、コードの言い換えではなく、意図や自明でない判断を記述します。
- `db/` と `src/lib/` のエクスポート関数には、目的、パラメーター、戻り値を説明する TypeScript 用のドキュメント コメント (TSDoc) または JavaScript 用のドキュメント コメント (JSDoc) を記述します。該当する場合は、挿入可能な `db` 引数も含めます。
- 再利用可能な Astro コンポーネントでは `Props` の契約を文書化し、関連コードが変更されたときにコメントも最新の状態に保ちます。
- 既存のフォーマットと lint のガイダンスを維持します。

指示ファイルを使い、Copilot がこれらの開発慣行に沿ってタスクを実行するための情報を提供します。

## 指示ファイル

カスタム指示を使うと、コーディング スタイルや要件を Copilot に伝え、より適切な提案やコード スニペットを生成できるようになります。希望するコーディング規約、ライブラリ、コメントの形式などを指定できます。リポジトリ全体に適用する指示も、特定の種類のファイルに適用する指示も作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストで Copilot に送信される単一の指示ファイルです。多くのリクエストに関係するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイルの種類、タスクに関するガイドラインを提供します。

> [!NOTE]
> その他の指示形式とサポート状況は、環境によって異なります。特定の形式に依存する前に、[カスタム指示のサポート リファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

変更を加える前に、スターター プロジェクトに含まれる指示ファイルを確認し、変更の影響を確かめます。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディング ガイダンスを確認します。この指示は、このリポジトリで Copilot を使うすべての操作に適用されます。
4. `.github/instructions` フォルダーを開いてファイルを確認します。Astro ファイル、Drizzle データ レイヤー、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象となるファイルを決める glob を指定しています。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルや既存プロジェクト ファイルへのリンクを確認します。大きな指示を再利用できる小さなファイルに分け、Copilot が参考にする例を示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存のファイルは有用ですが、まだ不足があります。新たに生成する TypeScript に TSDoc コメントを追加するよう、中心となる `copilot-instructions.md` ファイルを更新します。

1. `.github/copilot-instructions.md` で **Code formatting guidance** という見出しを探します。35 行目付近にあります。
2. 最後の箇条書きとして、次の内容を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。指示を保存した後、絞り込み機能の会話を終了して再開してください。機能のコンテキストを保ったまま、新しいガイダンスを読み込めます。

1. 絞り込み機能の会話が開いている場合は、`/exit` を入力して閉じます。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートで `game-filters-cli` ブランチに切り替えた状態から、保存済みの会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示された場合は、絞り込み機能の会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 更新したガイダンスを適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力して、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを取得する仕組みを確認し、新しい標準を機能に適用しました。このレッスンでは、次の作業を行いました。

- リポジトリの指示とパスで対象を限定した指示ファイルがエージェントに伝わる仕組みを確認した。
- 指示ファイルを更新して、コーディング標準を適用した。
- 指示ファイルがコードに与える影響を確認した。

次は、[再利用可能な quality-checks スキルをカスタマイズして実行し][next-lesson]、lint とテストを一貫して実施します。

## リソース

- [Copilot CLI のカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [効果的なカスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルやその他のリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
