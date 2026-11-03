---
title: "レッスン 5 - quality-checks スキルのカスタマイズと使用"
description: "既存の quality-checks スキルを調べ、レポート形式をカスタマイズして、フィルター機能の検証に使用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書く作業には、テスト、lint、継続的インテグレーション (CI) のほかの作業も含まれます。このようなタスクには**エージェントスキル**が適しています。スキルは、Copilot が作業を適切に実行するための指示を提供します。

このレッスンでは、既存の `quality-checks` スキルを確認し、結果の形式をカスタマイズして、スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys は、単体テスト、lint、型チェックに `quality-checks` スキルを使用します。チームは結果を読みやすくするため、レポートの形式を改善したいと考えています。

## 指示、スクリプト、リソース

エージェントスキルは、再利用可能なタスク指示、実行可能なスクリプト、補助リソースをまとめたもので、エージェントが必要に応じて読み込みます。基本的には、スキル名のフォルダーと、その中の `SKILL.md` で構成されます。Markdown には、スキルの名前と説明を定義するフロントマター、概要、呼び出すタイミングのガイダンスが含まれます。必要に応じて、スクリプトやリソースをサブフォルダーに追加できます。

> [!NOTE]
> この例では、プロジェクトの既存コマンドを使ってテストと linter を実行するため、追加のフォルダーやファイルは必要ありません。`quality-checks` スキルには `SKILL.md` だけが含まれています。

スキルを `.github/skills` に置くと、チームで共有できるリポジトリアセットになります。ユーザースキルフォルダー `~/.copilot/skills` にも配置できます。

## スキルを確認する

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 冒頭の `name` と `description` を読みます。Copilot は説明を使ってスキルを呼び出すタイミングを判断します。
3. 指示を読み、テストと lint の手順をどう案内するか確認します。
4. **Results output formatting** セクションがまだないことを確認します。

## 変更前にスキルを実行する

スキルは Copilot CLI から直接呼び出すことも、自然言語で実行を依頼することもできます。

1. Interactive モードのフィルター機能の会話に戻ります。
2. 次のプロンプトを送信します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

何を実行したか、成功したか、ツールが実際に何を報告したかがわかるよう、スキルを更新します。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルが自動的に保存されたことを確認します。

## 更新したスキルを実行する

Copilot CLI は会話を再起動せずに、編集したスキルを再読み込みできます。

1. 次のコマンドを入力します。

   ```text
   /skills reload
   ```

2. 変更前とまったく同じプロンプトを送信します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. レポートを確認し、最初の実行結果と比較します。

## まとめと次のステップ

既存のエージェントスキルを確認し、レポート形式をカスタマイズして、スキルを再読み込みして実行しました。この変更はフィルター機能の PR に含めます。次は、[Playwright MCP サーバーを通じて][next-lesson] Copilot にサイトを操作させます。

## ほかのスキルの例

次のコミュニティの例は参考資料であり、追加のタスクではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューションのワークフロー: `make-repo-contribution`][contribution-example]
- [要件文書: `prd`][prd-example]
- [図と同梱のエクスポートスクリプト: `drawio`][drawio-example]
- [ブラウザーテスト: `webapp-testing`][browser-example]

上流のコントリビューション例は `make-repo-contribution` という名前です。古い Tailspin テンプレートでは `make-contribution` という異なる名前が使われていました。このワークショップは、どちらのコントリビューションスキルにも依存しません。

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
