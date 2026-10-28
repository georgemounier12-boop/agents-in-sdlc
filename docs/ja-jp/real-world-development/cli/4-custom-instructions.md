---
title: "レッスン 4 - カスタム指示で Copilot をガイドする"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合は、そのガイダンスを Copilot が利用できるようにします。[指示ファイル][instruction-files]には、求めるコードだけでなく、その構成方法も記載します。フィルター機能を実装したので、Copilot が参照した指示を確認し、ドキュメント標準を追加してコードに適用します。

このレッスンでは、次の内容を学びます。

- リポジトリの指示とパスに応じた指示ファイルがエージェントに適用される方法を確認します。
- コーディング標準を守るため、指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。具体的には、次のとおりです。

- コメントにはコードを言い換えるのではなく、意図や自明でない判断を記載します。
- `db/` と `src/lib/` のエクスポート関数には、目的、パラメーター、戻り値を TSDoc/JSDoc で記載します。該当する場合は、注入可能な `db` 引数についても記載します。
- 再利用可能な Astro コンポーネントでは `Props` の契約を説明し、関連コードが変わったらコメントも最新の状態にします。
- 既存の書式設定と lint のガイダンスを維持します。

指示ファイルを使い、これらの開発手法に沿って作業するために必要な情報を Copilot に伝えます。

## 指示ファイル

カスタム指示を使うと、コーディング スタイルや要件に関するコンテキストと設定を Copilot に伝えられます。指示によって、より適切な提案やコード スニペットを得ることができます。好みのコーディング規約やライブラリ、コードに含めたいコメントの種類なども指定できます。リポジトリ全体、または特定の種類のファイルやタスクに対して指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**リクエストで Copilot に送信される単一の指示ファイルです。多くのリクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイルの種類、タスクに関するガイドラインを記載します。

> [!NOTE]
> ほかの指示形式やサポート状況は環境によって異なります。特定の形式に依存する前に、[カスタム指示のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スターター プロジェクトには、すぐに作業を始められるよう、指示ファイルがすでに用意されています。変更前に現在の内容を確認して、変更の影響を比較しましょう。

1. Codespace に戻ります。
2. Codespaces のエディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディング ガイダンスを確認します。これらの指示は、リポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データ層、テストなどに関する指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。このフィールドは、指示を適用するファイルを決める glob を指定します。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルやプロジェクト内の既存ファイルへのリンクを確認します。大きな指示を小さく再利用可能なファイルに分割し、参考例を指定できます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存のファイルは出発点として適切ですが、まだ足りない部分があります。新しく生成する TypeScript に TSDoc コメントを追加するよう、中核となる `copilot-instructions.md` を変更します。

1. `.github/copilot-instructions.md` で、**Code formatting guidance** というセクションを探します。35 行目付近にあります。
2. 最後の箇条書きとして、次の内容を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。更新した指示を保存したら、フィルター機能の会話を終了して再開します。これにより、機能に関するコンテキストを保ったまま、新しい指示を Copilot に読み込ませることができます。

1. フィルター機能の会話がまだ開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートで `game-filters-cli` ブランチに切り替え、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示されたら、フィルター機能の会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 次のプロンプトを使って、更新したガイダンスを適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力して、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを得る方法を確認し、新しい標準を機能に適用しました。このレッスンでは、次の作業を行いました。

- リポジトリの指示とパスに応じた指示ファイルがエージェントに適用される方法を確認しました。
- コーディング標準を守るため、指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、lint とテストを一貫して実行できるよう、再利用可能な [quality-checks スキルをカスタマイズして実行します][next-lesson]。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
