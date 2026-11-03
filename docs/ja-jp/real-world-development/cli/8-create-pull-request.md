---
title: "レッスン 8 - 機能 PR の作成とマージ"
description: "フィルター機能、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルター機能の実装、指示とスキルの更新、品質保証 (QA) プロファイル、テストは1つのブランチに保存されています。これらをまとめてレビューし、pull request を作成します。星評価の PR は自分でマージしましたが、今回は **Agent Merge** にプロセスを任せます。

このレッスンでは、次の内容に取り組みます。

- Agent Merge の概要と、マージのライフサイクルを自動化する仕組みを学ぶ。
- 機能全体と検証の証拠を確認する。
- フィルター機能の PR を作成する。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認する。

## シナリオ

フィルター機能の作業では、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys は、マージの承認を開発者が管理しながら、PR の残りの作業を自動化したいと考えています。

## Agent Merge について

**Agent Merge** は、PR をマージするまでに必要な作業を自動化します。有効にすると、Copilot は失敗した継続的インテグレーション (CI) チェックの修正、レビューコメントへの対応、必要に応じたリベースなど、PR の阻害要因に対処し、リポジトリで許可されていれば GitHub の自動マージを有効にします。

これまで **Merge pull request** は自分で選択していました。Agent Merge に任せる場合も、許可する前に作業をレビューし、準備ができていることを確認します。

## Agent Merge で PR を管理する

コードの作成とレビューが終わったら、PR を作成して Agent Merge に残りを任せます。

1. Codespace に戻ります。
2. `/agent` と入力してエージェントダイアログを開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` で Agent Merge を有効にします。
6. Copilot が PR の CI を監視し、すべて成功した後にマージすることを確認します。
7. Copilot が `PR #14 was squash-merged successfully.` などのメッセージを表示し、PR の状態が **Merged** になったことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリ設定、権限の不足を回避しません。ブロックされた場合は理由を確認し、リポジトリで許可される手順に従って手動でマージしてください。

## まとめと次のステップ

コードの生成、テストと検証、pull request の処理を自動化しました。このレッスンでは、次の作業を行いました。

- Agent Merge の概要と、マージのライフサイクルを自動化する仕組みを学んだ。
- 機能全体と検証の証拠を確認した。
- フィルター機能の PR を作成した。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認した。

次は、コンテキスト、モデル、共有、任意のクラウド委任に使える[Copilot CLI のスラッシュコマンドを確認します][next-lesson]。

## リソース

- [Copilot CLI で pull request を管理する][manage-prs]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
