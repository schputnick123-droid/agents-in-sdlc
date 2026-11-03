---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 应该了解背景信息，就需要确保它能获取这些上下文。[指令文件][instruction-files]可提供相关指导，让 Copilot 不仅了解要做什么，也了解应如何完成。

本练习将：

- 了解如何通过存储库自定义指令和按路径限定范围的指令文件，为 Copilot 提供项目上下文、编码规范和文档标准；
- 在 `.github/copilot-instructions.md` 中添加一条新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 学习路径不同，本学习路径不使用“前后对比”提示。Copilot 云智能体通过 GitHub 议题异步工作，因此较难实时并排展示指令的效果。在之后审查云智能体生成的拉取请求时，你会看到指令文件产生的影响。

## 指令文件

### 场景

和其他优秀的开发团队一样，Tailspin Toys 也有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用暗色模式并呈现现代风格。
- 应通过 TSDoc 文档注释为代码添加说明。
- 每个文件开头都应添加一段注释，说明文件的用途。

通过使用指令文件，你可以确保 Copilot 了解这些实践要求，并据此完成任务。

### 自定义指令

自定义指令可为 Copilot 提供上下文和偏好，帮助它更好地理解编码风格和要求。你可以通过指令引导 Copilot 提供更相关的建议和代码片段，例如指定编码约定、库，以及偏好的注释形式。指令既可以应用于整个存储库，也可以针对特定文件类型，为具体任务提供上下文。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库向 Copilot 提出请求时，都会发送此单一指令文件。内容应面向项目整体，涵盖大多数聊天或 CLI 请求都适用的信息，例如技术栈、项目概况、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对具体任务或文件类型创建，例如 TypeScript、Astro 文件，或创建 UI 组件和单元测试等任务。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 生成代码，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，都会使用存储库级指令文件和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还支持其他标准的指令文件，例如 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

完整介绍如何创建指令文件超出了本研讨会的范围。不过，示例项目展示了一种有代表性的做法。总体而言：

- 将 `copilot-instructions.md` 限定为项目级指导，例如项目概况、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件，为文件类型（例如单元测试、Astro 组件和数据层）或特定任务提供指导。
- 使用自然语言，让指导清晰易懂，并提供正反示例。

创建指令文件没有唯一的方法，使用 AI 也是如此。通过试验，你会逐渐找到适合项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应配备一组完善的指令文件。查看本项目中的指令文件时，你会发现其中包含多种任务的指导，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成指令文件。不同产品的操作有所不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。对应产品的课程会在相关处说明。
>
> 需要模板或起点？可以查看 [awesome-copilot][awesome-copilot]，其中收集了指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看本项目的自定义指令文件

花些时间阅读此存储库自带的指令文件：其中有一个核心 `copilot-instructions.md` 文件，以及一组针对不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，注意其中对项目的简要介绍，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等章节。在 **Code standards** 下，留意嵌套的 **GitHub Actions Workflows** 指导。这些内容适用于你与 Copilot 进行的各种交互。
3. 打开 `.github/instructions` 文件夹并查看其中内容。注意这里包含 Astro 文件、Drizzle 数据层、测试等方面的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意文件顶部的 `applyTo` 字段，它用于设置相对于存储库根目录的 glob 模式，决定哪些文件适用此指令。这里，任何匹配 `**/*.test.ts` 的 TypeScript 测试文件都会应用该指令。
5. 留意专门用于创建本项目单元测试的指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。注意其中指向其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成较小的可复用文件，并为 Copilot 指明生成代码时可参考的示例。（其中的路径相对于指令文件，而非存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 章节记录了项目编码标准，但目前尚未要求在代码中添加说明。接下来，你将添加 TSDoc 文档注释和文件注释头的规则。

## 添加新的存储库标准

接下来要做的一项编辑，是添加一条项目级规则，要求在代码中使用 TSDoc 文档注释并添加文件注释头。稍后的练习中，云智能体处理分配给它的议题时会遵循这项规则。

编辑前，先创建一个分支（云智能体会从议题目标分支读取指令，但你仍应整洁地提交更改）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加编码标准，以便改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 章节，该章节应位于第 27 行附近。注意其中记录了项目编码标准，但尚未规定添加代码文档，因此生成的辅助函数没有文档注释。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，指令文件既可以放在存储库级的 `.github/copilot-instructions.md` 中，提供全局指导，也可以放在 `*.instructions.md` 文件中，针对特定语言、文件类型或任务提供指导。存储库级文件适合放置 TSDoc 文档注释规则等项目标准。

## 提交、推送并合并指令更新

云智能体会从议题目标分支读取指令。下一练习中，你会将议题分配给 Copilot，而它会从 `main` 分支创建分支。因此，必须将指令更改合并到 `main`，云智能体才能获取这些更改。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上打开从 `custom-instructions-cloud` 合并到 `main` 的拉取请求，并将其合并。下一练习中，云智能体处理议题时就会读取这些指令。

> [!TIP]
> 如果希望直接在 `main` 分支上完成本研讨会，可以跳过创建分支的步骤，并直接将更改提交到 `main`。此处创建分支是为了模拟真实项目中的工作方式。

## 总结与后续步骤

你已了解 Copilot 如何使用本项目的指令文件获取上下文，并在 `.github/copilot-instructions.md` 中添加了一条新的存储库级标准。在本学习路径后续练习中，你将看到云智能体生成的拉取请求如何遵循这一标准。

接下来[将第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
