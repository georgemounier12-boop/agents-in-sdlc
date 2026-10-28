---
title: "演習 5: GitHub Copilot の作業を反復する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー エクスペリエンスの改善と機能追加を目的とする複数のタスクで GitHub Copilot を使いました。コードへのドキュメント追加、デザイン チームが改善を検討できる関連ゲーム機能の作成、ハイ コントラストとライト モードの切り替えを含むアクセシビリティ機能の実装を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送って作業を改善しましょう。

### シナリオ

これまでに何度も説明したように、生成 AI を導入してもソフトウェア設計や DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスを進めます。チーム全体にレビューを依頼する前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能に関して GitHub Copilot が提案した内容を確認しましょう。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期に、監督を受けずにタスクを実行するため、安全性を維持するセキュリティ制約が設けられています。次のような制約があります。

- Copilot はリポジトリへの読み取りアクセス権を持ち、書き込みできるのはコード用に使用するブランチ**のみ**です。
- cloud agent は GitHub Actions 内で実行され、作業用の独立した一時環境を作成します。
- GitHub Actions ワークフローを実行するには、人間の承認が必要です。
- MCP サーバーを含む[外部リソースへのアクセスは、既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が作成した最初のプル リクエスト (PR)、つまりコードへのドキュメント追加を確認します。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR に GitHub Copilot がファイアウォールでブロックされたという警告が表示される場合があります。Copilot は MCP サーバーなどの外部リソースへのアクセスが既定で制限されているため、これは**想定どおりの動作**です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com でリポジトリに戻ります。
2. **Pull Requests** を選択して、プル リクエストの一覧を開きます。
3. **Add missing documentation** などのタイトルのプル リクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プル リクエストには **[WIP]** フラグが表示されます。その場合は、Copilot の作業が完了するまで待ちます。数分かかることがあるため、休憩したり、これまでに学んだことを振り返ったりしてください。

4. プル リクエストの準備ができたら **Files changed** タブを選び、変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新たに更新されたコードを確認します。TSDoc のドキュメント コメントやその他のドキュメントが含まれます。実際の変更内容は状況によって異なります。

   変更を確認しながら、TSDoc コメント、TypeScript の規約、コメント ヘッダーを探します。これらは演習 2 の開始時に確認したカスタム指示ファイルから反映されています。

6. 更新内容をレビューして問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. 一部のワークフローが承認待ちになっていることを示す表示を確認します。
8. ワークフローが承認待ちの場合は **Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. プル リクエストのチェック セクションにワークフローが追加されます。問題がなければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

プル リクエストで Copilot と作業することは、一方通行ではありません。チーム メンバーに依頼するのと同じように、PR またはコード内のインライン コメントで Copilot をタグ付けできます。Copilot はコメントを確認し、対応する新しいセッションを開始します。結果は確定的ではないため、必ず成功する指示文はありません。次のような変更を依頼できます。

- 各コード ファイルの先頭に、役割を簡潔に説明するコメント ヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc のドキュメント コメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. **@copilot** をユーザーと同じようにタグ付けし、生成されたドキュメントへの変更を依頼するコメントを追加します。上記の例、またはコードベースのドキュメントに関する別の提案を使います。
2. **View Session** を選択し、Copilot の作業を確認します。Copilot が更新用の新しいセッションを開始します。
3. **Back to pull request** を選択すると、プル リクエストに戻ります。

    ![プル リクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot の変更が完了すると、プル リクエストに新しいコミットが表示されます。
5. **Files changed** タブを選択し、変更をレビューします。

問題がなくなるまで反復できます。満足できたら、ドラフト PR をレビュー可能な状態にして、`main` ブランチにマージします。

![PR をレビュー可能な状態にする](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue に対し、Copilot が作成した PR を確認しましょう。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルの PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 更新内容をレビューして問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. 一部のワークフローが承認待ちになっていることを示す表示を確認します。
7. ワークフローが承認待ちの場合は **Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

8. プル リクエストのチェック セクションにワークフローが追加されます。問題がなければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかることがあります。
9. **任意:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストすることもできます。Codespace に移動してターミナルを開き、Copilot が作成したブランチ名 (例: **copilot/fix-8**) に `<branch-name>` を置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。前と同じように、Copilot cloud agent に追加の変更を依頼して反復できます。たとえば、表示する関連ゲームの数を調整する、コメント ヘッダーや TSDoc コメントを追加するといった変更を依頼できます (この Issue はカスタム指示を更新する**前**に割り当てたことを思い出してください)。**Conversation** タブでコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ カスタム エージェントを使って実装した機能をレビューします。この PR には、演習 3 で依頼したハイ コントラスト モードと、演習 4 の mission control で追加を依頼したライト モードの両方が含まれています。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プル リクエストには **[WIP]** フラグが表示されます。その場合は、Copilot が作業を完了するまで待ちます。数分かかることがあります。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 実装を確認し、特に次の点に注意します。
   - モードを切り替える UI コンポーネント
   - ユーザー設定を保存するためのローカル ストレージの使用
   - ハイ コントラストとライト モードの CSS またはスタイルの変更
   - アクセシビリティ属性 (ARIA ラベル、キーボード ナビゲーションなど)
   - モードの切り替えを管理する JavaScript/TypeScript コード

6. 更新内容をレビューして問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. 一部のワークフローが承認待ちになっていることを示す表示を確認します。
8. ワークフローが承認待ちの場合は **Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. プル リクエストのチェック セクションにワークフローが追加されます。問題がなければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかることがあります。
10. **任意:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、Copilot が作成したブランチ名に `<branch-name>` を置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    次にアプリケーションを起動し、ブラウザーでハイ コントラストとライト モードの切り替えをテストします。ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティ カスタム エージェントが、アクセシビリティのベスト プラクティスに沿ってこれらの機能を実装するよう Copilot をガイドしたことを確認します。アクセシビリティ上の問題や改善点があれば、前の PR と同様にコメントで **@copilot** をタグ付けして変更を依頼できます。

## 任意の演習 — 委任を続ける

cloud agent は、実際のバックログ項目を渡して別の作業に集中できるときに効果を発揮します。習慣を身に付けるために、リポジトリに Issue を追加し、Copilot に割り当ててみましょう。たとえば、次の作業を依頼できます。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データ アクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。これらのレッスンで、次の作業を行いました。

- **このリポジトリに含まれるカスタム指示ファイルを確認し、cloud agent の出力への影響を確認しました。**
- **Copilot cloud agent に Issue を割り当て、環境のセットアップ、計画、非同期の作業を確認しました。**
- **アクセシビリティ用のカスタム エージェントを作成して使い、ハイ コントラストとライト モードの切り替えを追加しました。**
- **Copilot agents ページを mission control として使い、進行中のアクセシビリティ セッションを監視して調整しました。**
- **cloud agent のプル リクエストをレビューして反復し、`@copilot` に変更を依頼し、ワークフローを承認しました。**

## レビューと次のステップ

Cloud agent ハーネスを完了しました。さらに学びたい場合は、ほかのハーネスで今回の内容を広げられます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から Copilot Chat の agent mode と MCP 統合を体験します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、エージェント スキル、QA カスタム エージェント、Playwright MCP、Agent Merge、cloud agent に戻る任意の `/delegate` を使い、ターミナルからレビュー済みの変更を 2 つリリースします。

自分のリポジトリでも、次のような作業を試してください。

- リファクタリングやテスト カバレッジの Issue を cloud agent に割り当てる。
- 別の分野に特化したカスタム エージェントを作成する。
- 異なる技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
