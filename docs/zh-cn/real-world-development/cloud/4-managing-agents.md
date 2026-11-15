---
title: "练习 4 - 监视和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几次练习中，你让 Copilot 云智能体处理了三个改进用户体验和添加功能的任务。云智能体虽然可以异步自主工作，但监视这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个 mission control 中，你可以查看所有创建了未关闭拉取请求（PR）的智能体任务，探索执行的操作，甚至引导进行中的会话。

本课将学习如何：

- 使用智能体页面监视云智能体任务。
- 引导正在进行的会话，请求添加其他功能。

## 场景

将高对比度模式任务分配给智能体后，团队意识到也可以同时添加浅色模式。由于网站样式和切换功能正在更新，添加这一功能也很合理。你将引导智能体，在添加高对比度模式的同时实现浅色模式。

## 检查 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的所有任务的状态。

1. 前往 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。你应能看到分配给 Copilot 的任务，包括：
    - 更新代码文档。
    - 生成用于修改游戏的 API。
    - 为网站添加高对比度模式。
3. 选择其中一个正在运行的任务，检查 Copilot 已执行的操作。可能包括：
    - 从存储库检出代码。
    - 为 Copilot 创建环境。
    - 设置 MCP 服务器。
    - 执行完成分配任务所需的各项操作。

> [!NOTE]
> 列出的具体步骤会因 Copilot 的工作状态和采用的方法而异。

4. 同时留意右侧显示的拉取请求（PR）窗格。这里可以查看 PR 和更改的文件，以便进一步监视。

## 引导云智能体

了解当前正在执行的任务后，让 Copilot 在实现高对比度模式时也添加浅色模式切换开关。

1. 选择与添加高对比度模式相关的会话。具体标题会因 Copilot 使用的名称和当前工作状态而异。

    ![mission control 中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 监视会话几分钟，直到会话显示设置已完成并开始执行任务。出现与以下内容相似的消息时，就表示已开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中，添加以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中的云智能体任务截图，突出显示 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认提示并将其加入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会继续处理更新后的任务！它会先完成发送消息时正在执行的步骤，再将新请求加入工作流程。

这同样需要几分钟，可以借此机会暂停一下，回顾目前学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot 智能体页面，它是监视和引导 GitHub Copilot 云智能体任务的中心位置。使用这个 mission control，可以跟踪所有活动和已完成的任务、检查执行情况，甚至重定向进行中的任务以调整范围或提供其他指导。

你学习了以下内容：

- 使用 mission control 和智能体页面监视云智能体任务。
- 重定向正在进行的会话，请求添加其他功能。

Copilot 正在完成无障碍功能。接下来[迭代优化 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代优化 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
