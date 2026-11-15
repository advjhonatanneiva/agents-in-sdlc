---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプル リクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys の一連のワークフローを進めました。次の作業を行いました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトを確認し、登録済みの絞り込み Issue を見つけた。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプル リクエスト (PR) を手動でマージした。
- 絞り込み Issue を起点に、Plan モードで進め方を決め、Autopilot モードで実装し、Interactive モードでレビューした。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、ユニット テスト、lint、型チェックを実行した。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーで絞り込み機能を試した。
- 品質保証 (QA) カスタム エージェントを作成して選択し、要件、テスト カバレッジ、スキルの結果、ブラウザーでの検証根拠を確認した。
- 絞り込み機能全体の変更をレビューし、絞り込み PR に Agent Merge を許可した。
- コンテキスト、モデル、共有、任意のクラウドへの委任に使うスラッシュ コマンドを確認した。

## リリースした内容

このワークショップでは、更新した `main` からそれぞれ作成した 2 つのブランチで、2 つの PR を作成しました。

1. **星評価:** 既存の `starRating` と未評価状態をゲーム カードに表示します。
2. **絞り込み機能と品質ワークフロー:** 絞り込み機能を実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して関連するテストを追加します。

絞り込み機能の計画から PR の作成まで、同じ会話とブランチを使いました。ワークショップを簡潔にするため、これらの作業を 1 つの PR にまとめています。

## さまざまな検証方法

自動テスト、自分で行うブラウザー確認、MCP を使った Copilot のブラウザー操作でコードを確認しました。`quality-checks` スキルはユニット テスト、lint、型チェックを実行し、新しい形式で報告しました。QA は PR の作成前に、要件とテスト カバレッジのレビューにその結果を加えました。

追加するテストは、実際に不足しているカバレッジを補うものである必要があります。追加のテストが不要という QA の結果が正しい場合もあります。マージを許可する前にコードと根拠を確認し、変更後は影響する検証結果を更新してください。

## ベスト プラクティス

Copilot に与えるコンテキストとツールが、その作業を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成して MCP サーバーを構成しました。これらのカスタマイズは会話をまたいで再利用し、チームのニーズに合わせて調整できます。指示は標準を設定し、スキルは繰り返し実行するタスクを記述し、カスタム エージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。実装前に進め方を検討するには **Plan**、範囲を絞った変更で状況を把握しながら進めるには **Interactive**、範囲が明確なタスクには **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業には高性能なモデルを選びます。

ツールの整備と同様に、コンテキストも重要です。何を、なぜ、どのように作るかを明確に伝えると、出力が大きく変わります。

## さらに確認する

主なワークフローを学びました。Copilot CLI には、次のような機能もあります。

- `/review` でコード レビュー エージェントに変更を分析してもらう。
- `/rubber-duck` で問題について話し合い、別の視点を得る。
- `/fleet` で独立したサブタスクを並行して調整する。
- `/worktree` で別のタスクを分離する。
- `/delegate` で Copilot cloud agent にタスクを送信する。

## 次のステップ

どのツールも、使い続けることが上達への近道です。本番コード、趣味のコード、長年作りたいと思っていた小さなアプリなどで活用してください。チームと知見を共有し、互いに学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、任意の [Foundry Backer Concierge シリーズ][foundry]に進んでください。別の環境も確認する場合は、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を参照してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプル リクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
