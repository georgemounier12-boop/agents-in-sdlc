---
title: "レッスン 7 - QA エージェントを作成して使用する"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせた QA カスタム エージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザー上のフィルター機能を確認しました。次は、明確な QA プロセスを定義したカスタム エージェントに、これらの機能をまとめます。

このレッスンでは、次の内容を学びます。

- カスタム エージェントが指示、スキル、MCP ツールを使う方法を確認します。
- 再利用可能な品質保証プロファイルを作成して確認します。
- QA エージェントを選択し、フィルター Issue に基づく調査結果をレビューします。

## シナリオ

Tailspin Toys では、プル リクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テスト カバレッジ、ブラウザー上の動作を一貫した方法でレビューしたいと考えています。カスタム エージェントを使うと、QA プロセスを調整し、再利用可能なレポートを作成できます。

## カスタム エージェントとは

カスタム エージェントは、Markdown プロファイルで定義する、専門化された Copilot です。プロファイルでは、エージェントの目的、指示、使用できるツールを定義します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使用したカスタマイズには、それぞれ異なる役割があります。リポジトリの指示はチームの標準を説明し、`quality-checks` スキルは繰り返し実行できるチェックをまとめ、Playwright MCP はブラウザー ツールを提供します。QA プロファイルは、それらを使って要件を評価し、調査結果を報告するよう Copilot に指示します。既存の機能に代わるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能の PR を開く前に、再利用可能な QA プロファイルを Copilot に作成してもらいます。プロファイルでは、QA が実行するチェックと、その作業上の制約を定義します。

1. Codespace に戻り、フィルター機能の会話が Interactive モードであることを確認します。
2. 既定のエージェントに、新しいカスタム エージェントの作成を依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、Copilot が意図した QA ワークフローと制約をプロファイルに反映したことを確認します。確認せずに実行すると、検証だけを行うつもりでも、不完全または範囲の広すぎるエージェントが機能を変更する可能性があります。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. フロントマターを確認します。`description` は必須です。`name` は任意ですが、含めるとエージェントの表示名を明確にできます。
3. プロファイルの指示を読み、QA が要件を確認し、リポジトリの指示に従い、`quality-checks` スキルを実行して、Playwright MCP を使うことを確認します。
4. QA が根拠とともに結果を報告し、実装コードを変更する前に確認を求め、変更をコミットせず、プル リクエストを作成しないことを確認します。
5. 必要な責任や制約が欠けている場合は、続ける前に既定のエージェントに修正を依頼します。

## Issue に対して QA を実行する

Copilot CLI は、会話の開始時にプロジェクト エージェントを読み込みます。プロファイルを作成したら同じフィルター機能の会話を再開し、QA を選択します。そうすると、Issue と計画中に決めた内容を利用できます。

1. 次のプロンプトを使ってエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> エージェントを作成した直後は、利用可能なエージェント一覧に表示されない場合があります。エージェントは存在するため、上記のコマンドで有効にできます。

2. 次のプロンプトを使って、QA エージェントに機能をレビューしてもらいます。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を開始します。
4. 作業が完了したら、作成されたレポートを確認します。

## まとめと次のステップ

ワークフローに再利用可能な専門的な役割を追加し、その作業をレビューしました。このレッスンでは、次の内容を学びました。

- カスタム エージェントが指示、スキル、MCP ツールを使う方法を確認しました。
- 再利用可能な品質保証プロファイルを作成して確認しました。
- QA エージェントを選び、フィルター Issue に基づく調査結果をレビューしました。

実装、スキルの更新、QA プロファイル、テスト、検証レポートがそろいました。次は[それらを機能の PR にまとめ、Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI のカスタム エージェントを作成する][create-agents]
- [カスタム エージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
