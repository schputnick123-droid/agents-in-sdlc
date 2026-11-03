---
title: "レッスン 4 - カスタム指示による Copilot のガイド"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を扱うとき、コンテキストは重要です。タスクの進め方に希望がある場合、そのガイダンスを Copilot が利用できるようにします。[指示ファイル][instruction-files]には、必要なコードだけでなく、その構成方法も記述できます。フィルター機能を構築したので、Copilot が使った指示を確認し、ドキュメント標準を追加してコードに適用します。

このレッスンでは、次の内容に取り組みます。

- リポジトリの指示とパス固有の指示ファイルがエージェントに届く仕組みを確認する。
- コーディング標準に従うよう指示ファイルを更新する。
- 指示ファイルがコードに与える影響を確認する。

## シナリオ

Tailspin Toys では、コメントで意図や自明ではない判断を説明すること、`db/` と `src/lib/` のエクスポート関数に TypeScript ドキュメントコメント (TSDoc) または JavaScript ドキュメントコメント (JSDoc) で目的、パラメーター、戻り値 (注入可能な `db` 引数を含む) を記述すること、再利用可能な Astro コンポーネントの `Props` 契約を文書化すること、既存の書式と lint のガイダンスを保持することを標準にしています。指示ファイルを使い、Copilot がこれらの慣行に沿って作業できるようにします。

## 指示ファイル

カスタム指示は、コーディングスタイル、要件、使用ライブラリ、コメントの慣習などを Copilot に伝えます。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一の指示ファイルです。技術スタックやプロジェクトの概要など、プロジェクト全体に関係する情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種類、タスクに関する指針を記載します。

> [!NOTE]
> ほかの指示形式やサポート状況は環境によって異なります。形式に依存する前に、[カスタム指示のサポートリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトの指示ファイルを確認する

スタータープロジェクトに含まれる指示を確認します。

1. Codespace に戻ります。
2. Codespaces エディターで `.github/copilot-instructions.md` を開きます。
3. プロジェクトの説明とガイダンスを確認します。これらの指示は、このリポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、Astro、Drizzle データレイヤー、テストなどの指示を確認します。
5. `.github/instructions/unit-tests.instructions.md` を開き、`applyTo` フィールドが適用対象を決める glob であることを確認します。
6. `.github/instructions/drizzle.instructions.md` のリンクを確認します。指示を小さく再利用可能なファイルに分け、Copilot に既存コードの例を示せます。

## チームのガイダンスに合わせて指示を更新する

フィルター機能を実装したブランチ上で、生成される TypeScript に TSDoc コメントを追加する標準を設定します。

1. `.github/copilot-instructions.md` を開きます。
2. **Code formatting guidance** セクション (およそ35行目) を探します。
3. 最後の箇条書きとして、次を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

指示ファイルは自動的に保存されます。

## 更新したガイダンスを使用する

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。機能のコンテキストを保ったまま新しい指示を読み込むため、フィルター機能の会話を再開します。

1. フィルター機能の会話が開いている場合は `/exit` と入力して終了します。会話は自動的に保存されます。
2. リポジトリのルートにある `game-filters-cli` ブランチから、保存済みの会話を再開します。

   ```bash
   copilot --resume
   ```

3. 選択を求められたらフィルター機能の会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
4. 次のプロンプトを送信します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

5. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを読み込む仕組みを確認し、新しい標準を機能に適用しました。このレッスンでは、次の作業を行いました。

- リポジトリの指示とパス固有の指示ファイルを確認した。
- コーディング標準に従うよう指示ファイルを更新した。
- 指示ファイルがコードに与える影響を確認した。

次は、[再利用可能な quality-checks スキルをカスタマイズして実行します][next-lesson]。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示作成のベストプラクティス][instructions-best-practices]
- [Awesome Copilot - 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
