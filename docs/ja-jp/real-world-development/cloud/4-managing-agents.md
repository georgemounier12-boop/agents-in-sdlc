---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

直前の演習では、ユーザー エクスペリエンスの改善と機能追加を目的とする 3 つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクを管理するには、GitHub.com の[エージェント ページ][agents-page]など、さまざまなツールを利用できます。この mission control では、オープンなプル リクエスト (PR) があるすべてのエージェント タスクを確認できます。実行内容を調べたり、進行中のセッションに追加の指示を送ったりできます。

このレッスンでは、次の内容を学びます。

- エージェント ページで cloud agent のタスクを監視します。
- 進行中のセッションに指示を送り、機能の追加を依頼します。

## シナリオ

ハイ コントラスト モードの作成をエージェントに依頼した後、チームはライト モードも追加するよい機会だと考えました。サイトのスタイル更新と切り替え機能をすでに作業中なので、ライト モードも追加するのが自然です。ハイ コントラストに加えてライト モードも実装されるよう、エージェントの作業を調整します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認しましょう。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メイン ペインと左側のペインにあるタスク一覧を確認します。次のような、Copilot に割り当てたタスクが表示されます。
   - コードのドキュメントを更新する
   - ゲームを変更するための API を生成する
   - Web サイトにハイ コントラスト モードを追加する
3. 実行中のタスクを 1 つ選択し、Copilot が実行した作業を確認します。次のような操作が含まれます。
   - リポジトリからコードをチェックアウトする
   - Copilot が作業する環境を作成する
   - MCP サーバーをセットアップする
   - 割り当てられたタスクを完了するための手順を実行する

> [!NOTE]
> 表示される手順は、Copilot の作業状況や進め方によって異なります。

4. 右側に表示されるプル リクエスト (PR) ペインも確認します。ここでは、PR と変更されたファイルを確認し、作業をさらに監視できます。

## cloud agent の作業を調整する

現在実行中のタスクを確認できました。次は、ハイ コントラスト モードの作業中にライト モードの切り替えも追加するよう Copilot に依頼します。

1. ハイ コントラスト モードの追加に関連するセッションを選択します。正確なタイトルは Copilot が付けた名前や作業状況によって異なります。

    ![mission control のアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して作業が始まるまで、数分間セッションを確認します。次のようなメッセージが表示されたら、作業が始まっています。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot が作業中のセッションを調整するダイアログが強調表示された、エージェント ページの cloud agent タスク画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業計画に組み込む様子を確認します。

## Copilot の作業を待つ

前と同じように、Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で実行中の手順が終わると、新しい依頼を作業の流れに追加します。

ここでも完了まで数分かかります。これまでに学んだことを振り返りましょう。

## まとめと次のステップ

このレッスンでは、GitHub Copilot cloud agent のタスクを監視し、案内する中心的な場所である Copilot agents ページを確認しました。この mission control では、実行中と完了済みのタスクを追跡し、作業内容を確認し、進行中のタスクの範囲を変更したり、追加のガイダンスを提供したりできます。

このレッスンでは、次の内容を学びました。

- mission control とエージェント ページで cloud agent のタスクを監視しました。
- 進行中のセッションに追加機能を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、[Copilot が作成したプル リクエストを改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
