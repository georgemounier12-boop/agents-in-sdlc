---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプル リクエスト、再利用できるカスタマイズ、さらに学ぶためのリソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys の一連のワークフローで GitHub Copilot CLI を使いました。次の作業を行いました。

- Codespace を準備し、Copilot CLI をインストールし、プロジェクトを確認して、登録済みのフィルター Issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプル リクエスト (PR) を手動でマージしました。
- フィルター Issue に取り組み、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、単体テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を確認しました。
- 品質保証 (QA) のカスタム エージェントを作成して選択し、要件、テスト カバレッジ、スキルの結果、ブラウザーでの確認結果を評価しました。
- フィルター機能の変更全体をレビューし、フィルター PR に Agent Merge を許可しました。
- コンテキスト、モデル、共有、任意のクラウド委任に使えるスラッシュ コマンドを確認しました。

## 作成したもの

このワークショップでは、更新された `main` からそれぞれ別のブランチを作成し、2 つの PR を作成します。

1. **星評価:** 既存の `starRating` と未評価の状態をゲーム カードに表示します。
2. **フィルターと品質ワークフロー:** フィルターを実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して、関連するテストを追加します。

フィルターの計画から PR の作成までは、同じ会話とブランチを使いました。ワークショップを簡潔にするため、これらの作業を 1 つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分でのブラウザー確認、MCP を通じた Copilot のブラウザー操作など、複数の方法でコードを確認しました。`quality-checks` スキルは単体テスト、lint、型チェックを実行し、追加した形式で結果を報告しました。QA エージェントは、それらの結果と要件やテスト カバレッジのレビューをまとめて、PR を確認しました。

追加するテストは、実際の不足を補うものである必要があります。QA の実行で新しいテストが不要と判断されることもあります。マージを許可する前に、コードと検証結果を確認してください。変更を加えた場合は、影響を受ける検証をもう一度実行してください。

## ベスト プラクティス

Copilot に与えるコンテキストとツールが、作業の結果を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを構成しました。これらのカスタマイズを会話間で再利用し、チームのニーズに合わせて調整できます。指示は標準を設定し、スキルは繰り返し実行するタスクを説明し、カスタム エージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて**モードとモデル**を選びます。実装前に方針を考えるには **Plan**、範囲を絞った変更を対話しながら行うには **Interactive**、範囲が明確なタスクには **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

インフラと同じくらい、コンテキストも重要です。何を作るか、なぜ必要か、どのように作るかを明確に説明すると、結果が大きく変わります。

## さらに確認する

基本的なワークフローを学びました。Copilot CLI には、次のような便利な機能もあります。

- `/review` を使い、コード レビュー エージェントに変更を分析してもらいます。
- `/rubber-duck` を使って問題を整理し、別の視点を得ます。
- `/fleet` を使って、独立したサブタスクを並行して調整します。
- `/worktree` を使って、別のタスクを分離します。
- `/delegate` を使って、タスクを Copilot cloud agent に送信します。

## 次のステップ

どのツールも、使い続けることで上達します。実際の製品コード、趣味のコード、何年も作りたいと思っていた小さなアプリなどで活用してください。チームで学んだことを共有し、互いの経験から学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、任意の [Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境でのワークショップは、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を参照してください。

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
