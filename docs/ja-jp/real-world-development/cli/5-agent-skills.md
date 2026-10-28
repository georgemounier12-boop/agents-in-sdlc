---
title: "レッスン 5 - quality-checks スキルのカスタマイズと実行"
description: "既存の quality-checks スキルを確認し、結果の報告形式をカスタマイズして、フィルター機能の検証に使用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書く作業には、コードを書くこと以外にもさまざまな作業が伴います。コードが動作することを手動で検証し、指示ファイルでコーディング標準に従うようにしました。では、テストや lint、継続的インテグレーション (CI) のほかの作業はどうでしょうか。

このようなタスクには、**エージェントスキル**が適しています。スキルを使うと、こうした処理を適切に実行する方法を Copilot に伝えられます。

このレッスンでは、次の内容を学習します。

- 既存の `quality-checks` スキルを確認する。
- 結果の報告形式をカスタマイズする。
- スキルを再読み込みして実行する。

## シナリオ

Tailspin Toys は、単体テスト、lint、型チェックに `quality-checks` スキルを使います。チームは、結果を読みやすくするためにレポートを改善したいと考えています。

## 指示、スクリプト、リソース

エージェントスキルは、エージェントが必要に応じて読み込む、再利用可能なタスク指示、実行可能なスクリプト、補助リソースをまとめたものです。基本的には、スキル名のフォルダーと、その中にある `SKILL.md` という Markdown ファイルで構成されます。Markdown には、スキルの名前と説明を定義するフロントマター、概要、呼び出すタイミングのガイダンスが含まれます。フォルダーには、呼び出し時に使うスクリプトやその他のリソースを格納するサブフォルダーも追加できます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。この例では、テストと lint の実行に `npm` コマンドを使うため、補助ファイルは必要ありません。

スキルをプロジェクトの `.github/skills` フォルダーに置くと、チームで共有して再利用できるリポジトリ資産になります。また、ユーザーのスキル フォルダー (`~/.copilot/skills`) に配置することもできます。

## スキルを確認する

Tailspin Toys チームが単体テスト、lint、型チェックの実行用に作成した `quality-checks` スキルを確認しましょう。

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭にある `name` と `description` を読みます。説明は Copilot がスキルを呼び出すタイミングを判断するのに役立ちます。
3. 指示を読み、テストと lint の手順を Copilot にどのように案内しているかを確認します。
4. スキルに **Results output formatting** セクションがまだないことを確認します。

## 変更前にスキルを実行する

スキルは Copilot CLI から直接、または自然言語を使って呼び出せます。スキルに3種類のチェックを実行するよう依頼します。

1. フィルター機能の会話に戻り、**Interactive** モードにします。
2. 次のプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

何を実行したか、成功したか、ツールが実際に何を報告したかがわかる、よりよいレポートを作成するようスキルを更新しましょう。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

変更したスキルを試してみましょう。Copilot CLI は会話を再起動せずに、編集後のスキルを再読み込みできます。

1. 次を入力します。

   ```plaintext
   /skills reload
   ```

2. 先ほどと同じプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

既存のエージェントスキルをカスタマイズして実行しました。このレッスンでは、次の内容を学習しました。

- 単体テスト、lint、型チェック用の `quality-checks` スキルを確認した。
- 結果の報告形式をカスタマイズした。
- スキルを再読み込みして実行した。

この変更はフィルター機能と一緒に PR に含めます。次は、Copilot が Playwright MCP サーバーを通じてサイトを直接操作できるようにします。[Playwright MCP サーバーでの検証][next-lesson]に進みます。

## ほかのスキルの例

以下のコミュニティの例は参考資料であり、このレッスンで追加するタスクではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューションのワークフロー: `make-repo-contribution`][contribution-example]
- [要件文書: `prd`][prd-example]
- [図と同梱のエクスポートスクリプト: `drawio`][drawio-example]
- [ブラウザーテスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
