---
title: "レッスン 1 - GitHub Copilot app のインストール"
description: "GitHub Copilot app をインストールし、テンプレートから作成したリポジトリを接続して、ワークスペースを確認し、クイックチャットを試します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

[**GitHub Copilot app**][about-copilot-app] は、エージェント主導の開発に使用するデスクトップアプリケーションです。GitHub Copilot CLI を基盤とし、GitHub とネイティブに統合されているため、リポジトリ、ブランチ、CI パイプラインをすぐに利用できます。複数のエージェントをそれぞれ分離されたワークスペースで並列に指示し、繰り返し発生するタスクを自動化するワークフロー向けに設計されています。Node.js のインストールとプロジェクトのコピーが完了したので、次はアプリをインストールしてリポジトリを接続します。

このレッスンでは、次の内容を学びます。

- GitHub Copilot app をインストールしてサインインする。
- GitHub リポジトリからプロジェクトをアプリに追加する。
- テンプレートによって用意されたバックログを含め、ワークスペースを確認する。
- クイックチャットを試して、アプリ自体について学ぶ。

## シナリオ

チームは、増え続けるバックログに対応するために AI エージェントを導入しています。Copilot app では、Issue の選択、エージェントの実行、変更のレビュー、pull request のマージを一か所から指示できます。このレッスンでは、アプリをインストールして接続し、プロジェクトについての会話を始められるようにします。

> [!NOTE]
> 対象となる Copilot プランが必要です。Copilot Free、Copilot Student、または有料プラン (Pro、Pro+、Business、Enterprise) を利用してください。Copilot Business または Copilot Enterprise を使用している場合、管理者が **GitHub Copilot app** ポリシーを有効にしておく必要があります。

## GitHub Copilot app をインストールして構成する

GitHub Copilot app を使うには、まずインストールします。Windows、macOS、Linux 向けのバージョンが用意されています。アプリをインストールして認証し、Tailspin Toys リポジトリを追加しましょう。

1. ブラウザーで [GitHub Copilot app のランディングページ][download-app]を開きます。
2. 使用しているプラットフォーム向けのアプリをダウンロードし、ランディングページの手順に従ってインストールします。
3. インストールが完了したら、アプリを開きます。
4. **Sign in to GitHub** を選択し、画面の指示に従って認証します。データレジデンシーを使用する GitHub Enterprise Cloud の場合は、**Use GitHub Enterprise** を選択し、求められたら `*.ghe.com` のホスト名を入力します。GitHub Enterprise Server はサポートされていません。
5. オンボーディングでリポジトリを選択する画面が表示された場合は、テンプレートから作成した Tailspin Toys リポジトリを選択します。リポジトリの選択をスキップし、オンボーディング後にプロジェクトを追加することもできます。
6. テーマを選択する画面が表示されたら選び、オンボーディングを完了します。
7. サイドバーの **Sessions** にリポジトリが表示されているか確認します。すでに表示されていれば、再度追加する必要はありません。
8. まだ接続されていない場合は、**Sessions** の横にある **+** を選択して新しいセッションを作成し、**Add GitHub repository** を選択します。前提条件のレッスンで記録した所有者名とリポジトリ名を使用します。
9. サイドバーにリポジトリが表示されることを確認します。ここで使用するレイアウトでは、`tailspin-toys` などのリポジトリフォルダーが **Sessions** の下にグループ化され、その下に個々のセッションが表示されます。

## ワークスペースを確認する

プロジェクトを接続したら、ワークスペースの各領域を確認しましょう。アプリでは、サイドバーに主な機能がまとめられています。

- **New** - ここから Copilot との新しいチャットセッションを開始できます。
- **Pull requests** - アプリの GitHub ネイティブ統合を通じて pull request を表示します。アプリを離れずに pull request の参照や絞り込み、CI ステータスの確認、レビューを行えます。
- **Issues** - 同じように Issue を表示します。Issue の参照や絞り込み、Issue からのセッション開始を行えます。
- **Automations** — スケジュールまたはオンデマンドで実行する、保存済みのエージェントタスクです。ToDo リストの管理、定期的なプロジェクトの保守、その他の面倒な作業に役立ちます。まとめでは次のステップとして紹介しますが、ワークショップ内の演習には含めません。
- **Customize** - MCP server、プラグイン、スキルなどのコンポーネントを追加して、Copilot app の機能を拡張します。Playwright MCP の設定に使用します。
- **Chats** — ブランチや専用ワークスペースを必要としない、質問やブレインストーミング向けの簡易的な会話です。最初のチャットを開始すると、このセクションが表示されます。表示されていない場合は、**Sessions** の横にある **+** を選択して最初のチャットを開始してください。
- **Sessions** — リポジトリフォルダーと、そのエージェントセッションをグループ化します。`tailspin-toys` などのリポジトリ名を選択するとリポジトリビューが開き、その下のセッションを選ぶと会話に戻ります。各セッションは分離されたワークスペースで実行されるため、変更を競合させずに複数のセッションを実行できます。最初のセッションは星評価を追加するときに開始します。

ワークショップを進めながら、ワークスペースの機能をさらに確認していきます。

> [!TIP]
> 迷ったときは Copilot に質問できます。操作方法がわからない場合や、何かが可能かどうか知りたい場合は、Copilot に尋ねてみてください。

### 用意されたバックログを確認する

ほとんどのプロジェクトにはバックログがあり、Tailspin Toys も例外ではありません。テンプレートからリポジトリを作成したときに用意された Issue を確認しましょう。

1. サイドバーの **Sessions** で、個別のセッションではなく Tailspin Toys のリポジトリフォルダーを選択します。次に、リポジトリの上部ナビゲーションで **Issues** を選択します。
2. Issue 番号を前提にせず、次のタイトルで Issue を探します。

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Issue を選択して詳細を読みます。各 Issue はエージェントセッションの開始点にもなります。最初の小さな変更を終えた後、フィルター機能の Issue から作業を開始します。

> [!NOTE]
> **Issues** と **Pull requests** の一覧には、Copilot app に追加したリポジトリの項目だけが自動的に表示されます。他のリポジトリの作業項目を表示するには、そのリポジトリもアプリに追加してください。

## クイックチャットを試す

アプリ自体について質問すると、アプリに慣れることができます。**quick chat** は、ブランチや worktree を作らずに質問やブレインストーミングを行えるため、セッションを必要としない手早い質問に適しています。

1. サイドバーの **Chats** の横にある **+** を選択し、新しいチャットを開きます。**Chats** がまだ表示されていない場合は、**Sessions** の横にある **+** を選択して最初のチャットを開始してください。開始後に **Chats** が表示されます。
2. GitHub Copilot app のセッションの仕組みについて、次のように質問します。

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. 会話ビューで回答を読みます。各セッションが独立した git worktree で実行されるため、変更を競合させずに複数のエージェントを並列実行できることがわかります。会話は続けることも、いつでも新しいチャットを開始することもできます。

## まとめと次のステップ

GitHub Copilot app をインストールし、プロジェクトを接続して、ワークスペースを確認しました。このレッスンでは、次の作業を行いました。

- アプリをインストールして GitHub にサインインした。
- GitHub リポジトリからプロジェクトを追加した。
- ワークスペースを確認し、**Issues** で用意されたバックログを見つけた。
- クイックチャットで手早い質問をした。

次は、[最初のエージェントセッションを開始し][next-lesson]、ゲームカードに星評価を表示します。

## リソース

- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app の概要][getting-started]
- [GitHub Copilot app でのエージェントセッションの操作][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
