---
title: "第 4 课 - 使用自定义说明引导 Copilot"
description: "了解存储库说明，添加文档标准，并将其应用于筛选功能代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，应确保 Copilot 能获取相应指导。[说明文件][instruction-files]不仅描述想要什么代码，也描述代码应如何组织。现在你已经构建了筛选功能，可以了解 Copilot 使用的说明、添加文档标准，并将标准应用到代码中。

本课将学习如何：

- 了解存储库说明和按路径限定的说明文件如何提供给智能体。
- 更新说明文件，确保遵循编码标准。
- 了解说明文件对代码的影响。

## 场景

Tailspin Toys 与优秀的开发团队一样，制定了一套开发实践指南和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明其用途、参数和返回值（如有可注入的 `db` 参数，也应予以说明）。
- 可重用的 Astro 组件应说明其 `Props` 接口；相关代码更改时，注释也应保持更新。
- 应保留现有格式和 lint 指南。

通过说明文件，可以确保 Copilot 获得正确的信息，并按照上述实践完成任务。

## 说明文件

自定义说明可为 Copilot 提供上下文和偏好，帮助它更好地理解你的编码风格和要求。你可以指定编码约定、库以及注释风格等，从而获得更相关的建议和代码片段。你可以为整个存储库创建说明，也可以针对特定类型的文件创建说明。

有两种说明文件：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个说明文件。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指导的说明文件。

> [!NOTE]
> 其他说明格式及其支持情况会因环境而异。依赖特定格式前，请查看[自定义说明支持参考][custom-instructions-support]。

## 浏览此项目中的自定义说明文件

入门项目已包含一组说明文件。先查看现有内容，再进行更改，以了解更改的影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而不是终端）中打开 `.github/copilot-instructions.md`。
3. 浏览文件，了解项目简介和编码指导。这些说明适用于 Copilot 在此存储库中的每次交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。你会看到 Astro 文件、Drizzle 数据层、测试等方面的说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段；它设置了说明适用文件的 glob 模式。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它指向其他说明文件和现有项目文件的链接。通过这种方式，可以将较大的说明集拆分成更小、可重复使用的文件，并提供可参考的示例。

## 更新说明文件以符合团队指南

现有文件已经很有帮助，但还需要补充一项指导。修改核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，该部分应位于第 35 行附近。
2. 在最后一条项目符号后添加以下内容：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存！

## 使用更新后的指导

Copilot CLI 会在对话开始时加载存储库说明。保存更新后的说明后，退出并恢复筛选对话，这样 Copilot 就能在保留功能上下文的同时使用新指导。

1. 如果筛选对话仍处于打开状态，请输入 `/exit` 将其关闭。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支中，恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选对话，并确认检出的是 `game-filters-cli` 分支。
3. 让 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff` 并阅读更改的 TypeScript 文件。留意新生成的 TSDoc 注释，并确认它们准确说明了代码。

## 总结和后续步骤

你已了解 Copilot CLI 如何获取说明，并将新标准应用到功能中。具体来说，你：

- 了解了存储库说明和按路径限定的说明文件如何提供给智能体。
- 更新了说明文件，确保遵循编码标准。
- 了解了说明文件对代码的影响。

下一课将介绍如何[自定义并运行可重复使用的 quality-checks 技能][next-lesson]，确保 lint 和测试能够一致地运行。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [创建有效自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot：说明文件和其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
