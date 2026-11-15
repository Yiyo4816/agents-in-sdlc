---
title: "练习 1 - 自定义说明（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解背景信息，就应确保它能获取这些上下文。[说明文件][instruction-files]可提供此类指导，让 Copilot 不仅了解要做什么，也了解如何完成任务。

本练习将学习如何：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义说明和按路径限定的说明文件提供给 Copilot。
- 向 `.github/copilot-instructions.md` 添加新的全局存储库标准。

> [!NOTE]
> 与 VS Code 和 CLI 工作环境不同，本练习不会比较提示执行前后的结果。Copilot 云智能体会在 GitHub 议题上异步工作，因此难以实时并排展示说明的影响。稍后检查云智能体生成的拉取请求时，你会看到说明文件的影响。

## 说明文件

### 场景

与优秀的开发团队一样，Tailspin Toys 制定了一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用暗色模式并具有现代感。
- 应通过 TSDoc 文档注释为代码添加说明。
- 每个文件开头都应添加一段注释，说明文件的用途。

使用说明文件可以确保 Copilot 获得正确的信息，并按照上述实践完成任务。

### 自定义说明

自定义说明可为 Copilot 提供上下文和偏好，帮助它更好地理解你的编码风格和要求。这项强大的功能可以引导 Copilot 提供更相关的建议和代码片段。你可以指定编码约定、库以及注释风格等。你可以为整个存储库创建说明，也可以针对特定类型的文件创建说明。

有两种说明文件：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个说明文件。它应包含与大多数 Copilot 聊天或 CLI 请求相关的项目级信息，例如所用技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建的说明文件。你可以为特定语言（如 TypeScript 或 Astro）提供指南，也可以为创建 UI 组件或一组单元测试等任务提供指导。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，都会使用存储库级说明文件和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot [还支持其他标准的说明文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md 文件。

### 管理说明文件的最佳实践

本研讨会不会全面介绍如何创建说明文件。不过，示例项目展示了具有代表性的方法。总体而言：

- 让 `copilot-instructions.md` 专注于项目级指导，例如构建内容的描述、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用自然语言，确保指导清晰，并提供代码应如何编写或不应如何编写的示例。

创建说明文件没有唯一方法，使用 AI 也是如此。通过实践，你会找到适合项目的方法。

> [!TIP]
> 所有使用 GitHub Copilot 的项目都应具备完善的说明文件。浏览此项目时，你可能会发现有适用于多种任务的文件，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成说明文件。不同产品界面提供的方式各不相同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。本系列中与你当前界面对应的课程会在相关处介绍这些方式。
>
> 想要模板或起点？请浏览 [awesome-copilot][awesome-copilot]，其中包含大量说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览此项目中的自定义说明文件

花些时间阅读此存储库提供的说明文件，包括一个核心 `copilot-instructions.md` 文件和一组用于不同任务的 `*.instructions.md` 文件。你可以在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。查看 **Code standards** 下的嵌套 **GitHub Actions Workflows** 指导。这些内容适用于你与 Copilot 进行的任何交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件。你会看到适用于 Astro 文件、Drizzle 数据层、测试等内容的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段；它设置了相对于存储库根目录的 glob 模式，决定说明适用哪些文件。例如，任何 TypeScript 测试文件（如匹配 `**/*.test.ts` 的文件）都会匹配此模式。
5. 留意此项目中用于创建单元测试的专门说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到文件底部。留意指向其他说明文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的说明集拆分成更小、可重复使用的文件，并提供可供 Copilot 参考的示例。（这些路径相对于说明文件，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目的编码标准，但尚未要求在代码中添加文档。接下来会添加有关 TSDoc 文档注释和文件注释头的规则。

## 添加新的存储库标准

接下来是本练习中唯一需要编辑的内容：添加一条项目级规则，要求在代码中使用 TSDoc 文档注释和文件级注释头。云智能体稍后处理本系列中的议题时会使用这项规则。

编辑前，先创建一个分支（云智能体会从议题目标分支读取说明，但仍应整洁地提交你的更改）：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加一条编码标准，改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，应该位于第 27 行附近。留意其中记录的项目编码标准；目前尚无代码内文档规则，因此生成的辅助函数没有文档注释。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 正如上一课所介绍的，可以在存储库级别（`.github/copilot-instructions.md`）创建全局说明，也可以针对特定语言、文件类型或任务使用 `*.instructions.md` 文件。存储库级文件适合放置刚添加的文档注释规则等项目级标准。

## 提交、推送并合并说明更新

云智能体会从议题目标分支读取说明。下一练习中，将议题分配给 Copilot 后，它会从 `main` 分支创建分支。因此，说明更改必须先合并到 `main`，云智能体才能使用。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求并合并。这样，云智能体在下一练习中处理议题时就会读取这些说明。

> [!TIP]
> 如果希望在本研讨会中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。此处保留分支步骤，是为了模拟真实项目中的工作方式。

## 总结和后续步骤

你已了解 Copilot 如何获取此项目的说明，并向 `.github/copilot-instructions.md` 添加了新的存储库级标准。后续练习中，云智能体生成的拉取请求会体现该标准。

接下来，将[第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：说明文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
