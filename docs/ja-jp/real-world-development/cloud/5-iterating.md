---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、GitHub Copilot とともにユーザーエクスペリエンスや機能を改善する複数のタスクに取り組みました。コードにドキュメントを追加し、デザインチームが試せる関連ゲーム機能を作成し、高コントラストとライトモードの切り替えを含むアクセシビリティ機能を実装するよう依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送ります。

### シナリオ

生成 AI を導入しても、ソフトウェア設計や開発・運用 (DevOps) の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。ここでは、チームでのレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot が提案した変更を確認します。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は非同期かつ自動的に作業するため、安全性を保つためのセキュリティ制約が設定されています。

- Copilot はリポジトリへの読み取りアクセス権を持ち、書き込みはコード用ブランチにのみ許可されます。
- cloud agent は GitHub Actions 内に作られる一時的な別環境で作業します。
- GitHub Actions のワークフローを実行するには、人間による承認が必要です。
- Model Context Protocol (MCP) サーバーを含む外部リソースへの[アクセスは既定で制限][agent-firewall]されています。

## 生成されたドキュメントをレビューする

最初に、GitHub Copilot cloud agent がコードにドキュメントを追加したプルリクエスト (PR) を確認します。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認するときに、ファイアウォールによって GitHub Copilot がブロックされたという警告が表示される場合があります。Copilot は MCP サーバーへの呼び出しを含む外部リソースへのアクセスが既定で制限されているため、これは想定された動作です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** など、同様のタイトルが付いた PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR に **[WIP]** が表示されます。作業が完了するまで待ちます。数分かかる場合があるため、休憩したり、これまでに学んだことを振り返ったりしてください。

4. PR の準備ができたら、**Files changed** タブを選択して変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく追加された TSDoc のドキュメントコメントや、そのほかのドキュメントを含むコードを確認します。具体的な変更は毎回異なります。

   TSDoc のドキュメントコメント、TypeScript の規約、コメントヘッダーに注目してください。これらは演習 2 の最初に確認したカスタム指示ファイルによるものです。

6. 更新内容をレビューして問題がなければ、**Conversation** タブに戻って下にスクロールします。
7. 承認待ちのワークフローがあることを示す表示を探します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. ワークフローが PR のチェックに追加されたことを確認します。問題がなければ、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

Copilot との PR 作業は一方通行ではありません。チームメンバーと同じように、PR やコードのインラインコメントで Copilot にメンションできます。Copilot はコメントを確認し、対応する新しいセッションを開始します。結果は確率的に変わるため、指定のプロンプトを1つに決めることはできません。次のような変更を依頼できます。

- 各コードファイルの先頭に、ファイルの概要を説明するコメントヘッダーを追加する。
- TypeScript と Astro ファイルに TSDoc のドキュメントコメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. 上記の案や、コードベースのドキュメントに関する別の提案を使って、**@copilot** をメンションしたコメントで変更を依頼します。
2. **View Session** を選択して Copilot の作業を確認します。更新のために新しいセッションが始まります。
3. **Back to pull request** を選択すると PR に戻れます。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、PR に新しいコミットが表示されます。
5. **Files changed** タブを選択して変更をレビューします。

必要な結果が得られるまで反復できます。問題がなければ、PR をドラフトからレビュー可能な状態に変更して `main` ブランチにマージします。

![PR をレビュー可能な状態に変更する画面](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページへの関連ゲーム表示を依頼した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** など、同様のタイトルが付いた PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 更新を確認して問題がなければ、**Conversation** タブに戻って下にスクロールします。
6. 承認待ちのワークフローがあることを示す表示を探します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. ワークフローが PR のチェックに追加されたことを確認します。問題がなければ、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
9. **省略可能:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動テストできます。Codespace に移動してターミナルを開き、Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot は関連ゲーム機能を作成しました。前と同じように、Copilot cloud agent に更新を依頼して反復できます。たとえば表示する関連ゲームの数を変えたり、カスタム指示の更新前に割り当てたことに注意しながら、コメントヘッダーと TSDoc のドキュメントコメントを追加したりできます。前と同じように **Conversation** タブにコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用のカスタムエージェントで実装した機能を確認します。この PR には、演習 3 で依頼した高コントラストモードと、演習 4 の mission control で追加を依頼したライトモードの両方が含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** など、同様のタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR に **[WIP]** が表示されます。完了するまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 次の点に注意して実装を確認します。
   - モードを切り替える UI コンポーネント。
   - ユーザー設定を保存する local storage の使い方。
   - 高コントラストモードとライトモードの CSS またはスタイルの変更。
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)。
   - モード切り替えを管理する JavaScript/TypeScript コード。

6. 更新を確認して問題がなければ、**Conversation** タブに戻って下にスクロールします。
7. 承認待ちのワークフローがあることを示す表示を探します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. ワークフローが PR のチェックに追加されたことを確認します。問題がなければ、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
10. **省略可能:** Codespace でこのブランチに切り替え、高コントラストモードとライトモードを手動テストできます。Codespace に移動してターミナルを開き、Copilot が作成したブランチ名に置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    次にアプリケーションを起動し、ブラウザーで高コントラストモードとライトモードを切り替え、ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティ用のカスタムエージェントは、アクセシビリティのベストプラクティスに沿って機能を実装するよう Copilot をガイドしました。懸念点や改善案があれば、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## オプションの演習 — 委任を続ける

cloud agent に実際のバックログ項目を任せると、ほかの作業に集中できます。リポジトリでいくつか Issue を作成し、Copilot に割り当ててみてください。

- ゲーム詳細ページに支援者向け問い合わせフォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。各レッスンを通じて、次のことを行いました。

- このリポジトリにあるカスタム指示ファイルを確認し、cloud agent の出力に与える影響を調べました。
- Issue を Copilot cloud agent に割り当て、環境のセットアップ、計画、非同期の実行を確認しました。
- アクセシビリティ用のカスタムエージェントを作成して使い、高コントラストモードとライトモードを追加しました。
- Copilot agents ページを mission control として使い、アクセシビリティセッションを監視して作業中に方向付けました。
- **@copilot** をメンションして変更を依頼し、ワークフローを承認して、cloud agent のプルリクエストをレビューし改善しました。

## レビューと次のステップ

Cloud agent ハーネスを完了しました。引き続き Copilot を試す場合は、ほかのハーネスで異なる環境のシナリオを確認できます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE 内で Copilot Chat のエージェントモードと MCP 統合を確認します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、agent skill、QA カスタムエージェント、Playwright MCP、Agent Merge、cloud agent に戻る任意の `/delegate` を使い、ターミナルから2つの変更をレビューしてリリースします。

自分のリポジトリでは、次のアイデアを試せます。

- リファクタリングやテストカバレッジの Issue を cloud agent に割り当てる。
- 別の分野のカスタムエージェントを作る。
- 異なる技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

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
