---
title: "演習 4 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせて、専門的な人工知能 (AI) アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義し、特定の種類の作業を効果的に進めるための指示、ベスト プラクティス、コーディング パターン、分野固有の知識を Copilot に提供できます。チームの専門知識を再利用可能なエージェントにできます。たとえば、[Web Content Accessibility Guidelines (WCAG)][wcag] 準拠を徹底するアクセシビリティ エージェント、安全なコーディング慣行に従うセキュリティ エージェント、一貫したテスト パターンを守るテスト エージェントを作成できます。

カスタム エージェントは、プロジェクトの `.github/agents` フォルダーまたはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、その後にエージェントの動作、専門知識、指示を定義する Markdown プロンプトを記述します。

### カスタム エージェントと agent skills の違い

カスタム エージェントと[エージェント スキル][agent-skills-concept]には、役割が一部重なるところがあります。どちらも主に Markdown ファイルで定義し、AI に作業の方法を指示します。明確に区別するには、**カスタム エージェントは作業者、スキルはツール**と考えてください。

カスタム エージェントは独自のコンテキスト ウィンドウを持ち、スキルやほかのエージェントを調整しながら作業できます。このラボでは、アクセシビリティ カスタム エージェントがサイトをアクセシビリティ ガイドラインに照らしてレビューし、更新します。その作業の一部として、プル リクエストのワークフロー スキルやテスト実行・管理スキルなどを呼び出すこともできます。

> [!NOTE]
> カスタム エージェントの作成に唯一の正解はありません。AI を使うほかの作業と同様に、環境やシナリオに合わせてテストと調整を続けてください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、見え方や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを使えるようにしたいと考えています。ユーザーからの最近のフィードバックによると、テキストと背景色のコントラストが不十分なため、ダーク テーマが読みにくいと感じる人がいます。このアクセシビリティ上の課題に対処するため、デザイン チームはユーザーが切り替えられるハイ コントラスト モードを求めています。

アクセシビリティは重要なため、できるだけ早く実装します。カスタム エージェントを利用して機能を生成します。

この演習では、次の作業を行います。

- 既存のアクセシビリティ カスタム エージェントを確認する。
- Copilot Chat でアクセシビリティ エージェントを使い、ハイ コントラスト モードを実装する。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに作成されています。Copilot を導く方法を理解するため、内容を確認します。

まず codespace に戻り、番号付きの手順に沿ってアクセシビリティ作業用のブランチを作成します。演習 3 の絞り込みプル リクエスト (PR) とは分けて作業します。

1. codespace のターミナルで、`main` から新しいブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. `.github/agents/accessibility.md` を開きます。
3. `name` と `description` フィールドを含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタム エージェントには、`name` と `description` を含む frontmatter が必要です。

4. 続くセクションを確認します。次の内容が記載されています。
   - アクセシブルな Web サイトのコード生成に関する主な責任。
   - アクセシビリティのベスト プラクティス。
   - ハイパーテキスト マークアップ言語 (HTML)、カスケーディング スタイル シート (CSS)、JavaScript のコード例。
   - よくある問題と避けるべき点。

## Copilot Chat でカスタム エージェントを使う

VS Code では、`.github/agents` に定義したカスタム エージェントが Copilot Chat ビュー下部のエージェント ドロップダウンに表示されます。カスタム エージェントを選択すると、そのエージェント専用の指示とツールに基づいてチャット セッションを進められます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めてください。モードとモデルの選択は演習によって異なります。必要な場合は各手順に記載しています。

1. Chat ビューのエージェント ドロップダウンで **Agent** が選択されていない場合は選択します。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部で、現在のエージェント (既定では **default**) が表示されているエージェント ドロップダウンを選択します。
3. 利用可能なエージェントの一覧から **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティ エージェントに送信します。

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat がコードベースを調査し、変更を提案してプロジェクト ファイルを編集します。編集はファイル パスと差分とともにチャット内に表示されます。
6. 編集が適用されると、**Files changed** の表示が更新され、このセッション中にエージェントが変更したファイルを確認できます。

> [!NOTE]
> この作業には数分かかる場合があります。Copilot はリポジトリに実際の変更を加えています。Astro コンポーネント、CSS、関連テストなどの既存ファイルが編集されます。

次の演習では、作業中の変更を確認し、エージェントに指示を出します。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野向けの専門的な AI アシスタントである [GitHub Copilot のカスタム エージェント][custom-agents]について確認しました。チームの専門知識と標準を再利用可能なエージェントにまとめ、Copilot の作業を導けます。

次の内容を確認しました。

- 既存のアクセシビリティ カスタム エージェントを確認した。
- Copilot Chat でアクセシビリティ エージェントを使い、ハイ コントラスト モードを実装した。

次は、エージェントの作業を[監視して調整し][next-lesson]、変更を確認しながら同じセッションにライト モードの切り替えを追加します。

## リソース

- [カスタム エージェントについて][custom-agents]
- [統合開発環境 (IDE) でカスタム エージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタム エージェント][custom-agents-vscode]
- [カスタム エージェントの構成][custom-agents-config]
- [awesome-copilot のカスタム エージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright Model Context Protocol (MCP) サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
