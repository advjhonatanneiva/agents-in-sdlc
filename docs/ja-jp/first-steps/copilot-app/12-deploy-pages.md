---
title: "レッスン 12 - GitHub Pages でサイトをデプロイする"
description: "GitHub Copilot app を使って Pages ワークフローを構成し、プル リクエストをレビューして、公開された Space Quiz を確認します。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

GitHub Pages を使って Space Quiz を公開しましょう。Copilot はワークフローの準備とデプロイを支援できますが、変更をレビューし、サイトが公開されたことを確認するのは開発者の役割です。

このレッスンでは、次の作業を行います。

- Copilot に GitHub Pages のデプロイを構成するよう依頼する。
- マージ前にワークフローとプル リクエストをレビューする。
- デプロイの進行状況を確認し、公開されたサイトを検証する。

## シナリオ

Space Quiz はすでに GitHub リポジトリにありますが、手軽に試せる場所がありません。単一ファイルのサイトを GitHub Pages で公開すると、サーバーやランタイム依存関係を追加せずに、公開された Uniform Resource Locator (URL) から利用できるようになります。

## Copilot にデプロイの準備を依頼する

> [!WARNING]
> プル リクエストを作成すると、コードがコミットおよびプッシュされる場合があります。このプロンプトを送る前に、承認設定と送信先のリポジトリを確認してください。**Approve all** が有効な場合、別途確認を求められないことがあります。

1. `space-quiz` の Copilot app セッションで、次のプロンプトを送信します。

   ```plaintext
Prepare a GitHub Pages deployment for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages after changes reach the default branch. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and report its link and any required Pages settings or permissions. Do not merge, enable auto-merge, change Pages settings, or deploy. Stop and wait for my review.
   ```

## ワークフローとプル リクエストをレビューする

変更を確認し、マージや公開の許可を出す前に、プル リクエストとワークフローの内容をレビューします。

1. README は、リポジトリにまだ存在しない場合に限り追加されていることを確認します。
2. ワークフローのビルドとデプロイの手順、トリガー、権限を含めて、プル リクエストの変更をレビューします。
3. ワークフローが、まだマージされていないプル リクエストではなく既定のブランチからデプロイすることを確認します。
4. 修正が必要な場合は依頼し、更新後の差分を確認します。マージを許可する前に、必要なプル リクエストのチェックが合格するまで待ちます。

## マージとデプロイを許可する

> [!WARNING]
> 次のプロンプトを送ると、エージェントにレビュー済みのプル リクエストのマージ、リポジトリの Pages 設定の変更、サイトの公開を許可します。レビューを完了し、リポジトリとプル リクエストを確認した後にのみ送信してください。承認設定によっては、これらの操作に別途確認を求められない場合があります。

1. `<PR_NUMBER>` をレビューしたプル リクエストの番号に置き換えてから、次の別個のプロンプトを送信します。

   ```plaintext
I have reviewed pull request <PR_NUMBER> in this repository and authorize its merge and deployment. Enable GitHub Pages using GitHub Actions, merge that pull request only after required checks pass, and monitor the deployment until it succeeds. Report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
   ```

![マージされた Space Quiz のプル リクエスト、Build website、Upload artifact、Deploy to Pages の各ステップが成功した GitHub Actions Deploy website 実行、および yourname.github.io/space-quiz/ で公開された Space Quiz の 3 段階を示す図](../../../_images/first-steps-app-pages-deploy.svg)

## デプロイを検証する

デプロイが成功し、公開 URL からサイトを利用できることを確認します。

1. GitHub Actions のデプロイ実行を追跡し、成功するまで待ちます。
2. 報告された URL を開き、Space Quiz が読み込まれることを確認します。

エージェントに Pages の有効化やプル リクエストのマージを許可できない場合は、提示された具体的な手順に従います。デプロイが成功し、公開されたサイトが読み込まれるまで、タスクを完了したと判断しないでください。

## まとめと次のステップ

Space Quiz が公開され、エージェントの報告だけに頼らずデプロイを確認しました。[レッスン 13: レビューと次のステップ][next-lesson]に進みます。

[next-lesson]: ../13-review/
