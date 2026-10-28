---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Copilot cloud agent の演習を始める前に、必要な準備をします。Tailspin Toys リポジトリのコピーを作成し、指示ファイルの編集や cloud agent が作成した内容のレビューに使う [Codespace][codespaces] を起動します。

## ラボ用リポジトリをセットアップする

作成するコードのためのリポジトリを用意するには、[テンプレート][template-repository]から自分用のインスタンスを作成します。新しいインスタンスにはラボに必要なファイルがすべて含まれます。演習ではこのリポジトリを使用します。

1. 新しいブラウザー ウィンドウで [Tailspin Toys テンプレート][tailspin-template]を開きます。
2. ラボのリポジトリ ページで **Use this template** ボタンを選択し、自分用のコピーを作成します。次に **Create a new repository** を選択します。

    ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターの案内に従ってください。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成できます。

    ![リポジトリ テンプレートの設定を入力する](../../../_images/ex0-repository-settings.png)

4. 作成したリポジトリのパス (**organization-or-user-name/repository-name**) をメモしてください。ラボの後半で使用します。

## Codespace を作成する

次に、ラボの演習に使う Codespace を作成します。

[GitHub Codespaces][codespaces] は、ブラウザーから直接コードの作成、実行、デバッグができるクラウド開発環境です。複数のプログラミング言語、拡張機能、ツールをサポートする、機能豊富な IDE を利用できます。

1. 作成したリポジトリを開きます。
2. 緑色の **Code** ボタンを選択します。

    ![Code ボタンを選択する](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選択し、**+** ボタンを選択して Codespace を新しく作成します。

    ![新しい Codespace を作成する](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかりますが、サービスを手作業でインストールするよりもずっと速く完了します。その間に GitHub Copilot の他の機能を試してみましょう。次の演習から使い始めます。

> [!CAUTION]
> 後の演習で Codespace に戻ります。今はブラウザーのタブを開いたままにしてください。

> [!NOTE]
> このワークショップは Codespace またはローカルの [dev container][dev-containers] で実行できるように構成されています。どちらの環境にも、演習をスムーズに進めるために必要な前提条件がすべてインストールされています。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、プロンプトが表示されたら **Reopen in Container** を選択してください。Codespace と同じ dev container が VS Code によって構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## まとめ

ラボ用リポジトリのコピーを作成できました。また、Copilot cloud agent と一緒に作業するための Codespace の作成も開始しました。

## 次のステップ

次は、cloud agent が従うカスタム指示を追加します。[演習 1 - カスタム指示][next-lesson]に進みます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces の使用を開始する][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
