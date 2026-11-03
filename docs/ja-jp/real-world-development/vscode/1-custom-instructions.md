---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を扱うとき、タスクの進め方や Copilot が知っておくべき背景情報を利用できるようにすることが重要です。[指示ファイル][instruction-files]を使うと、コード生成の指針を Copilot に伝えられます。

この演習では、リポジトリとパス固有の指示を確認し、既存の指示で出版社取得ヘルパーを生成してから `.github/copilot-instructions.md` に標準を追加し、同じプロンプトを再実行して結果を比較します。最後に指示の更新とフィルター用ヘルパーの変更を `main` に取り込みます。

> [!CAUTION]
> Copilot の生成結果は確率的なため、指定した標準から外れる場合があります。コードを一字一句同じにすることではなく、指示の更新による傾向の変化を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys の開発プラクティスには、データレイヤーの単体テスト、現代的なダークモードのユーザーインターフェイス (UI)、TypeScript ドキュメントコメント (TSDoc)、各ファイルの内容を説明する先頭コメントが含まれます。指示ファイルでこれらの要件を Copilot に伝えます。

### カスタム指示

`.github/copilot-instructions.md` はリポジトリへのすべてのリクエストに適用されるプロジェクト全体の指示です。`.github/instructions/*.instructions.md` は、特定の言語、ファイル種類、タスクのガイダンスを定義します。

> [!NOTE]
> IDE で指示ファイルが使われるのは Copilot Chat のコード生成であり、コード補完や次の編集候補ではありません。Copilot Chat、Copilot CLI、Copilot cloud agent は `applyTo` フロントマター付きのリポジトリ指示と `*.instructions.md` をコード生成に使います。AGENTS.md や CLAUDE.md など、別の形式もサポートされています。

指示ファイルは、プロジェクト全体のガイダンスを `copilot-instructions.md` に、ファイル種類やタスク固有のガイダンスを `*.instructions.md` に記載します。明確な自然言語とコード例を使います。各環境には指示ファイル生成機能もあります。たとえば VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init` を利用できます。テンプレートは [awesome-copilot][awesome-copilot]で確認できます。

### このプロジェクトの指示ファイルを確認する

1. Codespace で `.github/copilot-instructions.md` を開き、プロジェクトの概要、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** を確認します。**Code standards** には **GitHub Actions Workflows** のガイダンスもあります。
2. `.github/instructions` フォルダーを開き、Astro、Drizzle データレイヤー、テストなどの指示を確認します。
3. `.github/instructions/unit-tests.instructions.md` を開き、`applyTo` がリポジトリルートを基準とする glob で適用対象を決めることを確認します。たとえば `**/*.test.ts` に一致する TypeScript テストファイルが対象です。
4. 単体テストの指示と `.github/instructions/drizzle.instructions.md` の末尾にある他の指示ファイルや既存ファイルへのリンクを確認します。参照パスは指示ファイルの場所を基準とします。

> [!NOTE]
> **Code formatting requirements** にはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ求められていません。次に TSDoc コメントとファイルヘッダーの標準を追加します。

## 変更用ブランチを作成する

1. 前の演習の Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) でターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

まず、現在の指示を使ってプロンプトを実行し、後で指示を更新した後の結果と比較します。

1. Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。
2. 前の演習で開いたエディタータブを閉じ、`src/lib/publishers.ts` を開いてヘルパーの保存先を示します。
3. Chat ビューのエージェント選択から **Agent** を選択します。
4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを確認して変更を適用する様子を見ます。ヘルパーとテストが更新される場合があります。
6. `.github/instructions/drizzle.instructions.md` のデータレイヤーの規約により、提案されたヘルパーが型付きの関数で `db` クライアントを最初の引数に取り、型付き配列を返すことを確認します。
7. 生成されたコードに TSDoc コメントとファイル先頭コメントがないことを確認します。

> [!CAUTION]
> 指示を変更する前でも Copilot がコメントを追加する場合があります。その場合も問題ありません。更新後に一貫性が高まることを確認するのが目的です。

## リポジトリ標準を追加する

1. `.github/copilot-instructions.md` を開きます。
2. **Code formatting requirements** セクション (27行目前後) を探します。
3. 既存の標準の直後に次の行を追加して保存します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

## プロンプトを再実行して違いを確認する

1. Copilot Chat に戻り、**New Chat** を選択します。
2. `src/lib/publishers.ts` を開き、Copilot のコンテキストをそのファイルに合わせます。
3. 先ほどと同じプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. ファイル先頭に内容を説明するコメントがあり、関数に目的、パラメーター、戻り値を説明する TSDoc コメントが追加されていることを確認します。

## 指示をコミットしてブランチをプッシュする

1. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) でターミナルを開きます。
2. `git status` を実行し、指示の変更とヘルパーの変更を確認します。
3. 指示ファイルと Copilot がヘルパーの基盤として変更したファイルをステージしてコミットします。`git status` にテストなどの追加変更があれば、それらも追加します。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## pull request を作成してマージする

この演習では Copilot に GitHub 上の操作を依頼します。テンプレートには GitHub のツールが構成済みです。初回の利用時に VS Code からサインインを求められた場合は、画面の指示に従います。

1. Codespace の Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Chat ビューを開き、エージェント選択で **Agent** を選択します。
3. 次のプロンプトを送り、関連 Issue を探して現在のブランチから PR を作成するよう依頼します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. GitHub の操作を許可するよう求められたら、内容を確認してから **Allow** を選択します。
5. PR が作成されたら、次のプロンプトでマージしてローカルの `main` を更新するよう依頼します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. GitHub の操作やシェルコマンドを許可する際は、内容を確認してから承認します。

## まとめと次のステップ

既存の指示と更新後の指示で Copilot の提案を比較し、指示の変更とヘルパーの基盤を `main` に取り込みました。次は、[エージェントモードでフィルター機能を実装します][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示作成のベストプラクティス][instructions-best-practices]
- [カスタム指示を書くための5つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot - 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
