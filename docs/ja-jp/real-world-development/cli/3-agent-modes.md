---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan で進め方に合意し、Autopilot で Issue に基づくフィルター機能を構築して、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初にプロジェクトへ小さな機能を追加しました。大きな変更には、より確実なプロセスが必要です。GitHub Copilot CLI は組織の既存の開発フローに沿って作業できます。このレッスンから、Issue を起点に新機能を作成し、コードの妥当性と機能の動作を確認してからプロジェクトにマージする、エージェントを使った開発プロセスを始めます。

> [!NOTE]
> 機能開発を続ける間は、同じ会話とブランチを使います。通常はファイルの種類ごとにブランチや PR を分けますが、ここでは中心となる概念に集中するために簡略化します。

このレッスンでは、次のことを行います。

- GitHub Issue から Copilot との会話を始めます。
- Plan モードで要件を決めます。
- Autopilot モードで機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動検証します。

この機能の作業を続けながら、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加して QA エージェントを作成し、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、利用者はカテゴリやパブリッシャーでゲームを絞り込む必要があります。バックログの Issue には機能が記載されていますが、カテゴリを組み合わせる方法など、実装の詳細はコーディング前に合意する必要があります。Plan モードで要件を決めてから、Autopilot に範囲を限定した実装を許可します。

## 背景

AI コーディングエージェントを開発フローに導入しても、基本的な手順は変わりません。一般的な開発フローは次のとおりです。

1. 実装内容を記載した Issue から始めます。
2. 作成する内容を計画します。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーション (CI) が成功したら、コードをマージします。

> [!NOTE]
> 具体的な手順はチームや組織によって異なりますが、多くの場合は上記の流れを基本にしています。

標準の手順に従うことで、AI が生成したコードも手作業で書いたコードと同じように要件を満たし、レビューを受けられます。

## 会話モード

**会話モード**は、エージェントにどの程度の自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すとモードを切り替えられます。

- **Interactive:** 開発者とエージェントが協力して作業します。エージェントは変更を提案し、次の操作に進む前に入力を待ちます。
- **Plan:** エージェントは最初に計画を作成し、プロジェクトファイルを編集できません。
- **Autopilot:** エージェントはコードを書き、テストを実行し、タスクが完了するまで自律的に反復します。

まず Plan モードで計画を作成してレビューし、その後 Autopilot で実装します。

## Issue から始める

フィルター機能に着手する前に、Codespace とターミナルを準備します。

1. Codespace に戻ります。停止している場合は再起動します。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新してから、フィルター機能用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を2回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトが `#7` (または同様の番号) で始まることを確認します。`#` を使うと、GitHub 上の Issue やプルリクエスト (PR) をコンテキストに追加できます。

## フィルター機能を計画する

計画を立てることで、Copilot に機能の実装やタスクを任せる前に、進め方を決められます。複雑な作業では、まず計画を立てることをお勧めします。Plan モードに切り替えて計画を依頼しましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替え、プロンプトの下に **Plan** と表示されていることを確認します。
2. 直前の手順で追加した Issue の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調査し、適切な進め方を判断して計画を作成します。

3. フィルターの動作について質問された場合は、希望に基づいて回答します。正解は1つではありません。
4. 計画ができたら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画を読み返して確認します。
6. 決めた内容と異なる部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成してレビューしたら、実装に進みます。Autopilot を使うと、Copilot が作業を反復します。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みのバージョンで同様のラベルの選択肢を選択します。
2. プロンプトの下に **Autopilot** と表示されていることを確認します。
3. Copilot が計画に沿ってコードを生成し、テストを実行する様子を確認します。

> [!NOTE]
> 承認すると実装がすぐに始まる場合があるため、先に計画を確認してください。依存関係が不足している、またはポートが競合していると報告された場合は、チェック完了と見なす前に問題を解決します。

## 実装をレビューして検証する

生成されたコードは、ほかのコードと同じようにマージ前にレビューします。コードを確認し、サイトを起動して動作も確かめましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替え、モード表示が **Plan** や **Autopilot** ではないことを確認します。
2. `/diff` を入力し、フィルター機能の実装とテストを確認します。
3. 結果を Issue と計画時に決めた内容と照らし合わせます。
4. コードレビューが終わったら <kbd>Esc</kbd> を押して差分表示を閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に修正を依頼します。

## 新機能を試す

コードを確認できたら、アプリを起動し、Codespaces が転送するポートからサイトを開いて動作を確かめます。

1. Copilot に次のプロンプトを送り、アプリを起動します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. 計画で決めたとおりにカテゴリとパブリッシャーのフィルターを個別に適用し、組み合わせも試します。
4. リセットと該当項目がない場合の動作が、Issue と決定内容に合っていることを確認します。
5. Codespace に戻り、Copilot に起動した開発サーバーを停止するよう依頼します。

## まとめと次のステップ

異なる会話モードを使って、機能を構築してレビューしました。このレッスンでは次のことを行いました。

- GitHub Issue から Copilot との会話を始めました。
- Plan モードで要件を決めました。
- Autopilot モードで新機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動検証しました。

次は[カスタム指示を使って][next-lesson]、コードが文書化されたプラクティスに従うようにします。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンドリファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
