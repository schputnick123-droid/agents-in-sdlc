---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキストの管理、モデルの選択、セッション共有、任意のクラウド委任に使うスラッシュコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

GitHub Copilot CLI には、CLI と対話するためのスラッシュコマンドが多数あります。これまで `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` を使いました。このレッスンでは、次の内容に取り組みます。

- `/context` と `/compact` を使い、Copilot が会話のコンテキストを管理する方法を確認する。
- `/model` を使い、利用可能なモデルを確認する。
- `/share` でセッションをエクスポートまたは共有する方法を学ぶ。
- 並列作業、worktree、cloud agent への委任に使う任意のコマンドを確認する。

## シナリオ

CLI の中心的なワークフローを終えたので、いくつかの機能を試します。コンテキストを管理し、モデルを切り替えてセッションを共有し、必要に応じて [Copilot cloud agent][about-cloud-agent] に作業を委任します。

## Copilot CLI のコンテキストを確認する

複雑なタスクではモデルのコンテキストウィンドウが上限に近づくことがあります。Copilot CLI は必要に応じて会話を自動的に圧縮しますが、コマンドを使って確認や圧縮もできます。

1. Codespace に戻ります。起動していない場合は、リポジトリのルートから Copilot CLI を起動します。
2. 次のコマンドを入力します。

   ```text
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域へのコンテキストの割り当てを確認します。
4. 会話を圧縮します。

   ```text
   /compact
   ```

5. もう一度 `/context` を入力し、結果を比較します。会話が短い場合は、大きな違いがないこともあります。

> [!NOTE]
> コンテキストウィンドウが埋まると、Copilot CLI は自動的に圧縮します。自分でタイミングを選ぶ場合は `/compact` を使います。無関係なタスクに移るときに新しい会話を始めるには `/clear` または `/new` を使います。

## モデルを選ぶ

モデルごとに得意分野が異なります。Copilot CLI では、利用可能なモデルを一覧表示して選択できます。

1. `/model` と入力します。
2. 利用可能なモデルと使用状況を確認します。
3. 現在のモデルを維持するか、別のモデルを選択するか、<kbd>Esc</kbd> で一覧を閉じます。

## セッションを共有する

`/share` は、会話を Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist として公開したりできます。

1. `/help` を入力し、インストール済みバージョンの `/share` オプションを確認します。
2. 共有する場合は、用途に合った出力先を選びます。たとえば `/share file` はローカルの Markdown ファイルにエクスポートします。
3. 送信または公開する前に内容を確認します。エクスポートにはプロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有を意図していないリポジトリや会話の内容は公開しないでください。

## 任意: 作業を拡張または委任する

次のコマンドは任意です。新しい、範囲の明確なタスクで試し、結果は通常のワークフローでレビューしてください。

- `/fleet` は独立したサブタスクをサブエージェントに分割し、並列実行できます。
- `/worktree` は別のタスク用に分離された Git worktree を作成できます。
- `/delegate` は Copilot cloud agent にタスクを送り、非同期で作業させます。PR が作成される場合があります。

これらの操作によって追加の worktree やリモート作業が発生することがあります。非同期エージェントについて詳しく知りたい場合は、[cloud agent ワークショップ][cloud-workshop]を確認してください。

## まとめと次のステップ

スラッシュコマンドを使って Copilot CLI を設定し、セッションを共有し、内部の状況を確認しました。このレッスンでは、次の作業を行いました。

- `/context` と `/compact` を使い、Copilot が会話のコンテキストを管理する方法を確認した。
- `/model` を使い、利用可能なモデルを確認した。
- `/share` でセッションをエクスポートまたは共有する方法を学んだ。
- 並列作業、worktree、cloud agent への委任に使う任意のコマンドを確認した。

利用できるスラッシュコマンドはほかにもあります。次は、[ここまでの作業を振り返り、今後の学習方法を確認します][next-lesson]。

## リソース

- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
