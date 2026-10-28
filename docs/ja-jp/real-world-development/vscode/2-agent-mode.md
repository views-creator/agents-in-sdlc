---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な更新でも、通常は複数のファイルを変更し、テストを実行する必要があります。開発者は、関連するファイルを探して変更し、テストを実行して、デバッグし、変更漏れがあればさらに修正する、という作業を行います。Copilot Agent Mode は、こうした作業を支援します。

Copilot Agent Mode は IDE 内でより自律的に作業します。プロジェクト構成を調査し、必要なファイルを更新し、テストなどのタスクを実行し、見つかった問題を自動的に修正します。Agent Mode を使ってサイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェントモードと cloud agent は異なる用途向けです。エージェントモードは IDE 内で作業し、すばやいフィードバックとやりとりを可能にします。cloud agent はチームメンバーのように非同期で作業し、Issue やプルリクエストを通じて開発者と連携します。

この演習では、次のことを確認します。

- Copilot Agent Mode がプロジェクトを調査して関連ファイルを特定し、複数ファイルにまたがる変更を行う方法。
- GitHub Copilot Agent Mode がユーザーインターフェイス (UI) とデータレイヤーにまたがる新機能を実装する方法。
- 生成された変更とテストをレビューしてからコードベースにマージする方法。

## シナリオ

ゲームの一覧が増えてきたため、利用者がカテゴリとパブリッシャーで絞り込めるようにします。前の演習では publishers ヘルパーを追加しました。今回は Copilot Agent Mode で残りのデータレイヤー、UI、テストを実装します。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の現在の機能を確認します。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディングプラットフォームです。ゲームの一覧と詳細を表示します。単一の Astro アプリで、ビルド時に静的 HyperText Markup Language (HTML) としてページを生成します。ページは frontmatter 内で Drizzle ORM を使ってローカル SQLite データベースを直接クエリします。別のバックエンド アプリケーション プログラミング インターフェイス (API) やクライアント側 UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` に置き、インタラクティブな機能は標準 Document Object Model (DOM) API を使う小さな Astro `<script>` で追加します。

### Web サイトを起動する

単一の Astro 開発サーバーを起動する `npm run dev` タスクを用意しています。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習後なので、`main` にいるはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. フィルター機能用のブランチを作成して切り替えます。

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
> `http://localhost:4321` を開くか尋ねるダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro サーバーの `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して Web サイトを開きます。

> [!NOTE]
> codespace では、ターミナルの localhost URL を選択すると `https://<your-codespace-name>-4321.app.github.dev/` にリダイレクトされます。これは Web サーバーをホストする codespace へのプライベートトンネルです。

### Web サイトを確認する

Web サイトを起動したら、利用できる機能を確認します。

- **ホームページ:** ボードゲームのタイトル、画像、説明を一覧表示します。
- **ゲーム詳細ページ:** ゲームを選択すると、タイトル、説明、パブリッシャー、カテゴリなどの詳細が表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なるため、該当箇所で説明します。

Web サイトは動作していますが、新しい機能を追加して改善します。まずバックログを確認しましょう。テンプレートからプロジェクトを設定すると、Issue のバックログも自動的に作成されています。GitHub Copilot に項目を表示させます。

1. Chat ビューのエージェントドロップダウンから **Agent** を選択します。**Agent** エージェントは、複数ファイルの変更を自律的に計画・実装し、ターミナルコマンドやツールを実行します。

   ![Chat ビューのエージェント選択を示す画面](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が特定のモデルを推奨しているわけではありません。ワークショップの作成時に Claude Sonnet 4.5 を使用したため、手順に含めています。コード提案の一貫性を高めることが目的ですが、大規模言語モデル (LLM) は確率的に動作するため、結果が異なる場合があります。これは正常な動作です。

> [!NOTE]
> 確率的な動作のため、Copilot が別の MCP コマンドを使う場合がありますが、タスクは同様に完了できます。

3. 次のプロンプトを Copilot に送り、Issue のバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択し、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot が利用者にとって有用だと考える順に項目を整理したことを確認します。

## 指示ファイルを確認する

コードの生成を始める前に、Copilot にコンテキストを与える指示ファイルを確認します。Web サイトに機能を追加する方法を説明する[ユーザーインターフェイス (UI) の指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加に関する全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト容易性、アクセシビリティの原則。
   - Astro、Tailwind CSS、Drizzle などのファイル形式別指示へのリンク。

> [!TIP]
> 指示ファイルから、他の指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルからの相対パスです。指示を再利用しやすい小さな単位に分け、例やテンプレートを参照させることができます。

## フィルター機能を実装する

フィルター機能の実装には、少なくとも次の3種類の変更が必要です。

- データレイヤー (`src/lib/`) に残りのフィルター処理を追加または調整する。
- フィルター動作のテストを追加または更新する。
- ゲーム一覧ページにフィルター UI を追加する。

さらに、コードベースにマージする前にテストを実行して成功させる必要があります。Copilot Agent Mode にこれらの作業を依頼します。

1. 現在の Copilot 会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択します。

   ![UI 指示ファイルを選択する例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択を示す画面](../../../_images/shared-chat-mode-selector.png)

4. モデルが引き続き **Claude Sonnet 4.5** であることを確認します。
5. バックログの関連 Issue に基づいて機能を実装するよう、次のプロンプトを Copilot に送ります。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調査し、機能に関連するファイル (データレイヤー、UI、テストなど) を見つけて変更し、テストを実行する様子を確認します。

   ![Copilot がプロジェクトファイルを調査する画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行います。タスクやコードベースによって数分かかる場合があります。作業中、コードエディターに **Keep** と **Undo** が表示されることがあります。Copilot が完了すると、すべての変更に対して選択肢が表示されるため、作業中に選択する必要はありません。

7. Copilot から求められたら、**Continue** を選択してテストを実行します。

   ![Copilot Chat ペインにテスト実行の確認ダイアログが表示された画面](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりすることがあります。Copilot はコード生成とテストを繰り返し、エラーを検出しなくなるまで修正します。

   ![Copilot Agent Mode の完了済みチャットセッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot やほかの AI ツールが生成したコードも必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新しい機能を確認します。
11. 動作とコードを確認したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot Agent Mode を使って Tailspin Toys の Web サイトに機能を追加しました。この演習では、次のことを確認しました。

- GitHub Copilot Agent Mode が UI とデータレイヤーにまたがる機能を実装する方法。
- Copilot Agent Mode がプロジェクトを調査して関連ファイルを特定し、複数ファイルにまたがる変更を行う方法。
- 生成された変更とテストをレビューしてからコードベースにマージする方法。

次は[Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### ボーナスの探索演習 — ページ分割を実装する

ゲーム一覧が増えるとページ分割が必要になります。この演習で学んだことを使い、Copilot にページ分割を依頼します。次の点を検討してください。

- 既存の指示ファイルを使うなど、現在のベストプラクティスに従います。
- ページサイズを利用者が選べるようにするか、固定値にするかなど、ページ分割の方法を決めます。
- 希望する実装に必要なガイダンスをプロンプトに含めます。
- GitHub Copilot にコンテキストを与えて変更を依頼し、反復が必要な場合があります。これが Copilot を使う通常の流れです。

## リソース

- [Copilot の Ask、Edit、Agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
