---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、その情報にアクセスできるようにします。[指示ファイル][instruction-files]を使うと、Copilot に「何を」するかだけでなく、「どのように」実行するかも伝えられます。

この演習では、次の内容を学びます。

- リポジトリのカスタム指示とパスに応じた指示ファイルを使って、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準を Copilot に伝えます。
- リポジトリ全体に適用する標準を `.github/copilot-instructions.md` に追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、ここではプロンプトの「変更前と変更後」を比較しません。Copilot cloud agent は GitHub Issue に対して非同期で動作するため、リアルタイムで並べて効果を示すのが難しいためです。このハーネスの後半で、cloud agent が作成したプル リクエストをレビューするときに、指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。具体的には、次のとおりです。

- データ層には必ず単体テストを用意します。
- UI はダーク モードにし、現代的な見た目にします。
- TSDoc のドキュメント コメントをコードに追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使うと、Copilot が適切な情報を得て、これらの開発手法に沿ってタスクを実行できるようになります。

### カスタム指示

カスタム指示を使うと、コーディング スタイルや要件に関するコンテキストと設定を Copilot に伝え、より適切な提案やコード スニペットを得られます。リポジトリ全体、または特定の種類のファイルやタスクに対して指示を作成できます。好みのコーディング規約やライブラリ、コードに含めたいコメントの種類なども指定できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**リクエストで Copilot に送信される単一の指示ファイルです。技術スタック、開発対象の概要、ベスト プラクティス、全般的なガイダンスなど、チャットや CLI の多くのリクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類に指示を適用するためのファイルです。TypeScript などの言語、UI コンポーネントの作成、単体テストの追加といったタスクに関するガイドラインを記載できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使用されます。コード補完や次の編集候補には使用されません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの指示ファイルと、`applyTo` フロントマターで適用先を指定した `*.instructions.md` ファイルの両方を使用します。
>
> Copilot は、AGENTS.md や CLAUDE.md など、他の標準に基づく指示ファイルにも対応しています。[指示ファイルのサポート][custom-instructions-support]を参照してください。

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

このリポジトリに含まれる指示ファイルを読みましょう。中核となる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** のガイダンスも確認してください。これらの指示は、Copilot を使うあらゆる作業に適用されます。
3. `.github/instructions` フォルダーを開き、内容を確認します。Astro ファイル、Drizzle データ層、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。このフィールドは、指示を適用するファイルを決める、リポジトリ ルートからの glob を指定します。たとえば、`**/*.test.ts` に一致する TypeScript のテスト ファイルに適用されます。
5. このプロジェクトで単体テストを作成する際の指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` の末尾までスクロールします。`unit-tests.instructions.md` などの他の指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。これにより、大きな指示を小さく再利用可能なファイルに分割し、コード生成時に参考例を指定できます。リンクのパスはリポジトリ ルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションではプロジェクトのコーディング標準を説明していますが、コード内のドキュメント コメントはまだ必須ではありません。次の手順で、TSDoc のドキュメント コメントとファイル コメント ヘッダーのルールを追加します。

## リポジトリ全体に適用する標準を追加する

ここで行う編集は、コード内に TSDoc のドキュメント コメントとファイル コメント ヘッダーを追加するという、プロジェクト全体のルールを 1 つ追加するだけです。後の演習で Issue を割り当てると、cloud agent はこのルールを参照します。

編集する前に、作業ブランチを作成します。cloud agent は Issue の対象ブランチから指示を読み取りますが、変更を整理してコミットするためにもブランチを使います。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前に説明したように、`.github/copilot-instructions.md` はプロジェクト レベルの情報を Copilot に伝えるためのファイルです。コードの提案を改善するために、コーディング標準を記載しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。コーディング標準は記載されていますが、コード内のドキュメントに関するルールがありません。そのため、生成されたヘルパーにドキュメント コメントがありませんでした。
3. 既存の標準の直後に、次の Markdown を追加します。TSDoc のドキュメント コメントとファイル コメント ヘッダーを Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで見たように、リポジトリ レベルの `.github/copilot-instructions.md` はプロジェクト全体に適用され、`*.instructions.md` は言語、ファイルの種類、タスクに応じた指示を記載します。ドキュメント コメントのルールのように、プロジェクト全体に適用する標準はリポジトリ レベルのファイルに記載します。

## 指示の変更をコミット、プッシュ、マージする

cloud agent は、Issue の対象ブランチにある指示ファイルを読み込みます。次の演習で Copilot に Issue を割り当てると、Copilot は `main` からブランチを作成します。そのため、指示を参照できるように、変更を `main` に反映しておく必要があります。

1. ステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプル リクエストを作成し、マージします。次の演習で Issue に割り当てたとき、cloud agent はこの指示を参照します。

> [!TIP]
> ワークショップでは `main` に直接作業してもかまいません。その場合はブランチの作成を省略して、`main` に直接コミットできます。ここでブランチを作成するのは、実際のプロジェクトでの作業方法を体験するためです。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを得る方法を確認し、リポジトリ全体に適用する標準を `.github/copilot-instructions.md` に追加しました。残りのハーネスで cloud agent が作成するプル リクエストをレビューすると、この標準が反映されていることを確認できます。

次は [Copilot cloud agent に最初の Issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot をカスタマイズする指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
