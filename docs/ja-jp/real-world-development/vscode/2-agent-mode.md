---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの単純な更新でも、通常は複数のファイルを変更し、テストなどを実行する必要があります。開発者は、必要なファイルをすべて探し、変更を加え、テストを実行し、デバッグし、更新し忘れたファイルを見つけて、さらに変更を加えることになります。作業はそれだけではありません。

ここで役立つのが Copilot Agent Mode です。

Copilot Agent Mode は、IDE 内でより自律的に作業するように設計されています。開発者と同様に、既存のプロジェクト構成を調べ、必要な更新を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。Agent Mode を使って、サイトに新しい機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる用途向けに作られています。agent mode は IDE 内で作業し、すばやいフィードバックと対話を可能にします。cloud agent はチームの一員のように非同期で作業し、Issue とプル リクエストを通じてやり取りします。

この演習では、次の内容を学びます。

- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを特定し、複数のファイルにまたがる変更を行う方法
- GitHub Copilot Agent Mode で UI とデータ層にまたがる機能を実装する方法
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする方法

## シナリオ

ゲームの一覧が増えてきたため、カテゴリやパブリッシャーで絞り込めるようにします。前の演習では publishers ヘルパーを追加しました。次は Copilot Agent Mode を使い、残りのデータ層、UI、テストを完成させます。

## Tailspin Toys の Web サイトを起動する

変更を加える前に、Tailspin Toys の Web サイトを確認し、現在の機能を把握しましょう。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディング プラットフォームです。ゲームの一覧や詳細情報を表示します。単一の Astro アプリであり、ビルド時にページを静的 HTML として生成します。ページは frontmatter 内で Drizzle ORM を通じてローカル SQLite データベースを直接クエリします。別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータ アクセス ヘルパーは `src/lib/` にあり、対話機能は標準 DOM API を使う小さなスコープ付き Astro `<script>` で追加します。

### Web サイトを起動する

Web サイトを簡単に起動できるように、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace では次の手順で実行できます。

1. Codespace に戻ります。前の演習の後なので `main` にいるはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. フィルター作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが起動すると、次のような URL を含むバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くか確認するダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を、Mac では <kbd>Command</kbd>、Windows/Linux では <kbd>Ctrl</kbd> を押しながら選択して Web サイトを開きます。

> [!NOTE]
> Codespace でローカル URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的にリダイレクトされます。これは、Web サーバーをホストしている Codespace へのプライベート トンネルです。

### Web サイトを確認する

Web サイトが起動したら、機能を確認します。主な機能は次のとおりです。

- **ホーム ページ:** タイトル、画像、説明を含むボードゲームの一覧を表示します。
- **ゲーム詳細ページ:** ゲームを選択すると、そのゲームのタイトル、説明、パブリッシャー、カテゴリなどの詳細ページが表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

Web サイトの最初の実装は動作していますが、新しい機能を追加して改善します。まずバックログを確認しましょう。テンプレートからプロジェクトをセットアップすると、Issue のバックログが自動的に作成されています。GitHub Copilot に項目を表示してもらいます。

1. Chat ビューのエージェント ドロップダウンから **Agent** を選択します。**Agent** エージェントは、複数ファイルにまたがる変更の計画と実装、ターミナル コマンドの実行、ツールの呼び出しを自律的に行います。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者は、特定のモデルを推奨しているわけではありません。ワークショップの作成時に Claude Sonnet 4.5 を使用したため、手順に含めています。コードの提案にある程度の一貫性を持たせ、よい体験を提供することが目的です。ただし、LLM の動作は確定的ではないため、ワークショップの説明と異なる提案が表示されることがあります。これは正常な動作です。

> [!NOTE]
> LLM の確率的な特性により、Copilot が別の MCP コマンドを使う場合がありますが、タスクは同様に完了できるはずです。

3. 次のプロンプトを Copilot に送信して、バックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot は、ユーザーに最も役立つと思われる項目を優先順位付けしてくれます。

## 指示ファイルを確認する

エージェントにコードを生成させる前に、Copilot に作業のコンテキストを伝える指示ファイルを確認します。Web サイトに機能を追加する方法を説明する [UI 指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加に関する全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要
   - コンポーネントの設計、テスト容易性、アクセシビリティに関する原則
   - さまざまなファイルの種類に対応する指示へのリンク:
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指示ファイルから、他の指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルの場所からの相対パスです。これにより、指示を再利用し、複雑な内容を小さく管理しやすい単位に分け、例やテンプレートを提示できます。

## フィルター機能を実装する

フィルター機能を完成させるには、アプリケーションに少なくとも次の 3 つの変更が必要です。

- データ層 (`src/lib/`) の残りのフィルター ロジックを追加または改善する
- フィルターの動作を確認するテストを追加または更新する
- ゲーム一覧ページにフィルター UI を追加する

コードベースにマージする前に、テストを実行して成功させる必要もあります。Copilot Agent Mode にこれらの作業を任せましょう。

1. 現在の Copilot の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択し、指示ファイルを追加します。

   ![UI 指示ファイルの選択例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント ドロップダウンで、引き続き **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. モデルとして **Claude Sonnet 4.5** が引き続き選択されていることを確認します。
5. 次のプロンプトを使い、バックログの関連 Issue に基づいて機能を実装するよう Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、機能に関連するファイルを探す様子を確認します。データ層のヘルパー、UI、テストを見つけた後、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクト ファイルを調べる様子を示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行います。タスクやコードベースの複雑さによっては、数分かかることがあります。処理中にコード エディターで **Keep** と **Undo** が表示されることがあります。作業が完了すると、すべての変更に **Keep** または **Undo** が表示されます。処理中に選択する必要はありません。

7. Copilot に求められたら **Continue** を選択してテストを実行します。

   ![テストの実行を確認するダイアログを Copilot Chat ペインに表示したスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりすることがあります。問題ありません。Copilot はタスクが完了し、エラーを検出しなくなるまで、コード生成とテストを繰り返します。

   ![Copilot Agent Mode の完了した Chat セッションを示すスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot や他の AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新しい機能を試します。
11. すべてが動作し、コードのレビューも完了したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

おめでとうございます。この演習では、GitHub Copilot Agent Mode を使って Tailspin Toys の Web サイトに新機能を追加しました。次の内容を学びました。

- GitHub Copilot Agent Mode で UI とデータ層にまたがる機能を実装する
- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを特定して変更を調整する方法
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする方法

次は[Playwright MCP サーバーで機能をテストし][next-lesson]、プル リクエストを作成します。

### ボーナス演習 — ページネーションを実装する

ゲーム一覧が増えると、ページネーションが必要になります。この演習で学んだことを使い、Copilot にページネーションを実装するよう依頼します。次の点を検討してください。

- 既存の指示ファイルなど、現在のベスト プラクティスに従います。
- ページ サイズをユーザーが選べるようにするか、固定値にするかを検討します。
- 希望する実装に必要なガイダンスを、プロンプトで Copilot に伝えます。
- GitHub Copilot に変更やコンテキストを追加で伝えながら、繰り返し改善することがあります。Copilot を使う際の通常の流れです。

## リソース

- [Copilot の ask、edit、agent mode: それぞれの用途と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
