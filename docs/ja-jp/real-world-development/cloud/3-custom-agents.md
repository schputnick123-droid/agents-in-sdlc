---
title: "演習 3 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に特化した AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーまたはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。YAML フロントマターには `name` と `description` を含め、その後に動作、専門知識、指示を記述します。

[エージェントスキル][agent-skills-concept]との違いは、カスタムエージェントが作業者であり、スキルはツールとして機能することです。エージェントには独自のコンテキストウィンドウがあり、タスクの一部としてスキルやほかのエージェントを調整できます。

> [!NOTE]
> カスタムエージェントを作成する唯一の正解はありません。環境やシナリオに合わせて試し、改善してください。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず誰もが使えるサイトを目指しています。現在のダークテーマは文字と背景のコントラストが低く、読みにくいというフィードバックがありました。デザインチームは、利用者が切り替えられる高コントラストモードを求めています。この演習ではアクセシビリティ用カスタムエージェントを使って機能を実装します。

## アクセシビリティ用カスタムエージェントを確認する

既存のエージェントの指示を確認してから Issue に割り当てます。

1. Codespace に戻り、`.github/agents/accessibility.md` を開きます。
2. `name` と `description` を含む YAML フロントマターを確認します。カスタムエージェントにはこれらが必須です。
3. アクセシブルなサイト向けの責任、ベストプラクティス、HTML/CSS/JavaScript のコード例、よくある問題を確認します。

## Issue を作成して割り当てる

GitHub のエージェントページは、エージェントのタスクを割り当てて監視し、追加の指示を送るための場所です。高コントラストモードの Issue を作成してアクセシビリティエージェントに割り当てます。

1. リポジトリを開き、**Issues** タブを選択します。
2. **New issue**、**Blank issue** の順に選択します。
3. タイトルに `Add high contrast mode to website` と入力します。
4. 説明に次の内容を入力します。

   ```text
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

5. **Create** を選択して Issue を作成します。
6. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。
7. 一覧から **Accessibility agent** を選択し、**Assign** を選択します。

    ![カスタムエージェントと Accessibility が選択された Copilot cloud agent の割り当て画面](../../../_images/ex5-select-custom-agent.png)

Copilot はバックグラウンドで作業を開始します。次の演習では、エージェントのセッションを監視して方向付けします。

## まとめと次のステップ

カスタムエージェントが特定のタスクや分野に向けた再利用可能なアシスタントであることを確認し、GitHub Issue に割り当てました。次は、[エージェントページからセッションを監視して方向付けします][next-lesson]。

## リソース

- [カスタムエージェントについて][custom-agents]
- [Organization でカスタムエージェントを使う準備][org-custom-agents]
- [Enterprise でカスタムエージェントを使う準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
