---
title: "演習 3 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に特化した AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを置いてエージェントを定義し、重点的な指示、ベスト プラクティス、コーディング パターン、分野固有の知識を Copilot に提供できます。チームの専門知識を再利用できるエージェントとしてまとめることもできます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティ エージェント、安全なコーディング手法に従うセキュリティ エージェント、一貫したテスト パターンを維持するテスト エージェントなどを作成できます。

カスタム エージェントは、プロジェクト内の `.github/agents` フォルダーにある Markdown ファイル、またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、最低限 `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を定義する Markdown プロンプトを記述します。

### カスタム エージェントとエージェント スキルの違い

カスタム エージェントと[エージェント スキル][agent-skills-concept]には、役割が一部重複する部分があります。どちらも主に Markdown ファイルで定義し、AI に作業の方法を指示します。最もわかりやすい違いは、**カスタム エージェントが作業者であり、スキルがツールである**ことです。

カスタム エージェントには独自のコンテキスト ウィンドウがあり、作業の一部としてスキルや他のエージェントを調整するように設計されています。このラボのアクセシビリティ カスタム エージェントは、アクセシビリティ ガイドラインに沿ってサイトをレビューし、更新します。その際、プル リクエストのワークフローを実行するスキルや、テストを実行・管理するスキルを呼び出すことができます。

> [!NOTE]
> カスタム エージェントの作成に唯一の正解はありません。AI の他の機能と同様に、実際に試し、環境やシナリオに合わせて改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

カスタム エージェントを使い、次の内容を学びます。

- カスタム エージェントの定義方法
- タスクをカスタム エージェントに割り当てる方法

## シナリオ

Tailspin Toys は、視覚能力や好みに関係なく、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにしたいと考えています。最近のユーザー フィードバックでは、テキストと背景色のコントラストが不十分なため、現在のダーク テーマが読みにくいという意見がありました。このアクセシビリティ上の課題に対応するため、デザイン チームは、ユーザーが切り替えられるハイ コントラスト モードの実装を依頼しました。

アクセシビリティは重要なので、この機能をできるだけ早く実装します。カスタム エージェントを使って機能を作成します。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに作成されています。Copilot をどのようにガイドするかを理解するため、内容を確認しましょう。

Codespace に戻り、アクセシビリティ用のカスタム エージェント ファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` フィールドと `description` フィールドを含む YAML フロントマターを確認します。

> [!CAUTION]
> `name` と `description` を含むフロントマターは、カスタム エージェントに必須です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコードを生成する際の主な役割
   - アクセシビリティのベスト プラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある問題や間違い

## Issue を作成して割り当てる

Mission control は、環境内のすべてのエージェントを操作するための中心的な場所です。Copilot cloud agent にタスクを割り当て、進捗を監視し、追加のガイダンスを提供して作業の方向を調整できます。まず、ハイ コントラスト モードの作成を Copilot に割り当てましょう。

1. リポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して、新しい Issue を作成します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。
9. カスタム エージェントの一覧から **Accessibility agent** を選択します。

    ![カスタム エージェントとアクセシビリティ エージェントが強調表示された cloud agent の割り当て画面](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドでタスクを開始します。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に特化した AI アシスタントである、GitHub Copilot の[カスタム エージェント][custom-agents]を確認しました。カスタム エージェントを使うと、チームの専門知識や標準を再利用できる形にまとめ、Copilot が特定の種類の作業をより効果的に実行できるようにガイドできます。

このレッスンでは、次の内容を学びました。

- カスタム エージェントの定義方法
- タスクをカスタム エージェントに割り当てる方法

Copilot がハイ コントラスト モードを実装している間に、[mission control からエージェント セッションを監視して調整します][next-lesson]。

## リソース

- [カスタム エージェントについて][custom-agents]
- [組織でカスタム エージェントを使用する準備][org-custom-agents]
- [エンタープライズでカスタム エージェントを使用する準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
