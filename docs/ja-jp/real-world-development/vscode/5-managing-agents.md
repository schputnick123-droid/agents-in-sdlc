---
title: "演習 5 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の演習では Copilot Agent Mode に高コントラストモードを実装させました。作業中にデザインチームからライトモードの追加要望が届きました。新しい会話を始めずに、進行中の作業に追加の依頼を送ります。

この演習では、エージェントの作業をリアルタイムで監視し、提案された編集をレビューして受け入れまたは取り消し、作業中の会話にライトモードの要望を追加します。

## Copilot の進捗を監視する

Copilot Chat には、エージェントが調べたファイル、実行したコマンド、提案した編集が表示されます。編集には差分と **Keep** / **Undo** の操作が付きます。

1. 前の演習の Copilot Chat ビューに戻ります。
2. チャットをスクロールし、エージェントのツール呼び出しを確認します。
3. ファイル編集が提案されたら、**Keep** で採用するか **Undo** で取り消します。
4. Chat 上部の **Files changed** で、変更されたファイル一式を確認します。
5. 高コントラストモードの実装が完了するまで待ちます。ターミナルコマンドの実行などで **Allow** を求められた場合は、コマンドを確認してから許可します。

## ライトモードを追加するようエージェントを方向付ける

同じ会話を続けると、エージェントは先ほどのコンポーネント、スタイル、ローカルストレージのパターンを含むコンテキストを保持します。

1. Copilot Chat の入力欄に次の追加プロンプトを送信します。

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. エージェントが既存の変更を確認し、ライトモードを追加する様子を見ます。
3. 各差分をレビューし、**Keep** または **Undo** を選択します。
4. 意図しない方向に進んだ場合は、チャット入力欄の横にある **Stop** で停止して、明確な追加指示を送ります。

> [!TIP]
> 進行中のセッションへの追加指示は、新しい会話を始めるより効率的です。たとえば CSS を簡潔にする、設定パネルの構成を変えない、両方の切り替えにキーボードショートカットを追加する、といった要望を送れます。

## 変更内容をレビューする

コミットする前に、エージェントが変更した内容を確認します。

1. VS Code で **Source Control** ビューを開きます。
2. 変更されたファイル一覧を確認します。Astro コンポーネント、スタイル、関連テストなどが含まれる場合があります。
3. 変更したファイルの差分を確認し、ARIA 属性、キーボード操作、セマンティック HTML、ローカルストレージによる設定保持を確認します。
4. 問題がなければ **Source Control** パネルで変更をステージしてコミットします。次のレッスンで公開します。

## まとめと次のステップ

Copilot Chat Agent Mode の作業を監視し、各編集をレビューして受け入れまたは取り消し、追加プロンプトでライトモードを実装するよう方向付けしました。次は、[変更を pull request として公開してレビューします][next-lesson]。

## リソース

- [VS Code の Copilot Chat][copilot-chat-vscode]
- [エージェントモードの使用][agent-mode]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
