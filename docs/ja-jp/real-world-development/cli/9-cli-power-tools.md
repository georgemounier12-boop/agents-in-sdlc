---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュ コマンド"
description: "コンテキストの管理、モデルの選択、セッションの共有、任意のクラウド委任に使えるスラッシュ コマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

一般的な CLI ツールと同様に、GitHub Copilot CLI には対話に使えるスラッシュ コマンドが多数用意されています。これらのコマンドを使うと、高度な機能、内部情報、追加の構成オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などのコマンドを使いました。ここでは、ほかの便利なコマンドを確認しましょう。

このレッスンでは、次の内容を学びます。

- `/context` と `/compact` を使い、Copilot が会話のコンテキストを管理する方法を確認します。
- `/model` を使い、利用できるモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並列作業、worktree、cloud agent への委任に使える任意のコマンドを確認します。

## シナリオ

CLI の基本的なワークフローを終えました。次に、コンテキストの管理、モデルの切り替え、セッションの共有、任意の [Copilot cloud agent][about-cloud-agent] への作業委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクに取り組むと、モデルの最大コンテキスト ウィンドウに達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。また、スラッシュ コマンドを使って状態を確認したり、自分で圧縮したりできます。

1. Codespace に戻り、Copilot CLI がまだ開いていない場合は、リポジトリのルートから起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域にコンテキストがどのように分けられているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力し、結果を比較します。会話が短い場合は、大きな違いが見られないことがあります。

> [!NOTE]
> コンテキスト ウィンドウが埋まると、Copilot CLI は自動的にコンテキストを圧縮します。自分でタイミングを決める場合は `/compact` を使います。関係のない別のタスクに切り替えるために新しい会話を始める場合は、代わりに `/clear` または `/new` を使います。

## モデルを選ぶ

モデルごとに得意分野が異なり、開発者によって好みも異なります。Copilot CLI では、利用できるモデルを一覧表示し、使うモデルを選択できます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用できるモデルと使用量に関する情報を確認します。
3. 現在のモデルをそのまま使うか、別のモデルを選択するか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力して知見を共有すると、AI ツールの活用方法を互いに学べます。`/share` コマンドを使うと、セッションを Markdown ファイルまたは HTML ファイルにエクスポートしたり、共有可能なリンクを作成したり、GitHub gist として公開したりできます。

1. `/help` を入力し、インストール済みのバージョンで利用できる `/share` オプションを確認します。
2. セッションを共有する場合は、用途に合った出力先を選択します。たとえば `/share file` はローカルの Markdown ファイルにエクスポートします。
3. エクスポートした内容を誰かに送信したり公開したりする前に確認します。セッションのエクスポートには、プロンプト、応答、プロジェクトの詳細が含まれることがあります。

リンクや gist の公開は任意です。チームが共有を意図していないリポジトリや会話の内容は公開しないでください。

## 任意: 作業を並列化または委任する

ワークショップの主要な内容は完了しました。大きなタスクでは、Copilot CLI の次のコマンドも利用できます。

- `/fleet` は、独立したサブタスクを複数のサブエージェントに分割して並列に実行できます。
- `/worktree` は、別のタスク用に独立した Git worktree を作成できます。
- `/delegate` は、非同期で作業し、プル リクエストを作成することもある Copilot cloud agent にタスクを送信できます。

これらのコマンドを使うと、追加の worktree やリモート作業が発生することがあるため、任意の操作です。試すときは、範囲を明確にした新しいタスクから始め、通常のワークフローで結果をレビューしてください。非同期エージェントの作業を詳しく学ぶ場合は、[cloud agent ワークショップ][cloud-workshop]に進みます。

## まとめと次のステップ

Copilot CLI のスラッシュ コマンドを使うと、CLI の構成、セッションの共有、内部動作の確認ができます。このレッスンでは、次の内容を学びました。

- `/context` と `/compact` を使い、Copilot がコンテキストを管理する方法を確認しました。
- `/model` を使い、利用できるモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並列作業、worktree、cloud agent への委任に使える任意のコマンドを確認しました。

Copilot CLI には、ほかにも確認できるスラッシュ コマンドがあります。次は、これまでに学んだことを[振り返り][next-lesson]、さらに学ぶための次のステップを確認しましょう。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
