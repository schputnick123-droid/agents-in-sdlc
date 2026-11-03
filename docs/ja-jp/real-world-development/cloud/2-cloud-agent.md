---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

多くの組織では、セキュリティ問題、古いコード、バックログに残った機能などの技術的負債に取り組む時間が不足しています。GitHub Copilot cloud agent は、コードの更新や機能追加などを自律的に実行し、完了すると人間がレビューできるドラフトの pull request (PR) を作成します。

このレッスンでは、次の内容を確認します。

- コード生成環境をカスタマイズする。
- 操作が安全に行われるようにする。
- 範囲を明確にした Issue の重要性を理解する。
- Issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys には、最初のサイトを作成した請負業者が十分に整備しなかったため、ドキュメントがほとんどないという技術的負債があります。まず、アプリケーション内のすべてのエクスポート関数に TypeScript ドキュメントコメント (TSDoc) を追加します。また、デザインチームはゲーム詳細ページに同じカテゴリーの関連ゲームを表示し、完成度の高い実装ではなく、ユーザー体験 (UX) の受け入れテストに使えるものを求めています。これらは優先度の高い作業がほかにある間、cloud agent に任せられるタスクです。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent][copilot-agents] は、人間の開発者のようにバックグラウンドで作業できます。たとえば [GitHub Issue を Copilot に割り当て][assign-issue]ると、Copilot はドラフト PR を作成して進捗を記録し、環境を準備してタスクを始めます。作業中または完了後にセッションを確認し、レビューの準備ができると PR で通知されます。

## 範囲を明確にした指示の重要性

Copilot を使っても、考える作業が不要になるわけではありません。タスクを小さな段階に分け、試し、結果に応じて調整します。[Cloud agent 向けの作業環境とタスクのベストプラクティス][cloud-agent-best-practices]を確認してください。

## リポジトリのカスタム指示

cloud agent に作業を割り当てる前に、リポジトリに含まれる指示ファイルを確認します。

1. GitHub Web UI または Codespace で `.github/copilot-instructions.md` を開き、**Code standards** の TypeScript 規約と TSDoc コメントを確認します。
2. `.github/instructions/unit-tests.instructions.md` を開き、`applyTo` フロントマターが `**/*.test.ts` に適用されることを確認します。
3. cloud agent が後の演習で `Code lacks documentation` Issue を処理するとき、これらの指示とコメントヘッダーがどのように反映されるか確認します。

## Cloud agent の開発環境を設定する

誰がコードを書く場合でも、適切な状態で作業を始めるには、特定の環境やセットアップスクリプトが必要です。cloud agent も同様に、ソフトウェアエンジニアのように作業します。GitHub Actions を使って作業環境を準備し、`.github/workflows/copilot-setup-steps.yml` の[セットアップワークフロー][setup-workflow]で必要なツールと依存関係を設定できます。このラボ用ワークフローには、Node.js、プロジェクト依存関係、エンドツーエンド (E2E) テスト用ブラウザー、移行とシードを済ませた SQLite データベースのセットアップが含まれます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 手順に必要な最小限の権限を設定します。Copilot の操作には専用のトークンが付与されます。
    permissions:
      # 依存関係のインストールなどでセットアップ手順中にリポジトリをクローンする場合は、`contents: read` 権限が必要です。手順でクローンしない場合は、完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (現在、アプリ全体は Astro + Drizzle/libSQL で構成されています)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストでデータを利用できるよう、ローカル SQLite データベースを移行してシードします。
      - name: Set up the database
        run: npm run db:setup
```

このワークフローはほかの GitHub Actions ワークフローと同様ですが、重要な点がいくつかあります。

- `copilot-setup-steps` という単一のジョブがあり、Copilot が pull request の作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーがあるため、Copilot の実行を待たずにリポジトリの **Actions** タブから手動でテストできます。

## ドキュメントを追加する Issue を割り当てる

cloud agent にエクスポート関数への TSDoc コメント追加を依頼します。まず、Issue を作成してから Copilot に割り当てます。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して Issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次のテキストを入力します。

   ```text
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して Issue を作成します。
8. 画面右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

   ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

   ![Copilot の割り当て詳細画面](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. `[WIP]: Code lacks documentation` のようなタイトルの、新しく作成された pull request (PR) を開きます。PR が一覧に表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分待ち、Copilot がタスクリストを作成したことを確認します。

> [!NOTE]
> Copilot がタスクリストを作成するまで数分かかることがあります。Copilot は先ほどのワークフローを実行して環境を準備し、プロジェクトを分析して、作業の進め方を決めています。

13. タスクリストと、Copilot が実行する作業を確認します。
14. PR のタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを示す更新を確認します。
15. **View session** を選択します。

   ![Copilot のセッションビュー](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示を確認するため、ページの再読み込みが必要な場合があります。

16. ライブセッションをスクロールし、Copilot がコードを調査して状態を把握し、計画を決めてコードを作成する様子を確認します。

この処理には数分かかることがあります。cloud agent の主な利点は、作業を非同期で実行できることです。別の作業に集中できるため、次のタスクも cloud agent に割り当ててから、アプリケーションの機能追加に取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

関連ゲーム機能も cloud agent に割り当て、別の作業に集中できるようにします。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の要件を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して Issue を作成します。
8. 画面右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

   ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

しばらくすると、Issue の最初のコメントに Copilot が作業中であることを示す 👀 が表示されます。

![Copilot が作業中であることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は2つ目の依頼を処理しています。cloud agent はソフトウェアエンジニアのように作業するため、完了まで積極的に監視する必要はありません。次は[カスタムエージェントの作成と使用][next-lesson]に進みます。cloud agent は MCP サーバーも利用でき、前の演習で確認したカスタム指示も使用できます。

## まとめと次のステップ

このレッスンでは、[GitHub Copilot cloud agent][copilot-agents] を確認しました。cloud agent に Issue を割り当てると、技術的負債への対応、新機能の作成、コードのフレームワーク移行などを非同期で依頼できます。次の内容を確認しました。

- コード生成環境をカスタマイズした。
- 操作が安全に行われるようにした。
- 範囲を明確にした Issue の重要性を確認した。
- Issue を Copilot に割り当てた。

cloud agent がバックグラウンドで作業している間に、次は[カスタムエージェントの作成と使用][next-lesson]に進みます。[cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、前のレッスンで確認したカスタム指示も使用します。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
