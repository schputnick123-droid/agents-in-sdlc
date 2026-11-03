---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2つの pull request、再利用できるカスタマイズ、追加資料を振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使って、Tailspin Toys の一連の作業に取り組みました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトと用意されたフィルター機能の Issue を確認しました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初の pull request (PR) を手動でマージしました。
- フィルター機能の Issue から開始し、Plan モードでアプローチを決め、Autopilot モードで構築して、Interactive モードでレビューしました。
- カスタム指示でエージェントを案内し、`quality-checks` スキルをカスタマイズして単体テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を確認しました。
- 品質保証 (QA) カスタムエージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーでの証拠を評価しました。
- フィルター機能の変更全体をレビューし、Agent Merge を使って PR を処理しました。
- コンテキスト、モデル、共有、任意の cloud agent への委任に使えるスラッシュコマンドを確認しました。

## リリースしたもの

ワークショップでは、更新済みの `main` からそれぞれブランチを作り、2つの PR を作成します。

1. **星評価:** ゲームカードに既存の `starRating` と未評価状態を表示します。
2. **フィルター機能と品質ワークフロー:** フィルター機能を実装し、指示を更新して機能に適用し、`quality-checks` レポートをカスタマイズして、QA プロファイルと関連テストを含めます。

フィルター機能の計画から PR 作成まで、同じ会話とブランチを使いました。ワークショップを簡潔にするため、これらの作業を1つの PR にまとめました。

## 検証方法の違い

自動テスト、手動でのブラウザー確認、MCP を通じた Copilot のブラウザー操作でコードを確認しました。`quality-checks` スキルは単体テスト、lint、型チェックを実行し、新しい形式で結果を報告します。QA エージェントは PR の前に、結果と要件およびテストカバレッジのレビューをまとめます。

追加するテストは実際の不足を補うものにします。QA の実行で追加テストが不要と判断される場合もあります。コードと証拠をレビューしてからマージを許可し、変更後は関連する証拠を更新してください。

## ベストプラクティス

Copilot に与えるコンテキストとツールが作業の結果に影響します。このワークショップでは、指示、スキル、QA プロファイル、MCP サーバーを設定しました。セッション間でカスタマイズを再利用し、チームのニーズに合わせて調整してください。指示は標準を設定し、スキルは反復可能な作業を説明し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。構築前にアプローチを検討するには **Plan**、対象を絞った変更に関与し続けるには **Interactive**、範囲の明確な作業には **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業にはより高い能力を持つモデルを選びます。

何を、なぜ、どのように作るかを明確に伝えることも重要です。アイデアを本格的なセッションに進める前に、Quick chats を使って範囲を検討できます。

## さらに確認する機能

- `/review` でコードレビューエージェントに変更を分析させます。
- `/rubber-duck` で問題を相談し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送信します。

## 次のステップ

ツールを使いこなすには、使い続けるのが一番です。実稼働コード、趣味のコード、作りたいと思いながら手を付けられていなかったアプリに活用し、学んだことをチームと共有してください。

ターミナルから Tailspin Toys をさらに拡張するには、[Foundry Backer Concierge シリーズ][foundry]に進みます。別の環境も確認する場合は、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]を参照してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI のカスタマイズ][customize-cli]
- [Copilot CLI で pull request を管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
