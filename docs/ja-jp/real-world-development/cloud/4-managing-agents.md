---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

これまでに、Copilot cloud agent にユーザー体験の改善と機能追加に関する3つのタスクを依頼しました。非同期かつ自律的に作業するエージェントでも、進捗を監視することが重要です。GitHub の[エージェントページ][agents-page]では、開いている pull request (PR) があるタスクを確認し、作業中のセッションに追加の指示を送れます。

この演習では、エージェントページでタスクを監視し、進行中のセッションに機能を追加するよう依頼します。

## シナリオ

高コントラストモードを実装するエージェントの作業中に、デザインチームからライトモードも追加してほしいという要望が届きました。同じスタイル変更に含められるため、作業中のエージェントに追加で依頼します。

## Copilot cloud agent のタスクを確認する

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メインペインと左ペインのタスク一覧を確認します。コードのドキュメント追加、ゲームを変更する API の生成、高コントラストモードの追加など、割り当てたタスクが表示されます。
3. 実行中のタスクを選択し、コードのチェックアウト、環境の作成、MCP サーバーの設定など、Copilot が実行した作業を確認します。表示される詳細はタスクの状態や実行方法によって異なります。
4. 右側の PR ペインで、pull request と変更ファイルも確認します。

## Cloud agent に追加の指示を送る

1. 高コントラストモードのセッションを選択します。表示されるタイトルは Copilot の命名や作業状態によって異なります。
2. セットアップが完了し、作業が始まったことを示すメッセージが表示されるまで待ちます。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

    ![Copilot agent のタスク画面で、作業中のセッションを方向付けるダイアログを示した画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot が追加の依頼を認識し、作業に含めたことを確認します。

Copilot は現在進めている手順を終えた後、更新されたタスクに取り組みます。非同期作業のため、完了を待つ間は別の作業を進められます。

## まとめと次のステップ

Copilot agents ページでタスクを監視し、セッションの進行中に追加機能を依頼しました。次は、[Copilot が作成した pull request を反復してレビューします][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
