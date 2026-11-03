---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

コンテキストは生成 AI を扱ううえで重要です。タスクの進め方や Copilot が知っておくべき背景情報がある場合は、その情報を利用できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に何を実行するかだけでなく、どのように実行するかも伝えられます。

この演習では、次の作業を行います。

- リポジトリのカスタム指示とパス固有の指示ファイルが、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える仕組みを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスと異なり、この演習では指示の変更前後に同じプロンプトを実行しません。Copilot cloud agent は GitHub Issue に対して非同期で作業するため、影響をすぐに比較するのが難しいためです。後続の演習で cloud agent が作成する pull request をレビューするときに、指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には開発プラクティスのガイドラインがあります。データレイヤーには必ず単体テストを用意すること、UI をダークモードで現代的な印象にすること、コードに TSDoc コメントを追加すること、各ファイルの先頭に内容を説明するコメントを置くことなどです。指示ファイルを使って、これらのプラクティスに沿うために必要な情報を Copilot に提供します。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件を Copilot に伝えられます。リポジトリ全体、または特定のファイル種類やタスクに適用する指示を作成できます。

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**リクエストで Copilot に送信する単一の指示ファイルです。技術スタック、プロジェクトの概要、ベストプラクティスなど、全体に適用する情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種類、タスク向けのガイドラインを記載します。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成に使用されますが、コード補完や次の編集候補には使用されません。Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリレベルの指示と `applyTo` フロントマター付きの `*.instructions.md` ファイルを使用します。Copilot は AGENTS.md や CLAUDE.md など、ほかの形式の指示ファイルもサポートしています。

指示ファイルは、プロジェクト全体のガイダンスを `copilot-instructions.md` に置き、特定のファイル種類やタスクの指示を `*.instructions.md` に分けます。自然な言葉で明確に記述し、コードのよい例と避けるべき例を示します。どの構成が適切かは、実際に試して判断します。

> [!TIP]
> このプロジェクトには、UI 更新用の[指示ファイル][ui-instructions]や Astro 用の[指示ファイル][astro-instructions]など、さまざまなタスク向けの指示が含まれています。VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init` など、Copilot に指示ファイルを作成させる方法もあります。テンプレートを探す場合は、[awesome-copilot][awesome-copilot]を確認してください。

### このプロジェクトの指示ファイルを確認する

まず、スタータープロジェクトに含まれる指示を読み、Copilot の作業にどう影響するかを確認します。

1. Codespace で `.github/copilot-instructions.md` を開きます。
2. プロジェクトの説明、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などを確認します。**Code standards** の下には **GitHub Actions Workflows** のガイダンスもあります。これらは Copilot とのすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、Astro、Drizzle データレイヤー、テストなどの指示ファイルを確認します。
4. `.github/instructions/unit-tests.instructions.md` を開き、`applyTo` フィールドがリポジトリのルートからの glob によって適用対象を決めることを確認します。たとえば `**/*.test.ts` に一致する TypeScript テストファイルが対象です。
5. `.github/instructions/drizzle.instructions.md` の末尾にある、ほかの指示ファイルやプロジェクト内ファイルへのリンクを確認します。これらを使うと、指示を小さく再利用可能なファイルに分け、Copilot に参照例を示せます。リンク先のパスは指示ファイルの場所を基準とします。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** にはコーディング標準が記載されていますが、コード内のドキュメントに関するルールはまだありません。次に TSDoc コメントとファイルヘッダーのルールを追加します。

## リポジトリ標準を追加する

このハーネスで編集するのは、コード内のドキュメントとファイル先頭のコメントを標準に加える作業です。Issue に割り当てた cloud agent が指示を読み込めるよう、変更を `main` にマージします。

1. Codespace のターミナルで新しいブランチを作成します。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. `.github/copilot-instructions.md` を開き、**Code formatting requirements** セクションを探します。
3. 既存の標準の直後に、次の項目を追加して保存します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

このファイルはプロジェクト全体に適用されるため、リポジトリレベルの標準を置く場所として適しています。

## 指示の変更をコミット、プッシュしてマージする

Cloud agent は、Issue の対象ブランチにある指示ファイルを読み込みます。次の演習では `main` からブランチが作成されるため、追加した指示を `main` に取り込んでおきます。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` への pull request を作成してマージします。これにより、次の演習で Issue を割り当てたときに cloud agent がこの指示を使用します。

> [!TIP]
> このワークショップでは `main` に直接コミットしてもかまいません。ブランチを使う手順は、実際のプロジェクトと同様の流れを体験するためのものです。

## まとめと次のステップ

プロジェクトの指示ファイルが Copilot にコンテキストを伝える仕組みを確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。後続の演習で cloud agent が作成する pull request に、この標準が反映されます。

次は、[Copilot cloud agent に Issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を書くための5つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot - 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
