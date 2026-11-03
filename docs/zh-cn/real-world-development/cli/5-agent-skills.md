---
title: "第 5 课 - 使用 quality-checks 技能"
description: "查看现有的 quality-checks 技能、自定义其报告格式，并使用它验证筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

编写代码并不只是编写代码。我们已手动验证代码能够运行，并使用指令文件确保其遵循标准。但测试、lint 和其他持续集成 (CI) 环节怎么办？

对于这类任务，**智能体技能**最为合适！技能可以帮助 Copilot 了解如何正确执行这些操作。

本课将：

- 探索现有的 `quality-checks` 技能。
- 自定义其结果格式。
- 重新加载并运行技能。

## 场景

Tailspin Toys 使用 `quality-checks` 技能运行单元测试、lint 和类型检查。团队希望改进报告格式，让结果更容易阅读。

## 指令、脚本和资源

智能体技能将可复用的任务指令、可执行脚本和辅助资源打包在一起，由智能体按需加载。技能的核心是一个与技能同名的文件夹，其中包含 Markdown 文件 `SKILL.md`。Markdown 文件包含用于定义技能名称和说明的 frontmatter，以及技能用途概览和调用时机的指导。技能文件夹还可以包含脚本和其他资源子目录。

> [!NOTE]
> 技能不一定需要额外的文件或文件夹。Tailspin Toys 的 `quality-checks` 技能仅包含 `SKILL.md`，因为它使用了项目已有的命令。

技能可以放在项目的 `.github/skills` 文件夹中，作为存储库资源供团队共享和复用；也可以放在用户技能文件夹 `~/.copilot/skills` 中。

## 探索技能

来看看 Tailspin Toys 团队创建的 `quality-checks` 技能，它用于运行单元测试、lint 和类型检查。

1. 返回 Codespace。在 Codespaces 编辑器中打开 `.github/skills/quality-checks/SKILL.md`。
2. 阅读顶部的 `name` 和 `description`。说明内容可帮助 Copilot 判断何时调用此技能。
3. 阅读指令，了解其如何引导 Copilot 执行测试和 lint。
4. 注意技能目前尚无 **Results output formatting** 章节。

## 修改前先运行技能

可以通过 Copilot CLI 命令或自然语言直接调用技能。现在请 Copilot 运行该技能中的三项检查。

1. 返回筛选功能的 Interactive 对话。
2. 使用以下提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 留意最后生成的报告。

## 自定义报告

我们希望报告能说明运行了什么、是否成功，以及工具报告了哪些内容。现在更新技能，生成这种报告。

1. 返回 `.github/skills/quality-checks/SKILL.md`。
2. 在文件末尾添加以下章节：

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. 文件会自动保存。

## 运行更新后的技能

更新完成后，看看实际效果！Copilot CLI 无需重启对话即可重新加载已编辑的技能。

1. 输入：

   ```plaintext
   /skills reload
   ```

2. 使用与之前完全相同的提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看最终报告，并与首次报告进行比较。

## 总结与后续步骤

你已自定义并使用现有的智能体技能。本课中，你：

- 探索了用于单元测试、lint 和类型检查的 `quality-checks` 技能。
- 自定义了技能的结果格式。
- 重新加载并运行了技能。

此更改会与筛选功能一起包含在功能 PR 中。接下来让 Copilot 直接操作网站，[使用 Playwright MCP 服务器][next-lesson]。

## 更多技能示例

以下社区示例仅供参考，并非额外任务：

- [Agent Skills 规范][skill-spec]
- [贡献工作流：`make-repo-contribution`][contribution-example]
- [需求文档：`prd`][prd-example]
- [图表和随附的导出脚本：`drawio`][drawio-example]
- [浏览器测试：`webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
