---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: カスタム エージェント →][next-lesson]

agent mode でフィルター機能を作成しました。プル リクエストを開く前に、ブラウザーで実際に動作することを確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続して、Copilot に実際のブラウザーを操作させ、機能をテストします。その後、ブランチを公開して PR を作成します。

この演習では、次の内容を学びます。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot に新しいツールを追加する方法を学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot に Playwright MCP サーバーを使わせ、ブラウザーでフィルター機能を手動テストします。
- ブランチを公開し、フィルター作業のプル リクエストを作成します。

## Model Context Protocol (MCP) とは

agent mode は、エディターの外部にある機能を利用すると、さらに強力になります。Copilot は Model Context Protocol (MCP) を使って外部ツールにアクセスします。MCP は、エージェントが外部のツールやサービスと通信するための標準的な方法です。

![agent mode の内部構成と、コンテキスト、LLM、MCP サーバーや VS Code 拡張機能が提供するツールとのやり取りを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は、AI エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、AI エージェントは外部のツールやサービスとリアルタイムで通信できます。リソースを使って最新情報にアクセスしたり、ツールを使って依頼に応じた操作を実行したりできます。

これらのツールやリソースには、MCP サーバーを通じてアクセスします。MCP サーバーは AI エージェントと外部のツールやサービス (既存の API や npm パッケージなどのローカル ツール) 間の通信を管理します。それぞれの MCP サーバーが、AI エージェントに提供するツールとリソースを定義します。

よく使われる MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server):** GitHub リポジトリを管理する API にアクセスできます。AI エージェントはリポジトリの作成や更新、Issue やプル リクエストの管理などを実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp):** Playwright を使ったブラウザー自動化機能を提供します。Web ページへの移動、フォームへの入力、ボタンの選択などを実行できます。

ほかにもさまざまなツールやリソースにアクセスできる MCP サーバーがあります。GitHub は、エコシステム内での発見や貢献を促進するために [MCP registry](https://github.com/mcp) を運営しています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーはプロジェクト内の他の依存関係と同様に扱ってください。使用前にソース コードを慎重に確認し、発行元を検証して、セキュリティへの影響を検討してください。信頼できる MCP サーバーのみを使い、機密リソースや操作へのアクセスを許可する際は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースから利用できる MCP サーバーを構成します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. すでに `github` サーバーが構成されていることを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` エントリはプロジェクト テンプレートに含まれています。そのため、前の演習で Copilot が Issue のバックログを読み取ることができました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使用するため、ローカルにインストールする必要はありません。VS Code は HTTP で接続し、Copilot が初めてツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に、2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が操作できるブラウザーを利用できます。これを使って機能をテストします。

1. `.vscode/mcp.json` で `github` の隣に `playwright` エントリを追加し、ファイルを次のようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーと異なり、`playwright` は**ローカル** サーバーです。VS Code はマシン上で `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグを指定すると、Playwright は表示ウィンドウなしでブラウザーを実行します。デスクトップ環境がない Codespace では、この指定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトではエンドツーエンド テストに Playwright を使用しているため、通常はブラウザーがすでにインストールされています。Copilot がブラウザーがないと報告した場合は、`npx playwright install chromium` を実行して再度試すよう依頼します。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて**起動します。Copilot がサーバーのツールを初めて必要としたときに、VS Code がサーバーを起動し、信頼するか確認します。

1. `.vscode/mcp.json` では、`playwright` エントリの上に **Start** が表示されます。選択して今すぐサーバーを起動し、構成が正しいことを確認します。
2. VS Code からサーバーを信頼するか確認されたら、構成を確認して信頼を選択し、サーバーを起動します。

手動で起動する必要はありません。省略した場合は、次の手順で Copilot が初めてツールを使うときに、VS Code がサーバーを起動して信頼を確認します。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot は実際のブラウザーを操作できます。自分でアプリを操作して確認する代わりに、エージェントがページを開き、移動し、フィルターを適用して、結果を読み取って概要を報告します。会話を離れずに機能の動作を確認できる、最も速い方法です。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティ ツリー][playwright-mcp-server]を使います。エージェントは、構造化されラベル付けされた要素 (ボタン、リンク、リスト項目) を基に判断します。支援技術と同じように情報を扱うため、簡単な機能確認がアクセシビリティの簡易チェックにもなります。

サーバーが接続され、アプリが実行中であることを確認し、次のプロンプトを Copilot に送信してフィルター機能を試します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを通じてブラウザーを起動し、手順に沿って確認した結果を報告します。報告内容を Issue の受け入れ条件と照らし合わせます。問題があれば、プル リクエストを開く前にフォローアップで質問したり、コードを修正するよう依頼したりします。

> [!NOTE]
> テストするには、アプリが `http://localhost:4321` で実行されている必要があります。開発サーバーを停止している場合は、プロンプトを送る前にもう一度起動します。Copilot が Playwright MCP サーバーを初めて使うとき、ブラウザーをダウンロードする場合があります。ブラウザーがないと報告されたら、`npx playwright install chromium` を実行して再度試すよう依頼します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプル リクエストを作成する

機能が動作することを確認できたら、チームのレビュー用にプル リクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルに移動し、Copilot が変更した内容を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミット メッセージを生成します。

   ![変更内容を表示する Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プル リクエストを作成する

github.com や GitHub コマンド ライン インターフェイス (CLI) など、プル リクエストを作成する方法はいくつかあります。GitHub Copilot を使っているので、PR の作成も依頼しましょう。Copilot は関連する Issue を見つけ、PR と関連付けて作成できます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. Copilot が GitHub ツールを使用できるように、エージェントの一覧で **Agent** が選択されていることを確認します。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選択し、Copilot が情報を収集して操作を行うことを許可します。GitHub ツールを初めて使うときに認証を求められる場合があります。プロンプトに従って認証します。
5. Copilot が Issue を検索し、適切なものを見つけて PR を作成することを確認します。
6. Copilot が生成したリンクを選択してプル リクエストをレビューします。まだ**マージしないでください**。

## まとめと次のステップ

おめでとうございます。この演習では、次の作業を行いました。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot に新しいツールを追加する方法を学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- リリース前に Playwright MCP サーバーでブラウザーを操作し、フィルター機能を手動テストしました。
- ブランチを公開し、フィルター作業のプル リクエストを作成しました。

MCP サーバーを使って機能を**テスト**しましたが、MCP は Copilot を拡張する方法の 1 つです。次は[カスタム エージェントを作成し][next-lesson]、アクセシビリティ レビューなどの作業を効率化します。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
