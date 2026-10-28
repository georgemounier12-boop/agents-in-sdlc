---
title: "レッスン 1 - GitHub Copilot CLI のインストール"
description: "Codespace に Copilot CLI をインストールして認証し、基本操作を確認して、用意されたフィルター機能の Issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作する強力なエージェント型コーディング アシスタントです。コマンド ラインからコードベースを調べ、コードを生成し、コマンドを実行し、外部ツールと連携できます。タスクを任せたり、変更を依頼したりしながら、作業に集中できます。まずはツールをインストールします。使い慣れたツールで簡単にインストールできます。

このレッスンでは、次の内容を学習します。

- npm を使って GitHub Copilot CLI をインストールする。
- GitHub アカウントで認証する。
- ワークショップのリポジトリを信頼し、簡単な会話を試す。
- 組み込みの GitHub MCP サーバーを使って、フィルター機能の Issue を見つける。

## シナリオ

チームでは、増え続けるバックログに対応するために AI エージェントを使い始めています。Copilot CLI は、多くの開発者が日常的に使うターミナルでその機能を利用できるようにします。このレッスンではインストールと認証を済ませ、ワークショップの残りで使える状態にします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew でインストールできます。GitHub Codespaces には Node.js があらかじめインストールされているため、ここでは npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされ、バージョン要件を満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されます。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

初めて起動すると、Copilot CLI は GitHub アカウントでの認証を求めます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. プロンプトが表示されたら、デバイス コードを使った認証手順に従い、Copilot CLI を認証して承認します。
3. Copilot CLI に次のプロンプトが表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 表示されたパスが Tailspin Toys リポジトリであることを確認し、**Yes, and remember this folder for future sessions** を選択して「はい」と答えます。

> [!NOTE]
> Codespace では、GitHub セッションを通じてすでに認証されている場合があります。Copilot CLI が認証を求めずに起動した場合は、そのまま進めて問題ありません。

## 基本操作を確認する

通常のシェル プロンプトで入力したコマンドは、Codespace で直接実行されます。Copilot CLI の起動後は、自然言語の入力がエージェントに送られ、スラッシュ コマンドで会話を操作します。

1. `/model` を入力し、矢印キーで **Auto** を選択して <kbd>Enter</kbd> を押し、もう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` を入力して、インストール済みのバージョンで利用できるコマンドを確認します。次に <kbd>Esc</kbd> を押してヘルプ画面を閉じます。
3. Copilot に簡単な質問をして、すべてが動作していることを確認します。

   ```plaintext
   What are the key files in this project?
   ```

4. 回答を読み、Copilot が回答の前にリポジトリを調べることを確認します。
5. `/mcp list` を入力し、組み込みの GitHub MCP サーバーが利用できることを確認します。
6. Copilot にフィルター機能の Issue を探すよう依頼します。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. URL を開いて Issue を読みます。最初の小さな変更を完了した後、この Issue に取り組みます。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトしているブランチ上で動作し、自動的に worktree を作成することはありません。変更を加える前に、機能ブランチを作成します。

## ワークショップ用のショートカットを使う

Copilot CLI は、設定済みの権限の範囲外でツールを使う前に、通常は確認を求めます。このワークショップでは `--yolo` を使って再起動します。ユーザーが承認したこのショートカットを使うと、Codespace 内での承認プロンプトがなくなり、演習に集中できます。

> [!CAUTION]
> `--yolo` は、`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls` を有効にし、すべての権限を自動的に許可します。Codespace や VM などの隔離された環境でのみ使用し、日常の開発で使う既定のエイリアスにはしないでください。詳しくは[ツールの使用を許可または拒否する方法][allow-all-warning]を参照してください。

このワークショップでは、後のレッスンで Issue やプル リクエストを操作するため、`--enable-all-github-mcp-tools` を使って GitHub MCP の読み取り・書き込みツールを有効にします。Codespace によってローカル コンピューターへのアクセスは制限されますが、認証済みの GitHub リソースには引き続きアクセスできます。変更を公開またはマージする前に内容を確認してください。

1. `/exit` で Copilot CLI を終了します。
2. リポジトリのルートから再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. プロジェクトについてもう一度簡単な質問をし、会話が動作していることを確認してから `/exit` で終了します。

Copilot は会話を自動的に保存します。後で指示を変更したりエージェントを追加したりした後、`copilot --resume` を使って、同じ機能開発の会話とブランチに戻ります。

## まとめと次のステップ

GitHub Copilot CLI をインストールして認証し、ワークショップのリポジトリを信頼して簡単な会話を試しました。また、組み込みの GitHub MCP サーバーを使ってフィルター機能の Issue を見つけました。

- npm を使って GitHub Copilot CLI をインストールする。
- GitHub アカウントで認証する。
- ワークショップのリポジトリを信頼して、簡単な会話を試す。
- 組み込みの GitHub MCP サーバーを使って、フィルター機能の Issue を見つける。

次は、最初の変更に取り組み、ゲームカードに星評価を表示します。[最初の変更を始める][next-lesson]に進みます。

## リソース

- [GitHub Copilot CLI のインストール][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
