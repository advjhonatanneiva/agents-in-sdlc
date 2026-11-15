---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan で進め方を合意し、Autopilot で Issue に基づく絞り込み機能を実装し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初は小さな機能をプロジェクトに追加しました。しかし、より大きな変更には、さらに堅牢なプロセスが必要です。GitHub Copilot CLI は組織の既存の開発フローに沿って動作するため、適切な方法で必要な機能を作成できます。このレッスンから数回にわたり、一般的なエージェント主導の開発プロセスに沿って進めます。Issue を起点に機能を作成し、コードの妥当性と機能の動作を確認した後、プロジェクトにマージします。

> [!NOTE]
> 機能開発を続ける間、同じ会話とブランチを使います。通常は作業対象のファイルの種類ごとに異なるブランチやプル リクエスト (PR) を使いますが、ここでは主要な概念に集中できるよう手順を簡略化します。

このレッスンでは、次の作業を行います。

- GitHub Issue から新しい Copilot の会話を始める。
- Plan モードで要件を定義する。
- Autopilot モードで新機能を実装する。
- コードをレビューする。
- 転送されたブラウザーで機能を手動で検証する。

この機能開発を続けながら、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、Model Context Protocol (MCP) による検証を追加して品質保証 (QA) エージェントを作成し、機能のプル リクエスト (PR) を開きます。

## シナリオ

Tailspin Toys のカタログは拡大しており、訪問者はカテゴリや発行元でゲームを絞り込む必要があります。バックログの Issue には機能が説明されていますが、カテゴリの組み合わせ方など、コーディング前に合意が必要な詳細があります。Plan モードで判断を確定してから、範囲を明確にした実装を Autopilot に許可します。

## 背景

人工知能 (AI) コーディング エージェントを開発フローに導入しても、基本は変わりません。むしろ、基本の重要性は増します。多くの開発者は、次のような流れで作業します。

1. 実装内容を記述した Issue から始めます。
2. 実装する内容の計画を立てます。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新機能を手動で検証します。
6. プル リクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> 具体的な内容はチームや組織によって異なりますが、ほとんどの開発フローは上記の流れを基本にしています。

この標準的な進め方に従うことで、AI が生成したコードが要件を満たし、手書きのコードと同じ検証プロセスを経ることを確認できます。

## 会話モード

**会話モード**は、エージェントにどれだけの自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すとモードを切り替えられます。

- **Interactive**: 開発者とエージェントが協力します。エージェントは変更を提案し、入力を待ってから作業を続けます。
- **Plan**: エージェントが最初に計画を作成します。プロジェクト ファイルの編集はできません。
- **Autopilot**: エージェントが自律的にコードを書き、テストを実行し、タスクが完了するまで反復します。

まず Plan モードで計画をレビューしてから、Autopilot で実装します。

## Issue から始める

絞り込み機能の作業を始める前に、Codespace に戻り、リポジトリとターミナルの準備ができていることを確認します。

1. Codespace に戻ります。停止している場合は、再起動してから続けます。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新してから、絞り込み作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトが `#7` (または同様の番号) で始まることを確認します。`#` を使うと、GitHub の Issue またはプル リクエスト (PR) をコンテキストに追加できます。

## 絞り込み機能を計画する

計画を立てることで、Copilot に作業を任せる前に機能実装やタスクの進め方を決められます。複雑な作業では、時間をかけて計画することをお勧めします。Plan モードに切り替えて計画の作成を依頼します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** であることを確認します。
2. 直前の手順で追加した Issue 参照の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調査し、最適な方法を判断して計画を作成します。

3. 絞り込み機能の動作について質問された場合は、希望に沿って回答します。どの回答も間違いではありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 上下にスクロールして計画を確認します。
6. 自分の判断と合わない箇所があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成してレビューしたら、Copilot に実装を依頼します。Autopilot を使うと、Copilot が作業を進められます。

Autopilot では、Copilot がタスクを完了したと判断するまで問題に取り組み、反復できます。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みバージョンで同様のラベルの選択肢を選択します。
2. プロンプトの下にあるモード表示が **Autopilot** であることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると実装がすぐに始まる場合があります。先に計画をレビューしてください。Copilot から依存関係がない、またはポートが競合していると報告された場合は、チェック完了と判断する前にセットアップの問題を解決してください。

## 実装をレビューして検証する

ほかのコードと同じように、生成されたコードもマージ前にレビューします。コードを確認し、サイトを実行して動作も確かめます。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示が **Plan** または **Autopilot** ではないことを確認します。
2. `/diff` を入力し、絞り込み機能の実装とテストを確認します。
3. 結果を Issue と計画で決めた内容に照らし合わせます。
4. コードのレビューが終わったら、<kbd>Esc</kbd> を押して差分表示を終了します。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新機能を試す

コードに問題がなさそうでも、実際に動くか確認しましょう。前回と同じようにアプリを起動し、Codespaces の転送ポートからサイトを開きます。

1. アプリを起動し、Uniform Resource Locator (URL) を報告するよう Copilot に依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces からポート `4321` が利用可能だと通知されたら、**Open in Browser** を選択します。
3. カテゴリによる絞り込み、発行元による絞り込み、および計画で合意した組み合わせを試します。
4. リセット時と結果がない場合の動作が、Issue と決定内容に合っていることを確認します。
5. Codespace に戻り、起動した開発サーバーを停止するよう Copilot に依頼します。

## まとめと次のステップ

会話モードを切り替えて機能を作成し、レビューしました。このレッスンでは、次の作業を行いました。

- GitHub Issue から新しい Copilot の会話を始めた。
- Plan モードで要件を定義した。
- Autopilot モードで新機能を実装した。
- コードをレビューした。
- 転送されたブラウザーで機能を手動で検証した。

次は、コードが文書化された規約に従うように、[カスタム指示を使ってさらに詳しく確認します][next-lesson]。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
