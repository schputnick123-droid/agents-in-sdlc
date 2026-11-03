---
title: "レッスン 7 - QA エージェントの作成と使用"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせる QA カスタムエージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザーのフィルター動作を確認しました。ここでは、明確な品質保証 (QA) プロセスを持つカスタムエージェントにこれらの機能をまとめます。

このレッスンでは、指示、スキル、MCP ツールとカスタムエージェントの連携を確認し、再利用できる QA プロファイルを作成して、フィルター機能の Issue に照らして結果をレビューします。

## シナリオ

Tailspin Toys は、pull request (PR) を作成する前に、要件、コード品質、自動チェック、テストカバレッジ、ブラウザーでの動作を一貫して確認したいと考えています。カスタムエージェントで QA プロセスを調整し、再利用できるレポートを作成します。

## カスタムエージェントとは

カスタムエージェントは、Markdown プロファイルで定義する Copilot の専門的な役割です。プロファイルには目的、指示、利用できるツールを記述します。この演習では `.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

カスタマイズにはそれぞれ異なる役割があります。リポジトリの指示はチーム標準を説明し、`quality-checks` スキルは再利用可能なチェックをまとめ、Playwright MCP はブラウザーツールを提供します。QA プロファイルはこれらを使って要件を評価し、結果を報告するよう Copilot に指示します。

## QA プロファイルを作成する

機能の PR を開く前に、QA のチェック項目と権限の境界を定義したプロファイルを作ります。

1. Codespace に戻り、フィルター機能の会話が Interactive モードであることを確認します。
2. 既定のエージェントに次のプロファイル作成を依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

使用前に、プロファイルが意図した QA ワークフローと権限の境界を含むことを確認します。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. フロントマターを読みます。`description` は必須で、`name` は任意です。
3. 要件から始め、リポジトリの指示に従い、`quality-checks` スキルを実行し、Playwright MCP を使用することを確認します。
4. 証拠を添えて結果を報告し、実装コードの変更前に確認し、変更をコミットしたり PR を作成したりしないことを確認します。
5. 不足があれば、既定のエージェントに修正を依頼します。

## Issue に対して QA を実行する

Copilot CLI は会話開始時にプロジェクトのエージェントを読み込みます。プロファイルを作成した後、同じフィルター機能の会話を再開し、QA を選択します。

1. 次のコマンドを入力して QA を有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成直後は QA が使用可能なエージェント一覧に表示されない場合があります。その場合でも、上記のコマンドで有効にできます。

2. 次のプロンプトを送信します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA が作業を終えたら、レポートを読みます。正しい Issue と計画上の決定が使われていることも確認します。

## まとめと次のステップ

再利用可能な専門家の役割をワークフローに加え、結果をレビューしました。次は、実装、スキルの更新、QA プロファイル、テスト、検証レポートをまとめて[機能 PR を作成し、Agent Merge を使用します][next-lesson]。

## リソース

- [Copilot CLI 用のカスタムエージェントを作成する][create-agents]
- [カスタムエージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
