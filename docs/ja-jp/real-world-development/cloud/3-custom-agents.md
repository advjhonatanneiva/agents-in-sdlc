---
title: "演習 3 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせて、専門的な人工知能 (AI) アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義し、特定の種類の作業を効果的に進めるための指示、ベスト プラクティス、コーディング パターン、分野固有の知識を Copilot に提供できます。チームの専門知識を再利用可能なエージェントにできます。たとえば、[Web Content Accessibility Guidelines (WCAG)][wcag] 準拠を徹底するアクセシビリティ エージェント、安全なコーディング慣行に従うセキュリティ エージェント、一貫したテスト パターンを守るテスト エージェントを作成できます。

カスタム エージェントは、プロジェクトの `.github/agents` フォルダーまたはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、`name` と `description` を含む YAML frontmatter と、その後にエージェントの動作、専門知識、指示を定義する Markdown プロンプトを記述します。

### カスタム エージェントと agent skills の違い

カスタム エージェントと[エージェント スキル][agent-skills-concept]には、役割が一部重なるところがあります。どちらも主に Markdown ファイルで定義し、AI に作業の方法を指示します。明確に区別するには、**カスタム エージェントは作業者、スキルはツール**と考えてください。

カスタム エージェントは独自のコンテキスト ウィンドウを持ち、スキルやほかのエージェントを調整しながら作業できます。このラボでは、アクセシビリティ カスタム エージェントがサイトをアクセシビリティ ガイドラインに照らしてレビューし、更新します。その作業の一部として、プル リクエストのワークフロー スキルやテスト実行・管理スキルなどを呼び出すこともできます。

> [!NOTE]
> カスタム エージェントの作成に唯一の正解はありません。AI を使うほかの作業と同様に、環境やシナリオに合わせてテストと調整を続けてください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

カスタム エージェントを使って、次の内容を確認します。

- カスタム エージェントの定義方法。
- カスタム エージェントにタスクを割り当てる方法。

## シナリオ

Tailspin Toys は、見え方や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを使えるようにしたいと考えています。ユーザーからの最近のフィードバックによると、テキストと背景色のコントラストが不十分なため、ダーク テーマが読みにくいと感じる人がいます。このアクセシビリティ上の課題に対処するため、デザイン チームはユーザーが切り替えられるハイ コントラスト モードを求めています。

アクセシビリティは重要なため、できるだけ早く実装します。カスタム エージェントを利用して機能を生成します。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに作成されています。Copilot を導く方法を理解するため、内容を確認します。

codespace に戻り、アクセシビリティ カスタム エージェント ファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドを含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタム エージェントには、`name` と `description` を含む frontmatter が必要です。

3. 続くセクションを確認します。次の内容が記載されています。
   - アクセシブルな Web サイトのコード生成に関する主な責任。
   - アクセシビリティのベスト プラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題と避けるべき点。

## Issue を作成して割り当てる

Mission control は、環境のすべてのエージェント作業を管理する場所です。Copilot cloud agent にタスクを割り当て、作業を監視し、指示を追加したり方向を変えたりできます。まず、ハイ コントラスト モードの作成を Copilot に割り当てます。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して Issue を作成します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。
9. カスタム エージェントの一覧から **Accessibility agent** を選択します。

    ![カスタム エージェントの割り当てで Accessibility が選択された画面](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドでタスクに取り組みます。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野向けの専門的な AI アシスタントである [GitHub Copilot のカスタム エージェント][custom-agents]を確認しました。チームの専門知識と標準を再利用可能なエージェントにまとめ、Copilot の作業を導けます。

次の内容を確認しました。

- カスタム エージェントの定義方法。
- カスタム エージェントにタスクを割り当てる方法。

Copilot がハイ コントラスト モードの実装に取り組んでいる間に、次は [Mission control からエージェント セッションを監視して指示します][next-lesson]。

## リソース

- [カスタム エージェントについて][custom-agents]
- [Organization でカスタム エージェントを使う準備をする][org-custom-agents]
- [Enterprise でカスタム エージェントを使う準備をする][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
