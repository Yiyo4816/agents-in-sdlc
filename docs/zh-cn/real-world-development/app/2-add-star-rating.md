---
title: "第 2 课 - 添加星级评分：快速上手"
description: "在 GitHub Copilot app 中启动第一个智能体会话，为游戏卡片进行小幅修改，并将其作为第一个拉取请求合并。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

了解工作区并体验快速聊天后，现在可以启动一个**智能体会话**，对项目进行首次修改了。我们先从小处着手：游戏数据中已经包含星级评分，但主页上的游戏卡片尚未显示它。你将让智能体显示评分、审查更改，并通过第一个拉取请求合并它。

本课将学习如何：

- 启动智能体会话并了解会话的组成。
- 让智能体对项目进行小幅、聚焦的修改。
- 在工作区差异视图中审查更改。
- 在本地运行应用，并在浏览器中确认更改。
- 打开并合并第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都可以有星级评分，而且游戏详情页已经显示了评分。不过，主页上的游戏卡片只显示标题、类别、发行商和描述。作为热身练习，你将让智能体在每张卡片上显示现有评分。这项小而独立的修改非常适合首次会话。

## 会话结构

**会话**是与智能体进行的对话，它在独立的工作区中运行。每个会话都有专属的 **git 工作树和分支**，因此你可以同时运行多个会话——一个添加功能，另一个修复 bug——而不会发生更改冲突。会话按存储库分组显示在侧边栏中；选择任一会话即可切换到该会话。

会话中有三项内容：与智能体的**对话**、智能体探索和编辑文件时的**工具活动**，以及显示差异的**已更改文件**列表。

## 启动会话并提出修改请求

现在启动新会话，开始探索项目并实现功能。在[应用设置期间][prior-lesson]，你已从 GitHub 存储库将项目添加到应用。我们将为该存储库创建新会话，并提出修改请求。

1. 返回或打开 GitHub Copilot app。
2. 在侧边栏顶部选择 **+**，启动会话。
3. 为存储库选择 `tailspin-toys`。
4. 选择**新工作树**，并在提示框下方选择 **Interactive** 模式。使用以下提示请求修改：

    ```plaintext
    Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
    ```

5. 按 <kbd>Enter</kbd> 将提示发送给 Copilot。

Copilot app 会先创建新的工作树，也就是项目的独立副本。然后，它会探索项目，找到需要修改以添加功能的文件，并编写所需代码。现在，你已使用 Copilot app 添加了一项新功能！

## 审查差异

所有 AI 生成的更改都应在合并前审查，即使只是小改动也不例外。现在就在 GitHub Copilot app 中查看更改。

1. 在应用右上角选择 **Toggle review panel**。在打开的面板中选择 **Changes**，查看已更改的文件及其差异。

    ![GitHub Copilot app 顶部工具栏中的截图，箭头指向 Create PR 右侧的 Toggle review panel 按钮](../../../_images/app-2-review-panel.png)

2. 你应该会看到 `GameCard.astro` 中新增的代码。这个核心文件用于显示游戏详情。代码应该类似下面的小代码块：如果 `starRating` 存在，就显示评分；如果其值为 `null`，则显示 “No rating yet”：

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> 与所有生成式 AI 工具一样，Copilot 的输出具有概率性，并非完全确定，因此实际代码可能与上述示例有所不同，但整体应该比较相似。

## 检查更改

打开浏览器前，先审查智能体自动检查的结果。确认测试涵盖数值型 `starRating` 和 `null` 回退情况。缺少先决条件或跳过的检查不算通过；批准任何安装请求前，请先审查该请求。

当然，我们不能只读代码就假定它能正常工作。让 Copilot 打开网站，以便检查更新后的界面。可以让它启动网站，并在浏览器画布中打开。

> [!TIP]
> 画布是 GitHub Copilot app 内置的交互式小组件。稍后你会探索自定义画布，甚至创建自己的画布；现在先使用内置的浏览器画布。

1. 使用以下提示，请求 Copilot 启动应用并在浏览器画布中打开页面：

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. 片刻之后，应用会启动，GitHub Copilot app 中也会打开一个浏览器窗口。
3. 确认带评分的游戏卡片按五分制显示评分值。
4. 完成后，使用以下提示要求 Copilot 停止它为此会话启动的开发服务器：

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## 打开并合并第一个拉取请求

功能已完成！现在创建拉取请求（PR），将新代码合并到现有代码库中。

1. 在右上角选择 **Create PR**。
2. 如果出现提示，请选择 **Sign in with your browser**，并按照提示完成身份验证。
3. 如果 Copilot 说明存储库策略要求先关联议题，请确认它应遵循该流程。
4. Copilot 将开始创建 PR。
5. 选择聊天上方的 **PR** 气泡，在审查面板中打开 PR。你可以在此查看并审查拉取请求。
6. 准备好后，选择 **Ready to merge**。
7. 在新对话框中选择 **Merge pull request**，合并拉取请求！

## 总结和后续步骤

恭喜！你使用 GitHub Copilot app 发布了第一个更改！具体来说，你：

- 启动了智能体会话并了解会话的组成。
- 指示智能体对游戏卡片进行小幅、聚焦的修改。
- 在工作区差异视图中审查了更改。
- 在本地运行应用，并在浏览器中确认星级评分。
- 打开了第一个 PR，审查其检查结果，并明确合并了它。

接下来，你将[从筛选议题开始，并使用 Plan 和 Autopilot 模式][next-lesson]构建更大的功能。

## 资源

- [在 GitHub Copilot app 中使用智能体会话][agent-sessions]
- [关于 GitHub Copilot app][about-copilot-app]
- [使用 GitHub Copilot app 管理议题和拉取请求][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#安装并配置-github-copilot-app
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
