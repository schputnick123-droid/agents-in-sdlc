---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に特化した AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーまたはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。YAML フロントマターには `name` と `description` を含め、その後に動作、専門知識、指示を記述します。

[エージェントスキル][agent-skills-concept]との違いは、カスタムエージェントが作業者であり、スキルはツールとして機能することです。エージェントには独自のコンテキストウィンドウがあり、スキルや他のエージェントを調整できます。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。AI のほかの活用方法と同様に、環境やシナリオに合わせて試し、改善してください。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず誰もが使えるサイトを目指しています。現在のダークテーマは文字と背景のコントラストが低く、読みにくいというフィードバックがありました。デザインチームは、利用者が切り替えられる高コントラストモードを求めています。アクセシビリティ用カスタムエージェントに実装を依頼します。

この演習では、既存のアクセシビリティエージェントを確認し、Copilot Chat で使って高コントラストモードを実装します。

## アクセシビリティ用カスタムエージェントを確認する

1. Codespace に戻り、新しいブランチを `main` から作成します。フィルター PR とは分けて作業します。

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. `.github/agents/accessibility.md` を開きます。
3. `name` と `description` を含む YAML フロントマターを確認します。これらはカスタムエージェントに必須です。
4. アクセシブルなサイト向けの責任、ベストプラクティス、HTML/CSS/JavaScript の例、よくある問題を確認します。

## Copilot Chat でカスタムエージェントを使う

VS Code の Copilot Chat ビュー下部にあるエージェント選択には、`.github/agents` で定義されたカスタムエージェントが表示されます。

1. Copilot Chat を開き、**New Chat** で新しい会話を開始します。
2. Chat ビューのエージェント選択で **Agent** を選択します。
3. エージェント選択を開き、**Accessibility agent** を選択します。
4. 次のプロンプトを送信します。

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot がコードベースを調査し、変更を提案して適用する様子を確認します。変更はチャット内にファイルパスと差分付きで表示されます。
6. **Files changed** の一覧で、エージェントが変更したファイルを確認します。

処理には数分かかる場合があります。次の演習では、進行中の作業をレビューし、方向付けします。

## まとめと次のステップ

カスタムエージェントが専門的なタスクにどう使えるかを確認しました。次は、同じセッションの作業を[監視して方向付けし][next-lesson]、ライトモードの切り替えも追加します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの構成][custom-agents-config]
- [Awesome Copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
