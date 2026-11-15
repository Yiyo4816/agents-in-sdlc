---
title: "练习 2 - 使用 Copilot 智能体模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也要修改多个文件并执行一些操作，例如运行测试。作为开发者，通常需要找到所有相关文件、进行更改、运行测试、调试、发现遗漏的文件并再次更新……这样的工作还会不断重复。

这正是 Copilot 智能体模式的用武之地。

Copilot 智能体模式能够在 IDE 中更自主地工作。它的行为类似开发者：先探索现有项目结构，再执行必要更新、运行测试等任务，并自动修复发现的问题。现在了解如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和云智能体的名称相似，但适用于不同的体验。智能体模式在 IDE 中执行任务，可以快速反馈并随时交互。云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将学习如何：

- 使用 Copilot 智能体模式探索项目、识别相关文件并协调更改。
- 使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能。
- 在将代码合并到代码库之前，检查 Copilot 智能体模式生成的更改和测试。

## 场景

随着游戏列表不断增加，你希望用户可以按类别和发行方筛选。上一练习中已经添加了发行方辅助函数，现在将使用 Copilot 智能体模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

进行更改前，先浏览 Tailspin Toys 网站，了解它目前的功能。

该网站是一个带有开发者主题的棋盘游戏众筹平台，可列出游戏并显示详细信息。它是一个 Astro 应用，构建时会将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有独立的后端 API 或客户端 UI 框架。可重用的数据访问辅助函数放在 `src/lib/` 中；交互功能由使用标准 DOM API 的小型、限定范围的 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。按照以下步骤在 GitHub Codespace 中运行：

1. 返回 Codespace。上一练习结束后应已切回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），在 Codespace 中打开新终端。
3. 检出最新 `main` 并为筛选功能创建新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下内容的网址横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否为 `http://localhost:4321` 打开浏览器窗口，请选择 **x** 将其关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后在终端中选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中选择终端里的 localhost URL 后，系统会自动将你重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是连接到 Codespace 的私有隧道，此时 Web 服务器正在 Codespace 中运行！

### 浏览网站

网站运行后即可浏览其功能。主要功能包括：

- **主页**：显示棋盘游戏列表，包括标题、图片和描述。
- **游戏详情页**：选择游戏后会打开详情页，其中包含标题、描述、发行方和类别等信息。

## 使用 Copilot 浏览待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个干净的对话。不同练习使用的模式和模型可能不同；需要时会在相应步骤中说明。

网站的初始实现已可正常运行，但还可以添加新功能。先查看待办事项。使用模板设置项目时，系统已自动创建一组议题；让 GitHub Copilot 显示这些项目。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。**Agent** 智能体会自主规划并跨文件实现更改、运行终端命令并调用工具。

   ![显示 Chat 视图中智能体选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某个模型。编写研讨会时使用的是 Claude Sonnet 4.5，因此将其列入说明，希望代码建议相对一致，以提供良好体验。不过，由于 LLM 具有概率性，收到的建议可能与研讨会示例不同。这完全正常。

> [!NOTE]
> LLM 具有概率性，因此 Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 使用以下提示询问 Copilot 存储库中的待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 检查生成的议题列表。

留意 Copilot 甚至根据它认为对用户最有用的程度为议题排序。

## 检查说明文件

让智能体生成代码前，先检查将用于提供上下文的说明文件。接下来会使用[用户界面（UI）说明][ui-instructions]，了解如何为网站添加功能。

1. 在 Codespace 中前往 `.github/instructions/ui.instructions.md`。
2. 留意该文件对添加功能的整体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向各类文件专用说明的链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 说明文件可以引用其他说明文件和项目中的文件。路径相对于说明文件所在位置。这样可以重复使用说明、将复杂内容拆分为较小且易于管理的部分，并提供示例和模板。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md

## 实现筛选功能

要完成筛选功能，至少需要对应用进行以下三项独立更新：

- 在数据层（`src/lib/`）中添加或完善剩余筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页面，加入筛选 UI。

此外，在合并到代码库之前，测试必须运行并通过。Copilot 智能体模式可以完成这些任务！现在添加此功能。

1. 可以继续使用当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 说明文件。

   ![选择 UI 说明文件的示例屏幕截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉列表中仍选中 **Agent**。

   ![显示 Chat 视图中智能体选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择 **Claude Sonnet 4.5** 模型。
5. 使用待办事项中相关议题的要求，提示 Copilot 实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目并查找相关文件。你应能看到它找到数据层辅助函数、UI 和测试，随后开始修改文件并运行测试。

   ![Copilot 探索项目文件的屏幕截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务复杂度和代码库大小，这可能需要几分钟。在此过程中，代码编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会显示 **Keep** 或 **Undo** 按钮，因此无需在工作期间选择它们。

7. 按照 Copilot 的提示选择 **Continue**，运行测试。

   ![Copilot Chat 窗格中的对话框截图，提示用户确认是否运行测试](../../../_images/ex3-agent-mode-run-tests.png)

8. 整个过程中可能会出现暂停，也可能看到测试失败。这没有关系！Copilot 会在生成代码和运行测试之间迭代，直至完成任务且不再发现错误。

   ![包含 Copilot 智能体模式完整会话的屏幕截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，留意潜在问题。

> [!CAUTION]
> 请记住，始终需要检查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，体验新功能！
11. 确认功能正常并检查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习中，你使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加了新功能。你了解到如何：

- 使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能。
- 使用 Copilot 智能体模式探索项目、识别相关文件并协调更改。
- 在将代码合并到代码库之前，检查 Copilot 智能体模式生成的更改和测试。

接下来使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 扩展练习 - 实现分页

随着游戏列表不断增加，需要启用分页。使用本练习中学到的技能，提示 Copilot 更新网站以实现分页。考虑以下事项：

- 遵循现有最佳实践，包括使用已有的说明文件。
- 决定如何实现分页，以及是否允许用户选择页面大小，还是固定页面大小。
- 编写提示时，向 Copilot 提供必要指导，使其按预期实现功能。
- 你可能需要与 GitHub Copilot 迭代，通过提出更改请求并提供上下文来优化结果。这是使用 Copilot 的正常工作流程！

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
