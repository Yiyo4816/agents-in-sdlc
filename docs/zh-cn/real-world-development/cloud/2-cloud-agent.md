---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

几乎所有组织都面临技术债务问题，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云智能体可以自主执行更新代码和添加功能等任务。智能体完成工作后，会创建草稿 PR，供开发者检查。这样可以将繁琐任务交给智能体，加快开发流程，让开发者专注于更重要的事项。

本课将使用 Copilot 云智能体了解以下内容：

- 如何自定义代码生成环境。
- 如何确保操作安全。
- 明确界定议题范围的重要性。
- 如何将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些需要处理的技术债务。最初受雇开发网站的承包商没有妥善编写文档，事实上，你会发现项目完全缺少文档。首先，团队希望为应用中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队准备改进游戏发现功能。他们希望每款游戏的详情页显示相关游戏，也就是相同类别的其他游戏，让用户可以继续浏览。他们暂时不需要完善的实现，只需要可用于 UX 验收测试的功能。目前这项工作受到了阻碍，不过当前还有优先级更高的议题。

这些任务很容易被推迟，非常适合分配给 Copilot 云智能体。它可以异步处理任务，让开发者先完成其他工作，之后再回来检查 Copilot 的成果并确认一切符合预期。

## 了解 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发者一样在后台执行任务。分配任务的方式有多种，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求跟踪进度、设置工作环境并开始处理任务。你可以在 Copilot 会话进行期间或完成后查看详情。解决方案准备好等待检查时，Copilot 会在拉取请求中提及你。

## 明确界定说明范围的重要性

GitHub Copilot 没有魔法，不能让你跳过思考。即使看似简单的操作，深入分析后也会发现其复杂性，因此请[谨慎界定分配给 Copilot 云智能体的任务范围][cloud-agent-best-practices]。把它当作 AI 结对程序员：分阶段工作、学习、尝试并逐步调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 此存储库中的自定义说明

前面的练习介绍了自定义说明及其如何指导 Copilot。如果已经完成自定义说明练习，就会看到 `.github/copilot-instructions.md` 的作用；如果尚未学习，现在可以快速查看。将工作分配给 Copilot 云智能体前，先只读查看存储库中已有的说明文件，以便稍后发现它们的影响。

在存储库的 GitHub 网页界面或已启动的 Codespace 中打开以下文件：

- `.github/copilot-instructions.md`：检查 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` frontmatter，它会将这些说明限定在 `**/*.test.ts` 文件中。

下一部分将 `Code lacks documentation` 议题分配给云智能体时，检查生成的拉取请求是否包含 TSDoc 文档注释、TypeScript 约定和文件注释头。这些内容来自上述说明文件。稍后的练习会再次检查 PR。

## 设置 Copilot 云智能体的开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，以确保环境正常。将任务分配给 Copilot 时也是如此，因为它会以类似软件工程师（SWE）的方式执行工作。

云智能体使用 [GitHub Actions][github-actions] 执行任务。你可以通过 `.github/workflows/copilot-setup-steps.yml` 文件配置[专用设置工作流][setup-workflow]，在 Copilot 开始工作前运行，以便它访问所需的开发工具和依赖项。实验室已预先配置此工作流，以确保练习顺利进行。它会为单体 Astro 应用提供 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及已迁移和填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 可从存储库的 "Actions" 选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 将权限限制为 *步骤所需的最低权限*。Copilot 会获得自己的令牌来执行操作。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），则需要 `contents: read` 权限。如果未在设置步骤中克隆存储库，步骤完成后 Copilot 会自动为你执行此操作。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在基于 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试时有可用数据。
      - name: Set up the database
        run: npm run db:setup
```

此文件与其他 GitHub 工作流文件相似，但有几点值得注意：

- 它包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会执行此作业。
- 留意 `workflow_dispatch` 触发器。它允许你从存储库的 **Actions** 选项卡手动运行工作流，以便在等待 Copilot 运行之前测试工作流是否成功。

## 添加文档

所有人都知道文档的重要性，但大多数项目的信息要么已过时，要么完全缺失。这类技术债务经常无人处理，会降低效率，也会增加维护代码库或引入新开发者的难度。Copilot 擅长创建文档，因此非常适合将这类议题分配给云智能体。它会在后台生成所需文档。后续练习会回来检查结果。

1. 在新的浏览器选项卡中前往 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建新议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的拉取请求（PR），标题应类似 `[WIP]: Code lacks documentation`。如果列表中尚未显示新 PR，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 可能需要几分钟才能在 PR 中显示待办事项列表。它需要先创建环境（运行之前介绍的工作流）、分析项目并确定最佳解决方案。

13. 检查列表及其计划完成的任务。
14. 向下滚动 PR 时间线，应能看到 Copilot 已开始处理议题。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的指示器。

16. 浏览实时会话，了解 Copilot 如何解决问题，包括探索代码并了解当前状态、暂停思考并决定合适的计划，以及编写代码。

这可能需要几分钟。Copilot 云智能体的主要目标之一是异步执行任务，让我们可以专注于其他工作。现在利用这一特性，再给云智能体分配一个任务，然后转而为应用编写代码以添加功能。

## 为游戏详情页添加相关游戏部分

如前所述，GitHub Copilot 云智能体的一大优势是可以拆分工作：你专注于一组任务时，它可以处理另一组任务。为设计团队添加相关游戏部分可能不会花很长时间，但这段时间仍可用于其他工作。现在将任务分配给 Copilot 云智能体！

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，你会在议题的第一条评论中看到 👀，表示 Copilot 已开始处理！

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求！云智能体的工作方式类似软件工程师，因此无需一直监视；等任务完成后再检查即可。现在转而了解如何创建和使用自定义智能体。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。可以将议题分配给云智能体，让它异步执行任务。Copilot 可用于处理技术债务、创建新功能，或协助将代码从一个框架迁移到另一个框架。

你学习了以下内容：

- 如何自定义代码生成环境。
- 如何确保操作安全。
- 明确界定议题范围的重要性。
- 如何将议题分配给 Copilot。

云智能体正在后台工作。接下来将了解如何创建和使用自定义智能体。[Copilot 云智能体也可以使用 MCP 服务器][cloud-agent-mcp]，并可访问前面模块介绍的自定义说明。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
