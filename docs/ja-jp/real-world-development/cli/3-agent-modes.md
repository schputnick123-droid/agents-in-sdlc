---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan でアプローチに合意し、Autopilot で Issue からフィルター機能を構築し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初に小さな機能を追加しましたが、大きな変更にはより堅牢なプロセスが必要です。GitHub Copilot CLI は組織の既存のフローに沿って作業できます。このレッスンから数回にわたり、Issue から新機能を作成し、コードと動作を検証して、プロジェクトにマージするエージェント主導の開発プロセスを実践します。

> [!NOTE]
> 機能の作業を続ける間、同じ会話とブランチを使います。通常はファイルの種類ごとに異なるブランチや PR を使いますが、ここでは中心となる概念に集中できるよう手順を簡略化します。

このレッスンでは、次の内容に取り組みます。

- GitHub Issue から新しい Copilot の会話を開始する。
- Plan モードで要件を定義する。
- Autopilot モードを使って新機能を実装する。
- コードをレビューする。
- 転送されたブラウザーで機能を手動検証する。

この機能の作業を続けながら、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズして、MCP 検証を追加し、QA エージェントを作成して、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、訪問者がカテゴリーとパブリッシャーでゲームを絞り込める必要があります。バックログの Issue には機能が記載されていますが、カテゴリーの組み合わせ方など、実装前に合意すべき詳細があります。Plan モードで決定し、Autopilot には範囲を限定して実装させます。

## 背景

AI コーディングエージェントを開発フローに導入しても基本は変わりません。むしろ、基本はさらに重要になります。多くの開発者は、次のような流れに従います。

1. 実施する作業の詳細が記載された Issue を開く。
2. 構築する内容の計画を作成する。
3. コードを作成してレビューする。
4. テストを実行してコードを検証する。
5. 新機能を手動で検証する。
6. pull request (PR) を作成する。
7. コードのレビューと継続的インテグレーションの処理が成功したら、コードをマージする。

チームや Organization によって詳細は異なりますが、上記の流れを変形したものが一般的です。この標準的なアプローチを守ることで、AI が生成したコードが要件を満たし、手作業で作成したコードと同じ審査プロセスを通るようにできます。

## 会話モード

**会話モード**はエージェントの自律性を制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> でモードを切り替えます。

- **Interactive**: エージェントが変更を提案し、続行前に入力を待ちます。
- **Plan**: エージェントが計画を作成しますが、プロジェクトファイルは編集しません。
- **Autopilot**: エージェントが自律的にコードを書き、テストを実行して、タスク完了まで反復します。

Plan モードで開始して計画をレビューし、その後 Autopilot で実装します。

## Issue から開始する

フィルター機能に取り組む前に、Codespace とリポジトリを準備します。

1. Codespace に戻ります。停止している場合は再起動します。
2. 星評価の PR がマージされていることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルター作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を2回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトに `#7` などの Issue 番号が表示されます。`#` を使うと、GitHub の Issue や pull request (PR) をコンテキストに追加できます。

## フィルター機能を計画する

実装やタスクの進め方を先に定義すると、複雑な作業のアプローチをレビューできます。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替え、プロンプトの下に **Plan** と表示されていることを確認します。
2. 直前の手順で追加した Issue の参照に続けて、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

3. フィルターの動作について質問されたら、希望する動作を回答します。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画を読み、内容が判断と異なる場合は Copilot に修正を依頼します。

## Autopilot を承認する

計画をレビューしてから実装を開始します。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みバージョンに表示される同等の選択肢を選びます。
2. モード表示に **Autopilot** とあることを確認します。
3. Copilot が計画に沿ってコードを生成し、テストを実行する様子を確認します。

> [!NOTE]
> 承認するとすぐに実装が始まる場合があるため、先に計画をレビューしてください。依存関係の不足やポート競合が報告された場合は、チェック完了と判断する前にセットアップの問題を解決します。

## 実装をレビューして検証する

生成したコードも、他のコードと同じようにマージ前にレビューします。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替え、**Plan** または **Autopilot** と表示されていないことを確認します。
2. `/diff` と入力し、フィルターの実装とテストを確認します。
3. 結果を Issue と計画時の決定に照らし合わせます。
4. レビュー後、<kbd>Esc</kbd> を押して差分画面を閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解消を依頼します。

## 新しい機能を確認する

Codespaces のポート転送を使ってブラウザーでサイトを確認します。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら **Open in Browser** を選択します。
3. 計画で合意したカテゴリーとパブリッシャーの各フィルターおよび組み合わせを試します。
4. リセット時と該当結果がない場合の動作が、Issue と決定事項に合っていることを確認します。
5. Codespace に戻り、Copilot が起動した開発サーバーを停止するよう依頼します。

## まとめと次のステップ

複数の会話モードを使って機能を構築し、レビューしました。このレッスンでは、次の作業を行いました。

- GitHub Issue から新しい Copilot の会話を開始した。
- Plan モードで要件を定義した。
- Autopilot モードを使って新機能を実装した。
- コードをレビューした。
- 転送されたブラウザーで機能を手動検証した。

次は、[カスタム指示を使って][next-lesson]、コードが文書化されたプラクティスに従う方法を確認します。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
