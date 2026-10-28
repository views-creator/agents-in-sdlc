---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

直前の演習では、ユーザーエクスペリエンスの改善や機能追加を目的とする3つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

GitHub.com の[エージェントページ][agents-page]など、cloud agent に割り当てたタスクを管理するツールがあります。この mission control では、オープンなプルリクエスト (PR) があるエージェントタスクを確認できます。実行内容を調べたり、進行中のセッションに指示を与えたりできます。

このレッスンでは、次のことを行います。

- エージェントページで cloud agent のタスクを監視します。
- 実行中のセッションに指示を送り、機能を追加で依頼します。

## シナリオ

エージェントに高コントラストモードを依頼した後、チームはライトモードも追加したいと考えました。サイトのスタイルと切り替え機能をすでに変更しているため、同じ作業に含めるのが合理的です。高コントラストに加えてライトモードも実装するよう、エージェントの作業を誘導します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メインペインと左ペインのタスクリストを確認します。割り当てたタスクとして、次のようなものが表示されます。
   - コードベースのドキュメント更新。
   - ゲーム変更用アプリケーション プログラミング インターフェイス (API) の生成。
   - Web サイトへの高コントラストモードの追加。
3. 実行中のタスクを1つ選択し、Copilot が行った作業を確認します。次のような処理が含まれることがあります。
   - リポジトリのチェックアウト。
   - Copilot の作業環境の作成。
   - Model Context Protocol (MCP) サーバーの設定。
   - 割り当てられたタスクを完了するための各種処理。

> [!NOTE]
> 表示される具体的な手順は、作業の状態やエージェントが選んだ方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。PR や変更ファイルを監視できます。

## cloud agent に指示を送る

実行中のタスクを確認したので、高コントラストモードの作業中にライトモードも追加するよう Copilot に依頼します。

1. 高コントラストモードを追加するセッションを選択します。正確なタイトルは Copilot が付けた名前や作業状態によって異なります。

    ![Mission control に表示されたアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まったことを示すメッセージが表示されるまで、セッションを数分間確認します。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![エージェントページのタスクで、作業中のセッションへの指示ダイアログが強調表示された画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot が依頼を認識し、作業計画に含めたことを確認します。

## Copilot に作業を任せる

Copilot は更新されたタスクに取り組みます。メッセージを送信したときに実行中だった手順を終えた後、新しい依頼を作業に取り込みます。

今回も完了まで数分かかります。その間に、これまでに学んだことを振り返ってください。

## まとめと次のステップ

GitHub Copilot cloud agent の監視とガイダンスを行う中心的な場所である Copilot agents ページを確認しました。この mission control では、進行中または完了したタスクの追跡、実行内容のレビュー、作業中のタスクの方向付けができます。

このレッスンでは、次のことを行いました。

- mission control とエージェントページで cloud agent のタスクを監視しました。
- 実行中のセッションに指示を送り、追加機能を依頼しました。

Copilot がアクセシビリティ機能を実装する間に、作成されたプルリクエストを[反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
