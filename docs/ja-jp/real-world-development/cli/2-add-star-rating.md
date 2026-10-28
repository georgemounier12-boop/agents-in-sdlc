---
title: "レッスン 2 - 星評価を追加する: 小さな改善"
description: "Copilot CLI でゲーム カードに小さな変更を加え、転送されたブラウザーで差分を確認して、最初のプル リクエストをマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールし、会話を試しました。次は、プロジェクトに初めて変更を加えます。変更は小さくします。ゲームのデータには星評価がありますが、ホーム ページのゲーム カードにはまだ表示されていません。エージェントに評価を表示させ、変更をレビューして、最初のプル リクエストとしてマージします。

このレッスンでは、次の内容を学びます。

- 機能ブランチで Copilot の会話を始めます。
- 小さな変更を行うようエージェントに依頼します。
- `/diff` を使って変更をレビューします。
- 転送されたブラウザーでアプリを実行し、変更を確認します。
- 最初のプル リクエストを作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、詳細ページにはすでに表示されています。しかし、ホーム ページのゲーム カードに表示されるのは、タイトル、カテゴリ、パブリッシャー、説明だけです。最初の練習として、各カードに既存の評価を表示するようエージェントに依頼します。範囲が小さく独立した変更なので、最初のセッションに適しています。

## 会話の構成

**会話**は、Copilot CLI を使ってタスクに取り組む場所です。Copilot app と異なり、通常の CLI 会話では、専用の worktree ではなく、ターミナルで現在チェックアウトしているリポジトリと Git ブランチを使用します。保存した会話は後から再開できますが、ファイルとブランチは通常の Git 状態としてディスク上にあります。

会話では、次の 3 つを確認できます。プロンプトとエージェントの応答、ファイルを調べて編集するエージェントのツール操作、`/diff` で確認できる変更です。

## 会話を始めて変更を依頼する

機能の実装を始めるため、新しい会話を開始しましょう。

1. Codespace に戻ります。
2. まだターミナルを開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
3. まだ Copilot を起動していない場合は、次のコマンドを実行します。

   ```bash
   copilot --yolo
   ```

4. スラッシュ コマンド `/new` を実行して <kbd>Enter</kbd> を押し、新しいセッションを開始します。
5. 次のプロンプトを使って変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot はゲーム詳細の表示に使うファイルを調べ、必要なコードを作成します。Copilot CLI を使って新しい機能を追加できました。

## 差分をレビューする

小さな変更でも、AI が生成した変更はすべてマージ前にレビューします。Copilot CLI で変更を確認しましょう。

1. `/diff` を入力し、変更されたすべてのファイルを確認します。
2. ゲーム カードに評価がある場合は数値が表示され、`starRating` が `null` の場合は `No rating yet` と表示されることを確認します。
3. どちらの状態もテストで確認されていることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. レビューが終わったら <kbd>Esc</kbd> を押し、差分画面を終了します。

> [!NOTE]
> すべての生成 AI ツールと同様に、Copilot の動作は確率的であり決定論的ではありません。そのため、実際に生成されるコードは異なる場合があります。特定の実装を期待するのではなく、動作をレビューしてください。

## 変更を確認する

コードを読んだだけで動作すると判断するのではなく、Copilot に Web サイトを起動してもらい、Codespaces で転送されたブラウザーから更新された UI を確認します。

1. 次のプロンプトを使ってアプリの起動を依頼します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら **Open in Browser** を選択します。
3. ゲーム カードに 5 段階の評価が表示されていることを確認します。
4. Copilot に戻り、起動したサーバーを停止するよう依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初のプル リクエストを作成してマージする

機能が完成したので、プル リクエスト (PR) を作成して新しいコードをプロジェクトにマージします。

1. 既定のエージェントに変更をコミットするよう依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。Copilot CLI は PR の作成時に既存のコミットをプッシュし、PR の URL を表示します。
3. Copilot CLI が表示した URL を、Mac では <kbd>Command</kbd>、Windows/Linux では <kbd>Ctrl</kbd> を押しながら選択して PR を開きます。
4. 変更されたファイルとチェック結果を確認します。
5. 準備ができたら **Merge pull request** を選択し、マージを確定します。
6. Codespace に戻り、`/exit` を入力して Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

おめでとうございます。GitHub Copilot CLI を使って最初の変更をリリースしました。このレッスンでは、次の作業を行いました。

- 機能ブランチで Copilot の会話を始めました。
- ゲーム カードに小さな変更を加えるようエージェントに依頼しました。
- `/diff` を使って変更をレビューしました。
- 転送されたブラウザーでアプリを実行し、星評価を確認しました。
- 最初のプル リクエストを作成してマージしました。

次は[フィルター Issue から始め、Plan モードと Autopilot モードを使って][next-lesson]、より大きな機能を作成します。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
