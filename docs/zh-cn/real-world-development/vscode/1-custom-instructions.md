---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解背景信息，就应确保它能获取这些上下文。可以通过多种方式向 Copilot 提供特定上下文，其中一种主要方式是使用[说明文件][instruction-files]为代码生成提供指导。

本练习将学习如何：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义说明和按路径限定的说明文件提供给 Copilot。
- 在当前说明文件生效时生成筛选功能的第一个数据切片（发行方辅助函数）。
- 向 `.github/copilot-instructions.md` 添加新的全局存储库标准。
- 再次运行相同提示，观察生成的代码如何采用新标准。
- 将说明更新和筛选功能切片提交到 `main`，以便 Copilot 在下一练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不完全遵循你设置的标准。Copilot 具有非确定性。本练习的重点是观察更新说明后行为的*趋势变化*，而不是要求输出逐字一致。

## 说明文件

### 场景

与优秀的开发团队一样，Tailspin Toys 制定了一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用暗色模式并具有现代感。
- 应通过 TSDoc 文档注释为代码添加说明。
- 每个文件开头都应添加一段注释，说明文件的用途。

使用说明文件可以确保 Copilot 获得正确的信息，并按照上述实践完成任务。

### 自定义说明

自定义说明可为 Copilot 提供上下文和偏好，帮助它更好地理解你的编码风格和要求。你可以指定编码约定、库以及注释风格等，从而获得更相关的建议和代码片段。你可以为整个存储库创建说明，也可以针对特定类型的文件创建说明。

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

## 为更改创建分支

按照最佳实践，为更改创建一个分支。

1. 返回上一练习中使用的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个干净的对话。不同练习使用的模式和模型可能不同；需要时会在相应步骤中说明。

先在当前说明文件生效时发送提示。稍后更新说明，再次发送相同提示并比较结果，以观察自定义说明的影响。

1. 关闭之前练习中打开的编辑器选项卡，这样 Copilot 只会获取你需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，让 Copilot 应用文件更改。

   ![显示 Chat 视图中智能体选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和测试。
6. 留意建议的辅助函数是一个带类型的函数，它将 `db` 客户端作为第一个参数，并返回带类型的发行方数组。这来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层约定。
7. 留意建议的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有概率性，因此它可能在未被要求时也添加文档注释。如果发生这种情况也没关系；本练习的重点仍是观察更新说明后行为的一致性改进。

## 添加新的存储库标准

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

## 再次运行提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat**，清空上下文。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦于正确的文件。
3. 发送与之前**相同的提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 留意建议文件现在以类似以下注释块开头：

   ```typescript
   /**
    * Tailspin Toys 众筹平台的发行方数据访问辅助函数。
    * 提供用于从数据库获取发行方信息的函数。
    */
   ```

5. 留意建议的函数现在包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * 返回所有发行方的 ID 和名称列表。
    *
    * @param db - Drizzle 数据库客户端。
    * @returns 解析为发行方对象数组的 Promise。
    */
   ```

你刚刚引导 Copilot 遵循新的项目标准，并将其应用到真实代码中；下一练习会基于这些代码继续工作。

## 提交说明并推送分支

说明文件与存储库中的其他资源一样，需要通过源代码管理进行维护。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
2. 在终端中运行以下命令，确认说明更新和辅助函数更改都已应用：

   ```bash
   git status
   ```

3. 在终端中暂存并提交说明更新及 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示还有其他相关更新（例如测试），也要添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支已推送，现在创建拉取请求并将其关联到待办事项中的文档标准议题。也可以手动操作，不过 Copilot 可以使用项目中已连接的 GitHub 工具代为完成。让 Copilot 查找议题、创建用于关闭议题的 PR，然后合并 PR。

> [!NOTE]
> 这是第一次让 Copilot 代表你在 GitHub 上操作。项目模板已连接 GitHub 工具，因此无需设置。Copilot 首次使用这些工具时，VS Code 可能会提示你登录 GitHub。按照提示完成操作。稍后的练习将介绍此连接的工作方式（Model Context Protocol）。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保在智能体选择器中选中 **Agent**。
3. 让 Copilot 查找与更新说明文件有关的议题，并从当前分支创建 PR，说明其中包含说明更新和新的发行方辅助函数基础：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找议题并创建 PR。
5. 系统提示你允许 Copilot 代表你执行 GitHub 操作时，请检查命令并酌情选择 **Allow**。
6. PR 创建后，使用以下提示让 Copilot 合并 PR，并将分支切回 main：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 系统提示你允许 Copilot 执行 GitHub 操作和 shell 命令时，请检查命令并酌情选择 **Allow**。

你刚刚在 GitHub Copilot 的帮助下创建并合并了拉取请求！

## 总结和后续步骤

你已了解 Copilot 如何获取此项目的说明，并使用 VS Code 中的 Copilot Chat：

- 发送代码生成提示，并观察 Copilot 如何依据*现有*说明生成代码。
- 向 `.github/copilot-instructions.md` 添加新的全局存储库标准。
- 再次发送相同提示，观察建议的代码如何遵循新标准。
- 将说明更新和辅助函数基础提交到 `main`，以便下一练习继续使用。

接下来，在[智能体模式][next-lesson]中应用这些说明，让 Copilot 跨代码库添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义说明][personal-instructions]
- [Awesome Copilot：说明文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
