---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有バックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用します。変更の計画、コードの作成、動作のテスト、プル リクエストのレビュー、エージェントを使った作業管理を繰り返しながら進めます。

## シナリオ

あなたは、開発者をテーマにしたボードゲームのクラウドファンディングを運営する架空の会社、Tailspin Toys に新しく加わった開発者です。チームのバックログは GitHub の Issue として登録されており、すぐに着手できます。フィルターやページネーションなどの機能開発に加えて、アクセシビリティやコーディング標準などの品質改善も含まれています。

## 作業環境を選ぶ

GitHub Copilot は、どの環境でも利用できます。開発に使う環境を選択してください。各ワークショップでは、それぞれの環境をセットアップし、共通の Tailspin Toys シナリオに取り組みます。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使います。Plan モードと Autopilot モード、指示、スキル、カスタム エージェント、Playwright MCP、Agent Merge、実用的なスラッシュ コマンドを活用します。

### [GitHub Copilot app][app]

デスクトップ アプリでエージェント セッションを並行して実行し、セッション モードを切り替え、Canvas で共同作業を行い、GitHub の Issue とプル リクエストを管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub Issue を非同期エージェントに割り当て、作業を案内して進捗を監視し、エージェントが作成したプル リクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。エディターを離れずに、Copilot Chat の agent mode、MCP サーバー、カスタム エージェントを活用します。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
