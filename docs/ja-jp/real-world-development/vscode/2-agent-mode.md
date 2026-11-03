---
title: "演習 2 - Copilot Agent Mode による機能追加"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

小さなアプリの変更でも、複数のファイルを更新し、テストを実行する必要があります。必要なファイルを見つけ、変更し、テストし、失敗を調査して不足したファイルを直す作業を、Copilot Agent Mode に任せられます。

Copilot Agent Mode は IDE 内でより自律的に作業します。プロジェクト構成を調査し、必要なファイルを更新し、テストなどのタスクを実行して、見つかった問題を修正します。

この演習では、次の内容を学びます。

- Copilot Agent Mode でプロジェクトを調査し、関連ファイルを特定して、複数ファイルにわたる変更を調整する。
- GitHub Copilot Agent Mode で UI とデータレイヤーにまたがる機能を実装する。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする。

> [!NOTE]
> agent mode と cloud agent は異なる体験です。agent mode は IDE 内で作業し、すばやいフィードバックと対話を可能にします。cloud agent は Issue と pull request を通じて非同期にチームの一員のように作業します。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリーとパブリッシャーで絞り込めるようにします。前の演習で追加したパブリッシャー用ヘルパーを利用し、残りのデータレイヤー、UI、テストを Copilot Agent Mode で完成させます。

## Tailspin Toys の Web サイトを実行する

Tailspin Toys は開発者をテーマにしたボードゲームのクラウドファンディングサイトです。Astro アプリ1つで構成され、ビルド時にページを静的 HTML として生成します。ページは Drizzle ORM を通じてローカル SQLite データベースを直接参照します。別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` に置き、インタラクティブな動作には標準 DOM API を使う小規模な Astro の `<script>` を使用します。

1. Codespace に戻ります。前の演習を終えて `main` にいることを確認します。
2. Codespace 内で新しいターミナルを開きます。macOS では <kbd>Control</kbd>+<kbd>\`</kbd>、Windows または Linux では <kbd>Ctrl</kbd>+<kbd>\`</kbd> を押します。
3. `main` を更新してフィルター用ブランチを作成します。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL を含むバナーが表示されます。

   ```text
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くブラウザーウィンドウのダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルで Astro サーバーのアドレス `http://localhost:4321` を選択します。macOS では <kbd>Command</kbd> キー、Windows または Linux では <kbd>Ctrl</kbd> キーを押しながら選択します。

> [!NOTE]
> Codespace のターミナルで localhost URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的にリダイレクトされます。これは、Web サーバーをホストする Codespace へのプライベートトンネルです。

## Web サイトを確認する

サイトを起動したら、機能を確認します。主なページは次のとおりです。

- **ホームページ**: タイトル、画像、説明を含むボードゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、パブリッシャー、カテゴリーなどの詳細を表示します。

## Copilot でバックログを確認する

Web サイトの初期状態を確認したら、テンプレートから作成されたバックログの Issue を Copilot に表示させます。まず Codespace に戻り、Copilot Chat で **New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習ごとに異なるため、該当する手順で指定します。

1. Chat ビューのエージェント選択で **Agent** を選びます。Agent は複数ファイルにわたる変更の計画と実装、ターミナルコマンドの実行、ツールの呼び出しを自律的に行います。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> ここで特定のモデルを推奨するものではありません。このワークショップの作成時に Claude Sonnet 4.5 を使用したため、手順にも記載しています。提案の一貫性を高める意図がありますが、大規模言語モデル (LLM) は確率的に動作するため、ワークショップの例と異なる提案が表示されることがあります。これは正常です。

> [!NOTE]
> Copilot が別の MCP コマンドを使用する場合がありますが、タスクを完了できれば問題ありません。

3. 次のプロンプトを送信し、リポジトリの Issue 一覧とユーザーにとって有用な順の優先順位を尋ねます。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Issue 一覧を取得するコマンドの実行時に **Continue** を選択します。
5. 生成された Issue 一覧を確認します。Copilot はユーザーにとって特に有用だと判断した項目を優先順位付けします。

## 指示ファイルを確認する

コードを生成する前に、Copilot にコンテキストを提供する指示ファイルを確認します。Web サイトへの機能追加の進め方を説明する UI 指示ファイルを使用します。

1. Tailspin Toys リポジトリ内の `.github/instructions/ui.instructions.md` を開きます。
2. アーキテクチャの概要、コンポーネント設計、テスト容易性、アクセシビリティの指針を確認します。Astro、Tailwind CSS、Drizzle 向けの指示へのリンクも確認します。リンク先の相対パスは指示ファイルの場所を基準とします。

> [!TIP]
> 指示ファイルから、ほかの指示ファイルやプロジェクト内のファイルを参照できます。相対パスは指示ファイルの場所を基準とします。これにより、内容の再利用や、指示の分割、例やテンプレートの提示ができます。

## フィルター機能を実装する

フィルター機能には、データレイヤーの処理、テスト、ゲーム一覧ページの UI が必要です。Copilot に Issue の要件に基づいて残りの作業を実装させます。

1. 現在の会話を続けるか、**New Chat** で新しい会話を開始します。

2. **Add Context**、**Instructions**、**ui** の順に選択して指示ファイルを追加します。

   ![UI 指示ファイルを選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. エージェント選択で **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. モデル選択で **Claude Sonnet 4.5** が選択されていることを確認します。
5. 次のプロンプトを送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調査して関連ファイルを見つけ、データレイヤー、UI、テストを更新し、テストを実行する様子を確認します。

   ![Copilot がプロジェクトのファイルを調査する様子を示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの更新、テストの実行などを行うため、複雑さやコードベースによって数分かかることがあります。作業中にエディターに **Keep** と **Undo** が表示される場合がありますが、処理中に選択する必要はありません。完了すると、すべての変更に **Keep** または **Undo** が表示されます。

7. Copilot に求められたら **Continue** を選択してテストを実行します。

   ![Copilot Chat でテスト実行の確認を求めるダイアログのスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 処理中に一時停止したり、テストが失敗したりすることがあります。Copilot はコード生成とテストを繰り返し、エラーを検出しなくなるまで作業します。

   ![Copilot Agent Mode の完了した Chat セッションを示すスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードも、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新しい機能を試します。
11. 機能が動作することを確認し、コードをレビューしてから、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

Copilot Agent Mode を使い、UI とデータレイヤーにまたがる機能を実装し、プロジェクトの関連ファイルを調査して変更を調整し、生成された変更とテストをレビューしました。次は、[Playwright MCP サーバーで機能をテストし][next-lesson]、pull request を作成します。

### 追加演習 - ページネーションを実装する

ゲーム一覧が増えた場合に備え、既存の指示ファイルを使いながら、ページサイズの選択または固定サイズなどの要件を決めて Copilot にページネーションを実装させます。必要に応じてプロンプトを改善し、追加のコンテキストを提供してください。

## リソース

- [Copilot の ask、edit、agent モードと使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
