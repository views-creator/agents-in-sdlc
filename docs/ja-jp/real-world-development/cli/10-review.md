---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys の一連のワークフローで GitHub Copilot CLI を使用しました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールし、プロジェクトを確認して、登録済みのフィルター機能の Issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルター機能の Issue から始め、Plan モードでアプローチを決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム指示でエージェントをガイドし、既存の `quality-checks` スキルをカスタマイズして、単体テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を確認しました。
- 要件、テストカバレッジ、スキルの結果、ブラウザーでの証拠を評価する品質保証 (QA) カスタムエージェントを作成して選択しました。
- フィルター機能全体をレビューし、フィルター機能の PR で Agent Merge を許可しました。
- コンテキスト、モデル、共有、cloud agent への任意の委任に使うスラッシュコマンドを確認しました。

## リリースしたもの

ワークショップでは、更新済みの `main` からそれぞれブランチを作成し、2つの PR を行いました。

1. **星評価:** ゲームカードに既存の `starRating` と、評価がない場合の状態を表示します。
2. **フィルター機能と品質ワークフロー:** フィルター機能を実装し、指示を更新してその機能に適用し、`quality-checks` のレポートをカスタマイズして、QA プロファイルと関連テストを追加します。

フィルター機能の計画から PR の作成まで、同じ会話とブランチを使用しました。ワークショップを簡潔にするため、これらの作業を1つの PR にまとめました。

## 検証方法の違い

自動テスト、自分で行うブラウザー確認、MCP を使った Copilot によるブラウザー確認の複数の方法でコードを検証しました。`quality-checks` スキルは単体テスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA エージェントは PR の前に、これらの結果と要件およびテストカバレッジのレビューをまとめました。

追加するテストは実際の不足を補うものにします。新しいテストが不要な QA 実行も適切な結果になり得ます。ツールの不足、スキップされたチェック、失敗は明示すべき阻害要因であり、成功ではありません。コードと証拠を確認してからマージを承認し、変更後は関連する証拠を更新してください。

## ベストプラクティス

Copilot に与えるコンテキストとツールが、作業の結果を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを設定しました。チームのニーズに合わせてこれらを調整しながら、会話をまたいで再利用してください。指示は標準を設定し、スキルは繰り返し行うタスクを説明し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選択します。構築前にアプローチを検討するときは **Plan**、対象を絞った変更で作業に関与し続けるときは **Interactive**、範囲が明確なタスクには **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業にはより高い推論能力を持つモデルを選びます。

インフラストラクチャと同じくらいコンテキストも重要です。何を、なぜ、どのように作るかを明確に説明すると、出力は大きく変わります。

## さらに確認する機能

コアワークフローを学びました。Copilot CLI の次の機能も確認できます。

- `/review` でコードレビューエージェントに変更を分析させます。
- `/rubber-duck` で問題を話し合い、別の視点から意見を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスク用に分離された Git worktree を作成します。
- `/delegate` で Copilot cloud agent に作業を依頼します。

## 次のステップ

ツールを使いこなすには、使い続けることが一番です。実稼働コード、趣味のコード、いつか作ろうと思っていた小さなアプリなどで活用してください。チームと学んだことを共有し、チームからも学びましょう。引き続きドキュメントも確認してください。

ターミナルから Tailspin Toys を拡張する場合は、オプションの [Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境での作業も確認するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を参照してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI のカスタマイズ][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
