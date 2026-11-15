---
title: "演習 5: GitHub Copilot の作業を反復する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー エクスペリエンスの改善と機能追加に関する複数のタスクで GitHub Copilot と作業しました。コードへのドキュメント追加、デザイン チームが反復して改善できる関連ゲーム機能の作成、ハイ コントラストとライト モードの切り替えなどのアクセシビリティ機能を Copilot に依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送って作業を改善します。

### シナリオ

何度も説明してきたように、生成系人工知能 (AI) を利用しても、ソフトウェア設計と開発・運用 (DevOps) の基本は変わりません。Copilot が生成したコードをレビューし、通常の DevOps プロセスに沿って作業します。Copilot が作成したドキュメント、関連ゲーム機能、アクセシビリティ機能の提案をレビューし、チームにも確認してもらえる状態にします。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は非同期に、監督者なしでタスクを実行するため、安全性を維持するセキュリティ制約が設けられています。具体的には、次のとおりです。

- Copilot はリポジトリへの読み取りアクセスのみを持ち、書き込みアクセスはコード用のブランチに**限定**されます。
- Cloud agent は GitHub Actions 内で実行され、作業用の独立した一時環境を作成します。
- GitHub Actions のワークフローを実行するには、人間による承認が必要です。
- Model Context Protocol (MCP) サーバーを含む[外部リソースへのアクセスは、既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が作成した最初のプル リクエスト (PR)、つまりコードへのドキュメント追加を確認します。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認するときに、ファイアウォールによって GitHub Copilot がブロックされているという警告が表示される場合があります。Copilot は MCP サーバーへの外部呼び出しを含め、既定では外部リソースへのアクセスが制限されているため、これは**想定どおり**です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、プル リクエストの一覧を開きます。
3. **Add missing documentation** など、同様のタイトルのプル リクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プル リクエストのタイトルに **[WIP]** が含まれます。その場合は Copilot が作業を完了するまで待ちます。数分かかることがあります。休憩したり、これまでに学んだことを振り返ったりしてください。

4. プル リクエストの準備ができたら、**Files changed** タブを選択して変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新たに追加された TypeScript 用のドキュメント コメント (TSDoc) など、更新されたコードを確認します。具体的な変更内容は異なります。

    変更を確認しながら、TSDoc のドキュメント コメント、TypeScript の規約、コメント ヘッダーを確認します。これらは演習 2 の最初に確認したカスタム指示ファイルに基づいています。

6. 更新内容をレビューして問題がなければ、**Conversation** タブに戻って下にスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. ワークフローの承認待ちがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローの承認](../../../_images/shared-approve-workflows.png)

9. ワークフローがプル リクエストのチェック セクションに登録されたことを確認します。単一の Astro アプリのプロジェクト チェックがすべて合格するはずです。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

Copilot とのプル リクエスト作業は一方通行ではありません。チーム メンバーに依頼するように、プル リクエストやコードへのインライン コメントで Copilot にメンションできます。Copilot はコメントを確認し、対応する新しいセッションを開始します。結果は確率的に変わるため、指示文をひとつに決めて示すことはできません。Copilot に依頼する内容の例を挙げます。

- 各コード ファイルの先頭に、ファイルの役割を短く説明するコメント ヘッダーを追加する。
- TypeScript と Astro ファイルに TSDoc のドキュメント コメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. 上の例などを参考に、コードベースに追加したいドキュメントの変更をコメントで依頼し、他のユーザーと同じように **@copilot** をメンションします。
2. **View Session** を選択し、Copilot が作業する様子を確認します。Copilot は変更用の新しいセッションを開始します。
3. **Back to pull request** を選択するとプル リクエストに戻れます。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、プル リクエストに新しいコミットが表示されます。
5. **Files changed** タブを選択して変更をレビューします。

満足するまで Copilot に依頼を続けてかまいません。問題がなければ、PR をドラフトからレビュー可能な状態に変更して `main` ブランチにマージします。

![PR をレビュー可能な状態に変更する](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue を解決するため、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** など、同様のタイトルの PR を選択します。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 更新内容をレビューして問題がなければ、**Conversation** タブに戻って下にスクロールします。
6. ワークフローの承認待ちを示す表示を確認します。
7. ワークフローの承認待ちがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローの承認](../../../_images/shared-approve-workflows.png)

8. ワークフローがプル リクエストのチェック セクションに登録されたことを確認します。単一の Astro アプリのプロジェクト チェックがすべて合格するはずです。完了まで数分かかることがあります。
9. **任意:** codespace でこのブランチに切り替えて、関連ゲーム機能を手動テストできます。codespace に移動し、ターミナルを開いて、Copilot が作成したブランチ名 (例: **copilot/fix-8**) を `<branch-name>` に置き換えて次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。前と同じように、Copilot cloud agent に追加の変更を依頼できます。たとえば、表示する関連ゲームの数を調整したり、コメント ヘッダーと TSDoc のドキュメント コメントを追加したりできます。この作業はカスタム指示を更新する前に割り当てたため、指示が反映されていない場合があります。前と同様に、**Conversation** タブでコメントを追加すると、Copilot が確認して新しいセッションを始めます。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ カスタム エージェントで実装した機能を確認します。この PR には、演習 3 で依頼したハイ コントラスト モードと、演習 4 で Mission control から追加を依頼したライト モードの両方が含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** など、同様のタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プル リクエストのタイトルに **[WIP]** が含まれます。その場合は Copilot が作業を完了するまで待ちます。数分かかることがあります。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 実装をレビューします。特に次の点を確認します。
   - 表示モードを切り替えるユーザー インターフェイス (UI) コンポーネント。
   - ユーザー設定を保存するローカル ストレージの使い方。
   - ハイ コントラストとライト モードに対応するカスケーディング スタイル シート (CSS) またはスタイルの変更。
   - アクセシビリティ属性 (Accessible Rich Internet Applications (ARIA) ラベル、キーボード操作など)。
   - 表示モードを切り替える JavaScript/TypeScript コード。

6. 更新内容をレビューして問題がなければ、**Conversation** タブに戻って下にスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. ワークフローの承認待ちがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローの承認](../../../_images/shared-approve-workflows.png)

9. ワークフローがプル リクエストのチェック セクションに登録されたことを確認します。単一の Astro アプリのプロジェクト チェックがすべて合格するはずです。完了まで数分かかることがあります。
10. **任意:** codespace でこのブランチに切り替えて、アクセシビリティ機能を手動テストできます。Copilot が作成したブランチ名を `<branch-name>` に置き換えて、codespace のターミナルで次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動し、ブラウザーでハイ コントラストとライト モードの切り替えを試します。ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティ カスタム エージェントが、アクセシビリティのベスト プラクティスに従って Copilot の実装を導いたことを確認します。アクセシビリティ上の懸念や改善点がある場合は、前の PR と同様に、コメントで **@copilot** に更新を依頼できます。

## 任意の演習 - さらに委任してみる

Cloud agent は、バックログの実際のタスクを任せ、その間に別の作業を進めるのに向いています。習慣を身につけるために、リポジトリに Issue を追加して Copilot に割り当ててみてください。例を挙げます。

- ゲーム詳細ページに支援者向けの関心フォームを追加する。
- ゲーム一覧ページにページネーションを実装する。
- データ アクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。これらのレッスンを通じて、次の作業を行いました。

- **このリポジトリに含まれるカスタム指示ファイルを確認し、cloud agent の出力への影響を確認しました。**
- **Issue を Copilot cloud agent に割り当て、環境の設定、計画、非同期の実行を確認しました。**
- **アクセシビリティ用のカスタム エージェントを作成して使い、ハイ コントラストとライト モードの切り替えを追加しました。**
- **Copilot agents ページを Mission control として使い、進行中のアクセシビリティ セッションを監視して指示しました。**
- **cloud agent のプル リクエストをレビューして反復し、`@copilot` に変更を依頼してワークフローを承認しました。**

## レビューと次のステップ

Cloud agent ハーネスを完了しました。引き続き確認する場合は、ほかのハーネスで今回学んだ内容を別の環境で試せます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — 統合開発環境 (IDE) 内で Copilot Chat の agent mode と MCP 統合を試します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan モードと Autopilot モード、エージェント スキル、QA カスタム エージェント、Playwright MCP、Agent Merge、cloud agent に戻る任意の `/delegate` を使い、ターミナルからレビュー済みの変更を 2 つ届けます。

自分のリポジトリでは、次の案を試せます。

- リファクタリングやテスト カバレッジの Issue を cloud agent に割り当てる。
- 別の分野向けのカスタム エージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
