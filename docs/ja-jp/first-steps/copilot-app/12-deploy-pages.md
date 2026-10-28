---
title: "レッスン 12 - GitHub Pages でサイトをデプロイする"
description: "GitHub Copilot app を使って Pages のワークフローを設定し、プル リクエストをレビューして、公開された Space Quiz を確認します。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Space Quiz を GitHub Pages にデプロイして、誰でもアクセスできるようにしましょう。Copilot はワークフローの準備やデプロイを支援しますが、変更をレビューし、サイトが公開されたことを確認するのはあなたの役目です。

このレッスンでは、次の内容を学びます。

- Copilot に GitHub Pages の設定を依頼します。
- マージする前に、ワークフローとプル リクエストをレビューします。
- デプロイの状況を追跡し、公開されたサイトを確認します。

## シナリオ

Space Quiz はすでに GitHub リポジトリにありますが、まだ簡単に試せる場所がありません。単一ファイルのサイトを GitHub Pages で公開すれば、サーバーやランタイム依存関係を追加せずに、公開 URL からアクセスできるようになります。

## Copilot にサイトのデプロイを依頼する

`space-quiz` の Copilot app セッションで、次のプロンプトを送信します。

```plaintext
Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![3 段階の図: マージ済みの space-quiz プル リクエスト、Build website・Upload artifact・Deploy to Pages の各ステップが成功した GitHub Actions の Deploy website 実行、および yourname.github.io/space-quiz/ で公開された Space Quiz。](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> このプロンプトは、エージェントにプル リクエストの作成とマージ、およびリポジトリの Pages 設定の変更を依頼します。続行する前に、差分、デプロイ権限、GitHub の承認プロンプトを確認してください。

## ワークフローをレビューしてデプロイを確認する

1. README は、リポジトリにまだ存在しない場合にのみ追加されていることを確認します。
2. ワークフローのビルド手順とデプロイ手順を含め、プル リクエストの変更をレビューします。
3. 必須のプル リクエスト チェックが成功した後にのみマージします。
4. GitHub Actions のデプロイ実行が成功するまで状況を確認します。
5. 報告された URL を開き、Space Quiz が読み込まれることを確認します。

エージェントが Pages の有効化やプル リクエストのマージを行えない場合は、提示された具体的な手順に従います。デプロイが成功し、公開サイトが読み込まれるまでは、作業が完了したと判断しないでください。

## まとめと次のステップ

Space Quiz がライブサイトとして公開され、エージェントの報告に頼らずにデプロイを確認できました。次は[レッスン 13: レビューと次のステップ][next-lesson]に進みます。

[next-lesson]: ../13-review/
