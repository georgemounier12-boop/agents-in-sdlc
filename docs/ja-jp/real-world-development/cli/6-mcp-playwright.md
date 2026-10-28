---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または構成し、ブラウザーでフィルター機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

すでに説明したように、コードを書く作業には単にコードを書く以上のことが含まれます。データや外部サービスを扱い、Copilot が使える自動化機能を追加する必要もあります。そこで利用するのが MCP サーバーです。MCP サーバーを使うと、Copilot は CLI に組み込まれている機能だけでなく、追加のツールやサービスも利用できます。

このレッスンでは、次の内容を学びます。

- Model Context Protocol (MCP) と、Copilot CLI での使用方法を学びます。
- まだ利用できない場合は、Playwright MCP サーバーを追加します。
- エージェントにブラウザーを操作させ、フィルター機能を試します。

## シナリオ

単体テストやエンドツーエンド テストは重要ですが、UI の変更を検証するには、実際に UI を操作する必要があります。Copilot がユーザーと同じように作業中の Web サイトを使えるようにすると、変更の動作を確かめる手順をさらに自動化でき、期待どおりに動作するという確信が高まります。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、AI エージェントは外部のツールやサービスとリアルタイムで通信できます。最新情報にアクセスしたり、依頼に応じて操作を実行したりできます。

これらのツールやリソースには、MCP サーバーを通じてアクセスします。MCP サーバーは AI エージェントと外部のツールやサービスをつなぎます。それぞれの MCP サーバーが、AI エージェントに提供するツールとリソースを定義します。

よく使われる MCP サーバーには、次のものがあります。

- **[GitHub MCP Server][github-mcp]:** GitHub リポジトリ、Issue、プル リクエストを管理する API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]:** Playwright を使ったブラウザー自動化機能を提供します。

ほかにも多くの MCP サーバーがあります。GitHub は、エコシステム内での発見や貢献を促進するために [MCP registry][mcp-registry] を運営しています。

> [!CAUTION]
> MCP サーバーは、プロジェクト内の他の依存関係と同様に扱ってください。使用前にソース コードを確認し、発行元を検証して、セキュリティへの影響を検討してください。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は、Copilot がユーザーと同じように Web サイトを操作できるように追加します。

> [!NOTE]
> Copilot CLI で Playwright MCP がすでに構成されている場合は、セットアップ手順を省略して次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバーの追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前として `playwright` を入力し、<kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押してサーバーの種類として **STDIO** を確定し、<kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次のコマンドを貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright を使って Copilot に機能を試してもらう

前のレッスンでは、機能が期待どおりに動作することを手動で確認しました。次は、Copilot で利用できる Playwright MCP サーバーを使って同じ確認を行います。

1. 次のプロンプトを使い、Playwright MCP サーバーで機能を検証するよう Copilot に依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Copilot は通常、自動的に MCP サーバーを選択するため、明示的に使うよう指示する必要はありません。ただし、使用するツールがわかっている場合は、指定しておくと一貫性を高め、トークンを節約できます。

2. Copilot がブラウザーで操作を行い、機能の動作を確認する手順を表示する様子を確認します。
3. レポートを読み、すべてが期待どおりに動作することを確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作してからサーバーを停止し、レポートを作成します。

## まとめと次のステップ

おめでとうございます。Copilot CLI から Playwright MCP サーバーを使って、実際のブラウザーで機能を確認しました。このレッスンでは、次の内容を学びました。

- Model Context Protocol (MCP) と、Copilot CLI での使用方法を学びました。
- まだ利用できない場合に、Playwright MCP サーバーを追加しました。
- エージェントにブラウザーを操作させ、フィルター機能を試しました。

次は、スキルとブラウザー ツールをまとめた役割を定義するため、[QA カスタム エージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
