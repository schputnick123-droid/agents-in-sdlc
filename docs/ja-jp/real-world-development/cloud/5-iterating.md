---
title: "演習 5 - GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの監視と管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、コードへのドキュメント追加、関連ゲーム機能、高コントラストとライトモードのアクセシビリティ機能を Copilot に依頼しました。次は、生成された変更をレビューし、必要に応じて Copilot に修正を依頼します。

### シナリオ

生成 AI を利用してもソフトウェア設計と DevOps の基本は変わりません。チームに公開する前に、Copilot が作成したコードをレビューして通常の開発プロセスを進めます。

## GitHub Copilot cloud agent のセキュリティ

Cloud agent は非同期に作業するため、安全性を保つ制約があります。

- Copilot はリポジトリを読み取れますが、書き込みは作業ブランチに限定されます。
- GitHub Actions 内に一時的な独立環境を作成して実行します。
- GitHub Actions ワークフローの実行には人間の承認が必要です。
- MCP サーバーを含む外部リソースへのアクセスは、既定で制限されています。

## 生成されたドキュメントをレビューする

1. github.com でリポジトリを開き、**Pull Requests** を選択します。
2. **Add missing documentation** などのタイトルの PR を開きます。`[WIP]` が付いている場合は Copilot の作業完了を待ちます。
3. **Files changed** タブを選択し、生成された TSDoc コメントやその他のドキュメントを確認します。Exercise 2 で確認した TSDoc、TypeScript 規約、ファイルヘッダーに注目します。
4. **Conversation** タブに戻り、ワークフローの承認待ち表示があれば **Approve and run workflows** を選択します。
5. チェックの完了を待ち、単一の Astro アプリ用プロジェクトチェックが成功したことを確認します。

> [!NOTE]
> Copilot がファイアウォールでブロックされたという警告が表示される場合があります。外部リソースや MCP サーバーへのアクセスは既定で制限されているため、これは想定された動作です。必要に応じて [cloud agent のファイアウォール][agent-firewall]を構成できます。

## GitHub Copilot に変更を依頼する

PR のコメントで `@copilot` に変更を依頼できます。たとえば、各ファイルに内容を説明するヘッダーコメントを追加する、TypeScript と Astro に TSDoc コメントを追加する、Astro アプリ構成を説明する README を作成する、といった依頼ができます。

1. PR にコメントを追加し、`@copilot` をタグ付けしてドキュメントの変更を依頼します。
2. **View Session** を選択して Copilot の作業を確認します。変更用に新しいセッションが始まります。
3. **Back to pull request** を選択して PR に戻ります。
4. 新しいコミットが追加されたら、**Files changed** タブで差分を再確認します。

内容に納得できるまで反復します。その後、ドラフト PR をレビュー可能な状態にして `main` にマージできます。

## 関連ゲーム機能をレビューする

1. リポジトリの **Pull Requests** タブを開き、**Show related games on the game details page** などの PR を選択します。
2. **Files changed** タブでコードをレビューします。
3. **Conversation** タブに戻り、承認待ちのワークフローがあれば **Approve and run workflows** を選択します。
4. チェックの完了を待ち、単一の Astro アプリのプロジェクトチェックが成功したことを確認します。
5. 省略可能: Codespace のターミナルで Copilot が作成したブランチに切り替えて手動テストします (`<branch-name>` は実際のブランチ名に置き換えます)。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

必要なら PR のコメントで Copilot に追加変更を依頼できます。関連ゲームの数を調整する、TSDoc コメントを追加するなどの変更を依頼してください。cloud agent がカスタム指示を変更する前に割り当てられたタスクであるため、指示の適用状況も確認します。

## アクセシビリティ機能をレビューする

1. **Pull Requests** タブで **Add high contrast mode to website** などの PR を開きます。Copilot の作業中は `[WIP]` が表示される場合があります。
2. **Files changed** タブで実装をレビューします。次の点を確認します。
   - 表示モードを切り替える UI コンポーネント。
   - 利用者の設定を保持するローカルストレージ。
   - 高コントラストとライトモードの CSS またはスタイル。
   - ARIA ラベルやキーボード操作などのアクセシビリティ属性。
   - モード切り替えを管理する JavaScript/TypeScript。
3. **Conversation** タブに戻り、承認待ちのワークフローがあれば **Approve and run workflows** を選択します。
4. チェックが完了したら結果を確認します。
5. 省略可能: Codespace でブランチをチェックアウトして手動テストします。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

6. アプリを起動し、両方の表示モードを切り替えて、ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティ上の問題や改善点があれば、PR コメントで `@copilot` に追加変更を依頼します。

## 省略可能な演習 - さらに作業を委任する

バックログから Issue を作り、Copilot に割り当てて、別の作業を進める練習をします。たとえば、ゲーム詳細ページの支援者向けフォーム、ゲーム一覧のページネーション、データアクセスヘルパーの入力検証とエラー処理を依頼できます。

## まとめ

Cloud agent のハーネスを完了しました。カスタム指示を確認し、Issue を cloud agent に割り当てて非同期の作業を確認し、アクセシビリティ用カスタムエージェントを使って高コントラストとライトモードを追加し、agents ページで進捗を監視して方向付けし、pull request をレビューして `@copilot` に修正を依頼しました。

## レビューと次のステップ

このハーネスはここで完了です。別の環境から関連する作業を続ける場合は、[VS Code ハーネス][vscode]または[CLI ハーネス][cli]を確認してください。自分のリポジトリでは、リファクタリングやテストカバレッジの Issue を割り当てたり、別分野のカスタムエージェントを作ったり、異なる技術スタック向けに `copilot-setup-steps.yml` を設定したりできます。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの監視と管理][previous-lesson] |
|:--|

[previous-lesson]: ../4-managing-agents/
[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[vscode]: ../../vscode/
[cli]: ../../cli/
