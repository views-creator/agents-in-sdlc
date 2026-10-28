---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を利用するときはコンテキストが重要です。タスクを特定の方法で実行してほしい場合や、Copilot に事前知識を与えたい場合は、そのコンテキストを参照できるようにします。Copilot にコンテキストを伝える方法はいくつかあります。その中心となる[指示ファイル][instruction-files]を使うと、コード生成の指針を伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス単位の指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に届く仕組みを確認します。
- 既存の指示を使って、フィルター機能の最初のデータ部分となる publishers ヘルパーを生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再送信し、生成コードが新しい標準に従う様子を確認します。
- 指示の変更とフィルター機能の最初の部分を `main` にコミットし、後の演習で Copilot が更新したガイダンスを使えるようにします。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot は確率的に動作します。この演習では、指示の更新後に動作がどのように変化するかを見ることが目的であり、出力を一字一句一致させることではありません。

## 指示ファイル

### シナリオ

Tailspin Toys には開発プラクティスのガイドラインがあります。

- データレイヤーには必ず単体テストを用意します。
- ユーザーインターフェイス (UI) はダークモードにして、モダンな見た目にします。
- コードのドキュメントには TSDoc のドキュメントコメントを追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使い、Copilot がこれらのプラクティスに沿ってタスクを実行できるようにします。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件などのコンテキストを Copilot に与えられます。規約、ライブラリ、コードに含めるコメントの種類などを指定でき、より関連性の高い提案を得るのに役立ちます。リポジトリ全体にも、特定の種類のファイルにも指示を設定できます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリへのすべての要求で Copilot に送られる単一の指示ファイルです。技術スタック、開発内容の概要、ベストプラクティスなど、ほとんどの要求に関係するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、TypeScript や Astro などの言語やファイルの種類、UI コンポーネントや単体テストの作成などのタスクに合わせた指針を記載します。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示と、`applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方をコード生成時に使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの規格の指示ファイルにも対応しています。[詳細][custom-instructions-support]を確認してください。

### 指示ファイル管理のベストプラクティス

指示ファイルの詳しい作成方法はこのワークショップの範囲外ですが、サンプルプロジェクトには代表的な構成を含めています。

- `copilot-instructions.md` には、開発内容、プロジェクト構成、全体的なコーディング標準など、プロジェクトレベルの指針を記載します。
- `*.instructions.md` には、単体テスト、Astro コンポーネント、データレイヤーなどのファイルの種類や、特定タスク向けの指示を記載します。
- 自然な言葉で、明確なガイダンスとコードの良い例・悪い例を示します。

指示ファイルや AI の使い方に唯一の正解はありません。プロジェクトで試しながら最適な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトでは、指示ファイルを整備することをお勧めします。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスクの指示ファイルがあります。
>
> Copilot に指示ファイルの生成を依頼することもできます。VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init` など、各環境での方法は該当するハーネスで説明します。
>
> テンプレートや出発点を探す場合は、指示ファイル、カスタムエージェントなどを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

変更の影響を確認するため、スタータープロジェクトに含まれる指示ファイルを調べます。

1. 前の演習で使った Codespace に戻ります。
2. Codespaces のエディターで `.github/copilot-instructions.md` を開きます。ターミナルではなくエディターを使います。
3. プロジェクトの簡単な説明や **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** の指針も含め、Copilot とのあらゆるやりとりに適用されます。
4. `.github/instructions` フォルダーを開き、Astro、Drizzle データレイヤー、テストなどの指示ファイルを確認します。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。この glob はリポジトリのルートからのパスで、たとえば `**/*.test.ts` に一致する TypeScript のテストファイルに適用されます。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾にある他の指示ファイルやプロジェクトファイルへのリンクを確認します。これにより、大きな指示を再利用できる小さなファイルに分け、Copilot が参考にできるファイルを示せます。パスは指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で TSDoc コメントとファイルヘッダーのルールを追加します。

## 変更用のブランチを作成する

ベストプラクティスに沿って、変更用のブランチを作成します。

1. 前の演習で使った codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. 次のコマンドで新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なるため、該当箇所で説明します。

カスタム指示の影響を見るため、まず現在の指示ファイルを使ってプロンプトを送信します。後で指示を更新して、同じプロンプトを再送信し、結果を比較します。

1. Copilot が必要なコンテキストだけを使えるよう、前の演習で開いたエディタータブを閉じます。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置先を Copilot に示します。
3. Chat ビューのエージェントドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント選択を示す画面](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調べ、ヘルパーファイルとテストを含むコードを変更する様子を確認します。
6. 提案されたヘルパーが、最初の引数に `db` クライアントを受け取り、型付きのパブリッシャー配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約によるものです。
7. 提案コードには TSDoc のドキュメントコメントとファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントを追加する場合があります。その場合も問題ありません。ここでは、指示更新後の一貫性の改善を確認します。

## リポジトリ全体に適用する標準を追加する

先ほど確認したように、`.github/copilot-instructions.md` は Copilot にプロジェクト全体の情報を与えます。コード提案を改善するために、コーディング標準を追加します。

1. `.github/copilot-instructions.md` を開き直します。
2. **Code formatting requirements** セクションを探します。既存の標準には、生成コードのドキュメントに関するルールがありません。
3. 既存の標準の直後に、次の Markdown 行を追加し、ファイルコメントヘッダーと TSDoc コメントを指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ全体に適用する指示には `.github/copilot-instructions.md` を、言語やファイルの種類、タスクに適用する指示には `*.instructions.md` を使えます。今回のドキュメントコメントのルールのようなプロジェクト全体の標準には、リポジトリレベルのファイルが適しています。

## 同じプロンプトを再送信して違いを確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を選択し、Copilot の対象ファイルを指定します。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準を Copilot に伝え、次の演習で使うコードに適用できました。

## 指示とブランチをコミットしてプッシュする

指示ファイルもほかのアセットと同じようにソース管理します。変更をコミットし、ブランチをリポジトリにプッシュします。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
2. 次のコマンドを実行し、指示とヘルパーの変更が存在することを確認します。

   ```bash
   git status
   ```

3. 指示の更新とヘルパーの基礎部分について、Copilot が変更したファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status に追加の関連変更 (テストなど) が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

プッシュしたブランチから PR を作成し、バックログにあるドキュメント標準の Issue と関連付けます。GitHub ツールがプロジェクトに接続されているので、Copilot に Issue を見つけて PR を作成し、マージするよう依頼します。

> [!NOTE]
> この演習では初めて Copilot に GitHub 上の操作を依頼します。プロジェクトテンプレートでは GitHub ツールがワークスペースに設定済みのため、追加のセットアップは不要です。Copilot が最初にツールを使うとき、VS Code から GitHub へのサインインを求められることがあります。画面の手順に従ってください。後の演習では、この接続に使われる Model Context Protocol (MCP) を確認します。

1. codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択で **Agent** が選択されていることを確認します。
3. 次のプロンプトで、指示の更新と今後のフィルター作業に使う publishers ヘルパーの基盤の両方を説明する PR を、現在のブランチから作成するよう Copilot に依頼します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. GitHub の操作を許可するよう Copilot に求められた場合は、コマンドを確認して適切に **Allow** を選択します。
5. PR が作成されたら、次のプロンプトで PR をマージし、ローカルのブランチを `main` に戻すよう依頼します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. GitHub の操作やシェルコマンドの実行を許可するよう Copilot に求められた場合は、コマンドを確認して適切に **Allow** を選択します。

GitHub Copilot の支援でプルリクエストを作成し、マージできました。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する仕組みを確認し、VS Code の Copilot Chat を使って次のことを行いました。

- 既存の指示を使ってコード生成プロンプトを送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。
- 同じプロンプトを再送信し、提案コードが新しい標準に従う様子を確認しました。
- 次の演習で使うため、指示の更新とヘルパーの基盤を `main` にコミットしました。

次は[エージェントモード][next-lesson]で指示を活用し、Copilot にコードベース全体の機能を追加させます。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する5つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
