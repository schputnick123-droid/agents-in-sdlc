---
title: "第 4 课 - 使用自定义指令引导 Copilot"
description: "查看存储库指令，添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，就需要确保 Copilot 能获取相应指导。[指令文件][instruction-files]不仅说明你希望编写什么代码，也说明代码应如何组织。现在你已构建筛选功能，接下来将查看 Copilot 使用的指令、添加一项文档标准，并将其应用于代码。

本课将：

- 了解存储库指令和按路径限定范围的指令文件如何传递给智能体。
- 更新指令文件，确保遵循编码标准。
- 了解指令文件对代码的影响。

## 场景

和其他优秀的开发团队一样，Tailspin Toys 也有一套开发实践指南和要求，包括：

- 注释应解释目的和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中的导出函数应使用 TSDoc/JSDoc 说明其用途、参数和返回值（如有可注入的 `db` 参数也应说明）。
- 可复用的 Astro 组件应说明其 `Props` 接口；相关代码更改时，注释也应保持更新。
- 现有格式和 lint 指导必须保留。

通过使用指令文件，你可以确保 Copilot 了解这些实践要求，并据此完成任务。

## 指令文件

自定义指令可为 Copilot 提供上下文和偏好，帮助它更好地理解编码风格和要求。你可以通过指令引导 Copilot 提供更相关的建议和代码片段，例如指定编码约定、库，以及偏好的注释形式。指令既可以应用于整个存储库，也可以针对特定文件类型，为具体任务提供上下文。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库向 Copilot 提出请求时，都会发送此单一指令文件。内容应面向大多数请求都适用的项目级指导。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指导的文件。

> [!NOTE]
> 其他指令格式及其支持情况会因环境而异。依赖特定格式前，请查阅[自定义指令支持参考][custom-instructions-support]。

## 查看本项目的自定义指令文件

入门项目已包含一组指令文件。修改文件前先了解当前内容，之后便能观察更改的效果。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而非终端）中打开 `.github/copilot-instructions.md`。
3. 浏览文件，注意其中的项目简介和编码指导。此文件适用于在此存储库中与 Copilot 的所有交互。
4. 打开 `.github/instructions` 文件夹并查看其中内容。注意这里包含 Astro 文件、Drizzle 数据层、测试等方面的指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它用于设置决定适用文件范围的 glob 模式。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它指向其他指令文件和项目现有文件的链接。这样可以将较大的指令集拆分成较小的可复用文件，并为 Copilot 指明可参考的示例。

## 更新指令文件，符合团队要求

尽管现有文件已是很好的起点，但仍缺少一项标准。修改核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 章节，应该位于第 35 行附近。
2. 在章节最后添加以下要点：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存！

## 使用更新后的指导

Copilot CLI 会在对话开始时加载存储库指令。保存更新后的指令后，退出并恢复筛选会话，使 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍处于打开状态，输入 `/exit` 关闭。Copilot CLI 会自动保存对话。
2. 在 `game-filters-cli` 分支的存储库根目录运行以下命令，恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选对话，并确认当前检出的分支为 `game-filters-cli`。
3. 请 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff` 并审查已更改的 TypeScript 文件。留意新生成的 TSDoc 注释，并确认其准确说明了代码。

## 总结与后续步骤

你已了解 Copilot CLI 如何从指令文件获取上下文，并将新的标准应用于功能代码。具体来说，你：

- 了解存储库指令和按路径限定范围的指令文件如何传递给智能体。
- 更新指令文件，确保遵循编码标准。
- 观察指令文件对代码的影响。

接下来[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保以一致的方式运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot — 指令文件及其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
