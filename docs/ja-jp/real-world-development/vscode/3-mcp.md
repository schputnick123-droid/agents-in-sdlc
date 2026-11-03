---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェントモード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェントモードでフィルター機能を構築しました。pull request を作成する前に、ブラウザーで実際に動作することを確認します。**Playwright MCP サーバー**を接続し、Copilot にブラウザーを操作させて機能をテストしてから、ブランチを公開して PR を作成します。

この演習では、Model Context Protocol (MCP) の概要を確認し、Playwright MCP サーバーを追加して、Copilot にブラウザーで機能をテストさせ、フィルター機能の PR を作成します。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部のツールやサービスと通信するための標準です。MCP サーバーはエージェントと外部ツールの橋渡しをし、利用可能な tools と resources を提供します。

![エージェントモードがコンテキスト、LLM、ツール (MCP サーバーや VS Code 拡張機能が提供するツールを含む) と連携する仕組みの図](../../../_images/ex1-mcp-diagram.png)

よく使われるサーバーには、リポジトリ、Issue、pull request を管理する **[GitHub MCP Server](https://github.com/github/github-mcp-server)** と、ブラウザー自動化を行う **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)** があります。GitHub は [MCP registry](https://github.com/mcp) もホストしています。

> [!CAUTION]
> MCP サーバーはプロジェクト内のほかの依存関係と同様に扱ってください。使用前にソースコードと発行元を確認し、セキュリティへの影響を考慮します。信頼できるサーバーだけを使い、機密性の高いリソースや操作へのアクセスには注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` は、この VS Code ワークスペースで利用する MCP サーバーを構成します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. テンプレートに含まれる GitHub サーバーを確認します。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

このリモート GitHub MCP サーバーにより、前の演習で Copilot が Issue のバックログを読み取れました。ローカルにインストールする必要はなく、VS Code が HTTP 経由で接続し、最初のツール利用時に GitHub 認証を求めます。

## Playwright MCP サーバーを追加する

1. `.vscode/mcp.json` の `github` と並べて `playwright` エントリを追加します。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. ファイルを保存します。

`playwright` はローカルサーバーです。VS Code が `npx @playwright/mcp@latest` を実行して起動します。Codespace には表示用デスクトップがないため、`--headless` でブラウザーを画面表示なしで実行します。

> [!NOTE]
> Tailspin Toys の E2E テストではすでに Playwright を使うため、ブラウザーは通常インストール済みです。ブラウザーが見つからないと Copilot が報告した場合は、`npx playwright install chromium` を実行して再試行します。

## サーバーを起動して信頼する

VS Code は MCP サーバーを必要に応じて起動し、初回利用時に信頼を確認します。

1. `.vscode/mcp.json` の `playwright` エントリ上部に表示される **Start** を選択します。
2. 信頼の確認を求められたら構成を確認し、サーバーを信頼するよう選択します。手動で起動しない場合は、次の手順でツールが必要になったときに VS Code が起動します。

## フィルター機能をテストする

Playwright MCP サーバーを使うと、Copilot がページを開き、移動し、フィルターを適用して、結果を報告できます。

1. アプリが `http://localhost:4321` で実行中であることを確認し、Copilot に次のプロンプトを送信します。

   ```text
   Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

   1. Go to the games page and note how many games are listed.
   2. Apply a category filter and confirm the list updates to only show games in that category.
   3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
   4. Combine a category and a publisher filter and confirm the results respect both.

   Report what you observe at each step, and call out anything that does not behave as expected.
   ```

2. Copilot の報告を Issue の受け入れ条件と照らし合わせます。不具合があれば追加質問をするか、PR 作成前に修正を依頼します。
3. 開発サーバーを停止していた場合は、プロンプトを送信する前に再起動します。Playwright MCP の初回利用でブラウザーのダウンロードを求められたら、`npx playwright install chromium` を実行して再試行します。

## ブランチを公開して pull request を作成する

1. Codespace の **Source Control** パネルで Copilot の変更をレビューします。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンでコミットメッセージを生成します。
4. **Publish** を選択してブランチをリポジトリにプッシュします。
5. Copilot Chat で **New Chat** を選び、エージェント選択で **Agent** を選択します。
6. 次のプロンプトで、関連 Issue を探し、現在のブランチから PR を作成するよう依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

7. 必要に応じて **Continue** を選び、ツール利用や GitHub 認証を許可します。
8. Copilot が作成した PR のリンクを開いてレビューします。まだマージしないでください。

## まとめと次のステップ

Playwright MCP を追加し、フィルター機能をブラウザーでテストして、ブランチを公開し PR を作成しました。次は、[カスタムエージェントを作成して][next-lesson]、アクセシビリティレビューなどの専門タスクを効率化します。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
