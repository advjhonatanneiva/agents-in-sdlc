---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** を使うと、GitHub Copilot がクラウドで非同期に作業できます。GitHub 上でタスクを割り当てると、cloud agent がバックグラウンドでリポジトリを調査して変更を加え、プル リクエストを開きます。その間、別の作業を続けられます。

演習では、まず cloud agent が従うカスタム指示を追加し、GitHub Issue を割り当てて実装させます。カスタム エージェントを確認して利用し、エージェント ダッシュボードからセッションを監視して指示し、最後に作成されたプル リクエストをレビューして作業を反復します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと codespace を作成する |
| [1. カスタム指示][ex1] | コンテキスト | cloud agent が従うカスタム指示を追加する |
| [2. Cloud Agent][ex2] | 非同期エージェント | GitHub Issue を Copilot cloud agent に割り当てる |
| [3. カスタム エージェント][ex3] | 特化エージェント | カスタム エージェントを確認して使う |
| [4. エージェントの管理][ex4] | 監視 | エージェント セッションを監視して指示する |
| [5. 反復][ex5] | レビュー | プル リクエスト (PR) をレビューし、Copilot の作業を反復して次のステップを選ぶ |

## 前提条件

このワークショップに参加する前に、次の準備をしてください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランを持つ GitHub アカウント
- [ ] アカウントまたは Organization で Copilot cloud agent が有効になっていること

> [!TIP]
> 有料プランを利用していない場合でも、認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コード レビュー、Copilot CLI の機能が含まれているため、どのハーネスも完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Cloud agent は **Copilot Student、Pro、Pro+、Business、Enterprise** で利用できます。Copilot Free を除くすべてのプランが対象です。Copilot Business または Enterprise では、管理者が Organization で有効にする必要があります。

> [!NOTE]
> このハーネスでは Model Context Protocol (MCP) を扱いません。Copilot で MCP サーバーを使う方法を確認するには、[CLI ハーネス](../cli/)または [VS Code ハーネス](../vscode/)を参照してください。

## 開始する

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
