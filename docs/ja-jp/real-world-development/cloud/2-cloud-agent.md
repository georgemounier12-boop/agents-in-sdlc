---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

ほとんどの組織は、技術的負債に悩まされています。未解決のセキュリティの問題、更新が必要なレガシ コード、実装する時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot の cloud agent は、コードの更新や機能追加といったタスクを自律的に実行できます。作業が完了すると、人間の開発者がレビューできるドラフト PR を作成します。これにより、面倒な作業を任せて開発を加速し、より大きな課題に集中できます。

Copilot cloud agent を使って、次の内容を学びます。

- コードを生成する環境をカスタマイズする
- 操作を安全に行う
- 範囲が明確な Issue の重要性
- Copilot に Issue を割り当てる

## シナリオ

Tailspin Toys には解決したい技術的負債があります。最初のサイトを作成した委託先がドキュメントを適切に整備しなかったため、ドキュメントがほとんどありません。最初の作業として、アプリケーションのすべてのエクスポート関数に TSDoc のドキュメント コメントを追加します。

また、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリの他のタイトルを「関連ゲーム」として表示し、閲覧を続けられるようにします。まだ仕上げる必要はなく、UX の受け入れテストに使える状態にすることが目的です。現在は作業の妨げになっていますが、優先度がより高い Issue もあります。

どちらも、優先順位が下がりやすいタスクです。Copilot cloud agent に割り当てるのに適しています。cloud agent が非同期に作業している間、開発者は別のタスクに集中し、後から成果をレビューして期待どおりか確認できます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドでタスクを実行できます。人間の開発者に仕事を依頼するときと同じように、[GitHub Issue を Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)方法など、複数の方法で作業を依頼できます。割り当て後、Copilot は進捗を追跡するドラフト プル リクエストを作成し、環境をセットアップしてタスクに取り組み始めます。作業中または完了後にセッションを詳しく確認できます。提案された解決策のレビュー準備ができると、プル リクエストで通知されます。

## 適切な範囲の指示が重要

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な作業でも、詳しく調べると複雑なことがあります。[Copilot cloud agent に依頼するタスクの範囲を慎重に決めてください][cloud-agent-best-practices]。AI ペア プログラマーとして扱い、段階的に進め、学び、試し、必要に応じて調整します。生成 AI を取り入れても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

以前の演習では、カスタム指示と Copilot への影響を確認しました。カスタム指示の演習をすでに完了した場合は、`.github/copilot-instructions.md` が機能することを確認しています。まだの場合は、内容を簡単に確認しましょう。cloud agent に作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認して、後で指示の影響を見つけられるようにします。

リポジトリの GitHub Web UI で、または起動済みの Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクション、特に TypeScript の規約と TSDoc ドキュメント コメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` フロントマターを確認します。この指示は `**/*.test.ts` ファイルに適用されます。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てると、作成されたプル リクエストに TSDoc コメント、TypeScript の規約、コメント ヘッダーが反映されます。これらの内容は指示ファイルに基づいています。後の演習で PR をレビューするときに、もう一度確認します。

## Copilot cloud agent の開発環境をセットアップする

コードの作成には、誰が作業する場合でも、通常は環境やセットアップ スクリプトが必要です。タスクを Copilot に割り当てる場合も同様です。Copilot はソフトウェア エンジニアのように作業します。

cloud agent は [GitHub Actions][github-actions] を使って作業環境を構成します。作業開始前に実行する[セットアップ ワークフロー][setup-workflow]を `.github/workflows/copilot-setup-steps.yml` に作成すると、必要な開発ツールと依存関係を利用できるようになります。このラボの流れを整え、学習に集中できるよう、あらかじめ構成されています。cloud agent が Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー バイナリ、単一 Astro アプリの移行・シード済み SQLite データベースを利用できるようにします。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 必要な手順に限定して、できるだけ少ない権限を設定します。Copilot は操作用のトークンを別に受け取ります。
    permissions:
      # 依存関係のインストールなどでセットアップ手順中にリポジトリをクローンする場合は、`contents: read` 権限が必要です。クローンしない場合は、手順の完了後に Copilot が自動でクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ — Node.js (アプリ全体は Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ローカル SQLite データベースを移行してシード データを追加します。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローと同じように見えますが、重要な点がいくつかあります。

- `copilot-setup-steps` という 1 つのジョブがあります。このジョブは、Copilot がプル リクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot が実行するのを待たずに、正常に動作するかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く認識されていますが、多くのプロジェクトでは情報が古かったり、まったくなかったりします。このような技術的負債は放置されやすく、生産性を低下させ、コードベースの保守や新しいメンバーの受け入れを困難にします。Copilot はドキュメントの作成を得意とするため、この Issue は Copilot cloud agent に割り当てるのに適しています。バックグラウンドで必要なドキュメントを生成します。後の演習で成果をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して、新しい Issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

    ![Issue を Copilot に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

    ![Copilot の割り当て詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. 新しく作成されたプル リクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のような内容です。一覧に PR が表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が TODO リストを作成したことを確認できます。

> [!NOTE]
> Copilot の TODO リストが PR に表示されるまで数分かかることがあります。Copilot は前述のワークフローを実行して環境を作成し、プロジェクトを分析して、問題に取り組む方法を決めています。

13. リストと、実行予定のタスクを確認します。
14. プル リクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

    ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示を確認するには、ウィンドウを更新する必要がある場合があります。

16. ライブ セッションをスクロールし、Copilot が問題を解決している様子を確認します。コードを調べて状態を理解し、方針を考えて決め、コードを作成する手順が表示されます。

作業には数分かかります。Copilot cloud agent の主な目的は、タスクを非同期に実行して、ほかの作業に集中できるようにすることです。この機能を活用し、もう 1 つのタスクも cloud agent に割り当てたら、アプリへの機能追加に取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の大きな利点は、作業を分けて、エージェントが 1 つのタスクに取り組む間に別の作業を進められることです。デザイン チーム向けの関連ゲームの追加に時間がかからないとしても、その時間をほかの作業に使えます。cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して、空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

    ![Issue を Copilot に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Issue の最初のコメントに 👀 が表示され、Copilot が作業を始めたことがわかります。

![Copilot が作業中であることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組んでいます。Copilot cloud agent はソフトウェア エンジニアと同じように作業するため、常に監視する必要はありません。完了後にレビューできます。次はカスタム エージェントを作成して使います。

## まとめと次のステップ

このレッスンでは [GitHub Copilot cloud agent][copilot-agents] を確認しました。cloud agent に Issue を割り当てると、非同期に作業できます。技術的負債への対応、新しい機能の作成、フレームワーク間のコード移行などに利用できます。

このレッスンでは、次の内容を学びました。

- コードを生成する環境をカスタマイズする
- 操作を安全に行う
- 範囲が明確な Issue の重要性
- Copilot に Issue を割り当てる

cloud agent がバックグラウンドで作業している間に、次はカスタム エージェントの作成と使用を確認します。[Copilot cloud agent は MCP サーバーも使用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
