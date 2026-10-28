---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合は、そのガイダンスを Copilot が利用できるようにします。Copilot と特定のコンテキストを共有する方法はいくつかあります。その中でも重要なのが[指示ファイル][instruction-files]です。指示ファイルでは、コード生成の「方法」に関するガイダンスを伝えます。

この演習では、次の内容を学びます。

- リポジトリのカスタム指示とパスに応じた指示ファイルを使って、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準を Copilot に伝えます。
- 現在の指示に従って、フィルター用の最初のデータ部分 (publishers ヘルパー) を生成します。
- リポジトリ全体に適用する標準を `.github/copilot-instructions.md` に追加します。
- 同じプロンプトをもう一度実行し、生成コードが新しい標準に従うことを確認します。
- 次の演習で更新後のガイダンスを使えるよう、指示の更新とフィルター機能の一部を `main` にコミットします。

> [!CAUTION]
> 生成されたコードが、設定した標準の一部に従わないことがあります。Copilot の動作は確定的ではありません。この演習では、指示を更新した後の動作がどのように**改善するか**を確認します。生成結果を文字単位で一致させることが目的ではありません。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。具体的には、次のとおりです。

- データ層には必ず単体テストを用意します。
- UI はダーク モードにし、現代的な見た目にします。
- TSDoc のドキュメント コメントをコードに追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使い、これらの開発手法に沿って作業するために必要な情報を Copilot に伝えます。

### カスタム指示

カスタム指示を使うと、コーディング スタイルや要件に関するコンテキストと設定を Copilot に伝え、より適切な提案やコード スニペットを得られます。好みのコーディング規約やライブラリ、コードに含めたいコメントの種類なども指定できます。リポジトリ全体、または特定の種類のファイルやタスクに対して指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**リクエストで Copilot に送信される単一の指示ファイルです。技術スタック、開発対象の概要、ベスト プラクティス、全般的なガイダンスなど、チャットや CLI の多くのリクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類に指示を適用するためのファイルです。TypeScript などの言語、UI コンポーネントの作成、単体テストの追加といったタスクに関するガイドラインを記載できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使用されます。コード補完や次の編集候補には使用されません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの指示ファイルと、`applyTo` フロントマターで適用先を指定した `*.instructions.md` ファイルの両方を使用します。
>
> Copilot は、AGENTS.md や CLAUDE.md など、他の標準に基づく指示ファイルにも対応しています。[カスタム指示のサポート][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベスト プラクティス

指示ファイルの作成について詳しく説明することは、このワークショップの範囲外です。ただし、サンプル プロジェクトには代表的な構成例があります。基本的には、次のようにします。

- `copilot-instructions.md` には、開発対象の説明、プロジェクトの構造、全体に適用するコーディング標準など、プロジェクト レベルのガイダンスを記載します。
- `*.instructions.md` には、単体テスト、Astro コンポーネント、データ層などのファイル種類や、特定のタスクに対する指示を記載します。
- 自然な言葉で、わかりやすい例を使ってガイダンスを記載します。

指示ファイルの使い方に唯一の正解はありません。生成 AI の他の機能と同様、実際に試しながらプロジェクトに合った使い方を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な指示ファイルを用意することをお勧めします。このプロジェクトには、[UI の更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスクに対応するファイルがあります。
>
> Copilot に指示ファイルの作成を依頼することもできます。各環境で方法は異なります。たとえば、VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init` があります。使用中の環境に関係するレッスンで、該当する方法を説明します。
>
> テンプレートや出発点を探していますか？指示ファイル、カスタム エージェント、その他のリソースをまとめたリポジトリ [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを読みましょう。中核となる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** のガイダンスも確認してください。これらの指示は、Copilot を使うあらゆる作業に適用されます。
3. `.github/instructions` フォルダーを開き、内容を確認します。Astro ファイル、Drizzle データ層、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。このフィールドは、指示を適用するファイルを決める、リポジトリ ルートからの glob を指定します。たとえば、`**/*.test.ts` に一致する TypeScript のテスト ファイルに適用されます。
5. このプロジェクトで単体テストを作成する際の指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` の末尾までスクロールします。`unit-tests.instructions.md` などの他の指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。大きな指示を小さく再利用可能なファイルに分割し、コード生成時の参考例を指定できます。リンクのパスはリポジトリ ルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションではプロジェクトのコーディング標準を説明していますが、コード内のドキュメント コメントはまだ必須ではありません。次の手順で、TSDoc のドキュメント コメントとファイル コメント ヘッダーのルールを追加します。

## 変更用ブランチを作成する

ベスト プラクティスに沿って、変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する*前*に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

カスタム指示の影響を確認するため、現在の指示ファイルを使って最初にプロンプトを送信します。その後、指示を更新して同じプロンプトをもう一度送信し、違いを確認します。

1. 前の演習で開いたエディター タブを閉じ、Copilot が必要なコンテキストだけを使うようにします。
2. Copilot にヘルパーを配置する場所を示すため、`src/lib/publishers.ts` を開きます。
3. Chat ビューのエージェント ドロップダウンから **Agent** を選択し、ファイルを変更できるようにします。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べて、ヘルパー ファイルとテストなどにコードの変更を適用します。
6. 提案されたヘルパーが型付き関数であり、最初の引数に `db` クライアントを受け取り、型付きのパブリッシャー配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ層の規約に基づいています。
7. 提案されたコードに TSDoc のドキュメント コメントとファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、明示されていなくてもドキュメント コメントを追加する場合があります。その場合も問題ありません。この演習の目的は、指示を更新した後に動作の一貫性が改善されることを確認することです。

## リポジトリ全体に適用する標準を追加する

前に説明したように、`.github/copilot-instructions.md` はプロジェクト レベルの情報を Copilot に伝えるためのファイルです。コードの提案を改善するために、コーディング標準を記載しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。コーディング標準は記載されていますが、コード内のドキュメントに関するルールがありません。そのため、生成されたヘルパーにはドキュメント コメントがありませんでした。
3. 既存の標準の直後に、次の Markdown を追加します。TSDoc コメントとファイル コメント ヘッダーを Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで見たように、リポジトリ レベルの `.github/copilot-instructions.md` はプロジェクト全体に適用され、`*.instructions.md` は言語、ファイルの種類、タスクに応じた指示を記載します。今回追加したドキュメント コメントのルールのように、プロジェクト全体に適用する標準はリポジトリ レベルのファイルに記載します。

## プロンプトをもう一度実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話をクリアします。
2. `src/lib/publishers.ts` をもう一度選択し、Copilot が適切なファイルに注目するようにします。
3. **同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックが追加されていることを確認します。

   ```typescript
   /**
    * Tailspin Toys クラウドファンディング プラットフォーム向けのパブリッシャー データ アクセス ヘルパーです。
    * データベースからパブリッシャー情報を取得する関数を提供します。
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが追加されていることを確認します。

   ```typescript
   /**
    * ID と名前を含むすべてのパブリッシャーの一覧を返します。
    *
    * @param db - Drizzle データベース クライアントです。
    * @returns パブリッシャー オブジェクトの配列を解決する Promise です。
    */
   ```

Copilot に新しいプロジェクト標準を適用させ、次の演習で利用する実際のコードに反映できました。

## 指示をコミットしてブランチをプッシュする

指示ファイルもリポジトリの他の資産と同様に、ソース管理で管理します。変更をコミットしてブランチをリポジトリにプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで次のコマンドを実行し、指示とヘルパーの変更があることを確認します。

   ```bash
   git status
   ```

3. 指示の更新と、ヘルパーの基盤として Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加変更が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プル リクエストを作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント標準の Issue と関連付けてプル リクエストを作成します。手動で行うこともできますが、接続済みの GitHub ツールを使って Copilot に依頼できます。Issue を検索し、Issue を完了する PR を作成してから、マージを依頼します。

> [!NOTE]
> この演習では、Copilot が GitHub 上で操作を行います。プロジェクト テンプレートでは、GitHub ツールがワークスペースにあらかじめ接続されています。最初の使用時に、VS Code で GitHub へのサインインを求められる場合があります。プロンプトに従って接続してください。後の演習では、この接続が Model Context Protocol でどのように機能するかを学びます。

1. Codespace 内で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェントの一覧で **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関する Issue を探し、現在のブランチから PR を作成するよう Copilot に依頼します。指示の更新と、今後のフィルター作業に向けた publishers ヘルパーの基盤を含めます。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を見つけて PR を作成します。
5. Copilot が GitHub 操作の **Allow** を求めたら、コマンドを確認して必要に応じて **Allow** を選択します。
6. PR が作成されたら、次のプロンプトで PR をマージし、ローカル ブランチを `main` に戻すよう Copilot に依頼します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot が GitHub 操作やシェル コマンドの **Allow** を求めたら、コマンドを確認して必要に応じて **Allow** を選択します。

GitHub Copilot の支援でプル リクエストを作成し、マージできました。

## まとめと次のステップ

Copilot がこのプロジェクトの指示ファイルからコンテキストを得る方法を確認し、VS Code の Copilot Chat で次の作業を行いました。

- 既存の指示を使ってコード生成プロンプトを送り、Copilot の出力を確認しました。
- リポジトリ全体に適用する標準を `.github/copilot-instructions.md` に追加しました。
- 同じプロンプトをもう一度送り、提案されたコードが新しい標準に従うことを確認しました。
- 次の演習で利用できるよう、指示の更新とヘルパーの基盤を `main` にコミットしました。

次は Copilot がコードベース全体に機能を追加するときに、[agent mode で指示を活用します][next-lesson]。

## リソース

- [GitHub Copilot をカスタマイズする指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
