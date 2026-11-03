---
title: "レッスン 6 - Playwright MCP による機能の検証"
description: "Copilot CLI で Playwright MCP を確認または構成し、ブラウザーでフィルター機能を確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書く作業には、データや外部サービスとの連携も含まれます。Model Context Protocol (MCP) サーバーを使うと、Copilot CLI に組み込まれていないツールやサービスを追加できます。

このレッスンでは、Model Context Protocol (MCP) と Copilot CLI での利用方法を確認し、Playwright MCP サーバーを利用できるようにして、エージェントにブラウザーを操作させます。

## シナリオ

単体テストとエンドツーエンドテストは重要ですが、UI を検証するには実際に操作する必要があります。Copilot にユーザーと同じように Web サイトを使わせ、変更が期待どおりに動作するか確認します。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部のツールやサービスと通信し、最新情報にアクセスしたり、ユーザーに代わって操作したりするための仕組みです。MCP サーバーはエージェントと外部ツールの橋渡しをし、利用可能な tools と resources を提供します。

- **[GitHub MCP Server][github-mcp]** は、GitHub リポジトリ、Issue、pull request を管理する API にアクセスします。
- **[Playwright MCP Server][playwright-mcp-server]** は Playwright によるブラウザー自動化機能を提供します。

GitHub は、MCP サーバーの発見とエコシステムへの貢献を促進する [MCP registry][mcp-registry] をホストしています。

> [!CAUTION]
> MCP サーバーは、プロジェクト内のほかの依存関係と同様に扱ってください。使用前にソースコードと発行元を確認し、セキュリティへの影響を考慮します。信頼できるサーバーだけを使い、機密性の高いリソースや操作へのアクセスには注意してください。

## Playwright MCP サーバーを追加する

Playwright MCP がすでに Copilot CLI で設定されている場合は、追加手順を省略して次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力します。

   ```text
   /mcp add
   ```

3. 名前に `playwright` と入力し、<kbd>Tab</kbd> を押します。
4. サーバーの種類が **STDIO** であることを確認し、<kbd>Enter</kbd> を押してから <kbd>Tab</kbd> を押します。
5. **Command** の入力欄に次を貼り付けます。

   ```text
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) でサーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright を使って機能を確認する

計画時の決定事項と Issue は、すでに会話のコンテキストに含まれています。次のプロンプトで Copilot にフィルター機能を検証させます。

1. Copilot に次のプロンプトを送信します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

2. Copilot がブラウザーで実行する手順を確認します。
3. レポートを読み、動作が期待どおりか確認します。Playwright MCP が見つからない場合は、設定を確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作してからサーバーを停止し、結果を報告します。MCP の利用を明示しなくても Copilot が適切なサーバーを選ぶ場合がありますが、必要なツールを伝えると結果が安定しやすくなります。

## まとめと次のステップ

Copilot CLI から Playwright MCP を使って、実際のブラウザーで機能を確認しました。次は、スキルとブラウザーツールを組み合わせる[ QA カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
