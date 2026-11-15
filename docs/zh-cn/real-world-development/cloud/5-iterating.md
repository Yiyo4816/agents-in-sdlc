---
title: "练习 5：迭代优化 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 检查工作

在本实验中，你与 GitHub Copilot 合作完成了多项改进用户体验和添加功能的任务。你让 Copilot 为代码添加文档、为设计团队构建可迭代优化的相关游戏功能，并实现无障碍功能，包括高对比度和浅色模式切换开关。现在检查代码更改；如有需要，还可以向 Copilot 提供反馈，改进其工作。之后再启用团队其他成员的审查。

### 场景

如前所述，加入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。始终需要检查生成的代码，并遵循常规 DevOps 流程。接下来检查 GitHub Copilot 为创建文档、相关游戏功能和无障碍功能提出的更改。

## 安全和 GitHub Copilot 云智能体

Copilot 云智能体会异步且无人监督地执行任务，因此设置了一些安全限制，以确保操作安全：

- Copilot 对存储库只有读取权限，对工作分支只有写入权限。
- 云智能体在 GitHub Actions 中运行，并创建独立的临时环境来工作。
- 任何 GitHub Actions 工作流在运行前都需要人工批准。
- 默认情况下，[对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括 MCP 服务器。

## 检查生成的文档

先检查 GitHub Copilot 云智能体创建的第一个拉取请求（PR），其中包含代码文档。通过 GitHub.com 上的标准 PR 界面完成此操作。

> [!NOTE]
> 检查 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限地访问外部资源，包括外部 MCP 服务器调用。你可以[自定义或禁用 Copilot 云智能体防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开 PR 列表。
3. 打开标题类似 **Add missing documentation** 的 PR，也可能是更详细的标题。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题中会包含 **[WIP]**。此时请等待 Copilot 完成工作。这可能需要几分钟，可以休息片刻或回顾已学内容。

4. PR 准备就绪后，选择 **Files changed** 选项卡并检查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 浏览新更新的代码，其中包括新生成的 TSDoc 文档注释和其他文档。具体更改可能各不相同。

    检查更改时，留意 TSDoc 文档注释、TypeScript 约定和文件注释头。这些来自练习 2 开始时检查的自定义说明文件。

6. 检查更新并确认符合预期后，返回 **Conversation** 选项卡并向下滚动。
7. 你应能看到部分工作流正在等待批准的指示器。
8. 如果工作流正在等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应会进入 PR 检查队列。如果一切正常，单体 Astro 应用的项目检查就会通过。这可能需要几分钟。

## 请求 GitHub Copilot 更改

与 Copilot 合作处理拉取请求并非单向操作。你可以像提及团队成员一样，在 PR 评论或代码内联评论中提及 Copilot。Copilot 会读取这些评论并启动新会话处理。由于结果具有非确定性，无法提供唯一固定的提示文本。你可以要求 Copilot 更新以下内容：

- 在每个代码文件开头添加简短注释，说明文件用途。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用结构。

1. 添加评论请求 Copilot 更新生成的文档，并像提及普通用户一样标记 **@copilot**。可以使用上述建议，也可以提出其他文档改进请求。
2. 选择 **View Session**，观察 Copilot 如何执行工作。Copilot 会启动新会话并应用更新。
3. 选择 **Back to pull request** 返回 PR。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，PR 中应会出现新提交。
5. 选择 **Files changed** 选项卡检查更改。

可以继续迭代，直到满意为止。完成后，可将 PR 从草稿转换为准备审查状态，再合并到 main 分支。

![将 PR 转为准备审查状态](../../../_images/ex7-ready-for-review.png)

## 检查相关游戏功能

现在检查 Copilot 为解决游戏详情页显示相关游戏议题而创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR，也可能是更详细的标题。
4. 选择 **Files changed** 选项卡，检查生成的代码。
5. 检查更新并确认符合预期后，返回 **Conversation** 选项卡并向下滚动。
6. 你应能看到部分工作流正在等待批准的指示器。
7. 如果工作流正在等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 工作流应会进入 PR 检查队列。如果一切正常，单体 Astro 应用的项目检查就会通过。这可能需要几分钟。
9. **可选：**你也可以在 Codespace 中检出此分支，手动测试相关游戏功能。前往 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已构建相关游戏功能！与之前一样，你可以通过迭代向云智能体提出更新请求。例如，可以要求 Copilot 调整显示的相关游戏数量，或添加文件注释头和 TSDoc 文档注释（请记住，分配此任务时，你还没有更新自定义说明！）。与之前一样，可以在 **Conversation** 选项卡中添加评论来提出请求，Copilot 会读取评论并启动新会话。

## 检查无障碍功能

最后，检查使用无障碍自定义智能体实现的无障碍功能。此 PR 应同时包含练习 3 中分配的高对比度模式，以及练习 4 中在 mission control 请求的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR，也可能是更详细的标题。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题中会包含 **[WIP]**。此时请等待 Copilot 完成工作，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，检查生成的代码。
5. 检查实现，特别留意：
   - 用于切换模式的 UI 组件。
   - 使用本地存储保存用户偏好。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 检查更新并确认符合预期后，返回 **Conversation** 选项卡并向下滚动。
7. 你应能看到部分工作流正在等待批准的指示器。
8. 如果工作流正在等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应会进入 PR 检查队列。如果一切正常，单体 Astro 应用的项目检查就会通过。这可能需要几分钟。
10. **可选：**你可以在 Codespace 中检出此分支，手动测试无障碍功能。前往 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换开关，确认功能符合预期，并且页面重新加载后仍会保留设置。

无障碍自定义智能体帮助 Copilot 按照无障碍最佳实践实现这些功能。如果发现无障碍问题或改进点，可以像处理之前的 PR 一样，在评论中提及 **@copilot** 请求更新。

## 可选练习 - 继续委派任务

云智能体最适合处理真实待办事项，让你可以转而完成其他工作。养成这一习惯：在存储库中创建更多议题并分配给 Copilot。可以尝试以下任务：

- 在游戏详情页创建支持者兴趣表单。
- 为游戏列表页实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云智能体工作环境的学习。通过这些课程，你：

- **检查了此存储库提供的自定义说明文件**，以便之后观察其对云智能体输出的影响。
- **将议题分配给 Copilot 云智能体**，并观察它如何设置环境、制定计划并异步执行任务。
- **创建并使用自定义智能体**完成无障碍工作，添加高对比度和浅色模式切换开关。
- **使用 Copilot 智能体页面作为 mission control**，监视并引导进行中的无障碍会话。
- **检查并迭代优化云智能体的拉取请求**，通过提及 `@copilot` 请求更改并批准工作流。

## 回顾和后续步骤

你已完成云智能体工作环境的学习。若想继续探索，其他工作环境可以补充本系列所学内容：

- 🖥️ **[VS Code 工作环境](../../vscode/)** — 在 IDE 中探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 工作环境](../../cli/)** — 在终端中交付两项经过审查的更改，使用 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接。

在自己的存储库中，还可以尝试以下后续任务：

- 将重构或测试覆盖率议题分配给云智能体。
- 为其他领域创建自定义智能体。
- 为其他技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
