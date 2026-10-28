---
title: "レッスン 4 - カスタム指示で Copilot をガイドする"
description: "リポジトリの指示を確認し、ドキュメント標準を追加してフィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときはコンテキストが重要です。タスクを特定の方法で実行してほしい場合は、そのガイダンスを Copilot が参照できるようにします。[指示ファイル][instruction-files]には、作成するコードの内容だけでなく、その構成方法も記述できます。フィルター機能を作成したので、Copilot が参照した指示を確認し、ドキュメント標準を追加してコードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示とパスで適用範囲を指定した指示ファイルがエージェントに届く仕組みを確認します。
- コーディング標準に従うよう指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には開発プラクティスに関するガイドラインがあります。

- コメントにはコードの繰り返しではなく、意図や自明でない判断を記述します。
- `db/` と `src/lib/` のエクスポートされた関数には、`db` 引数を注入できる場合はその引数も含め、目的、パラメーター、戻り値を TypeScript Documentation (TSDoc)/JavaScript Documentation (JSDoc) で記述します。
- 再利用可能な Astro コンポーネントには `Props` の契約を記述し、コードを変更した場合はコメントも最新の状態にします。
- 既存のフォーマットと lint のガイダンスを維持します。

指示ファイルを使い、Copilot がこうしたプラクティスに沿って作業できるようにします。

## 指示ファイル

カスタム指示を使うと、コーディングスタイルや要件を Copilot に伝えられます。これにより、より適切な提案やコードを得られます。コーディング規約、ライブラリ、コードに含めるコメントの種類などを指定できます。リポジトリ全体に適用する指示も、特定のファイルやタスクに適用する指示も作成できます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリへのすべての要求で Copilot に送られる単一の指示ファイルです。技術スタック、開発内容の概要、ベストプラクティスなど、ほとんどのチャットや CLI 要求に関係するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、言語やファイルの種類 (TypeScript や Astro など)、UI コンポーネントや単体テストの作成などのタスクに合わせた指針を記載します。

> [!NOTE]
> 環境によって、その他の指示形式への対応状況は異なります。特定の形式を前提とする前に、[カスタム指示のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

変更の影響を確認するため、まずスタータープロジェクトに含まれる指示ファイルを調べます。

1. Codespace に戻ります。
2. Codespaces のエディターで `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディングガイダンスを確認します。この指示はリポジトリでの Copilot とのすべてのやりとりに適用されます。
4. `.github/instructions` フォルダーを開き、Astro、Drizzle データレイヤー、テストなどの指示ファイルを確認します。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。この glob によって、指示の適用対象が決まります。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルやプロジェクト内のファイルへのリンクを確認します。これにより、大きな指示を再利用できる小さなファイルに分け、Copilot が参考にできるファイルを示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存の指示に不足している点があります。中心となる `copilot-instructions.md` を変更し、新しく生成する TypeScript に TSDoc コメントを追加するよう指示します。

1. `.github/copilot-instructions.md` の **Code formatting guidance** セクションを探します。
2. 次の項目を最後の箇条書きとして追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを適用する

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。指示を保存した後、フィルター機能の会話を終了して再開すると、機能のコンテキストを保ったまま新しいガイダンスを読み込めます。

1. フィルター機能の会話を開いたままの場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートにある `game-filters-cli` ブランチから、次のコマンドで会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められた場合はフィルター機能の会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に次のプロンプトを送り、更新したガイダンスを適用します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot がこのプロジェクトの指示を読み取る仕組みを確認し、新しい標準を機能に適用しました。具体的には、次のことを行いました。

- リポジトリの指示とパス単位の指示ファイルがエージェントに届く仕組みを確認しました。
- コーディング標準に従うよう指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は[再利用可能な `quality-checks` スキルをカスタマイズして実行し][next-lesson]、lint とテストを一貫して実施します。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示のベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
