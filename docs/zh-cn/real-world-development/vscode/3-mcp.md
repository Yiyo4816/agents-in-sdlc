---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：智能体模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚刚使用智能体模式构建了筛选功能。创建拉取请求前，应先确认功能在浏览器中可以正常运行。你不必亲自操作应用，而是可以连接 **Playwright MCP 服务器**，让 Copilot 操作真实浏览器并测试功能，然后发布分支并创建 PR。

本练习将学习如何：

- 了解 Model Context Protocol (MCP) 以及 MCP 服务器如何使用新工具扩展 Copilot。
- 向工作区添加 Playwright MCP 服务器。
- 让 Copilot 使用它在浏览器中手动测试筛选功能。
- 发布分支，并为筛选功能创建拉取请求。

## 什么是 Model Context Protocol (MCP)？

当智能体模式可以访问编辑器之外的资源时，其功能会更强大。Copilot 使用 Model Context Protocol (MCP) 与外部工具和服务通信，这是智能体连接外部工具和服务的标准方式。

![智能体模式内部工作原理示意图，显示它如何与上下文、LLM 和工具交互，其中包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 为 AI 智能体提供了与外部工具和服务通信的方式。借助 MCP，AI 智能体可以实时通信，使用资源访问最新信息，并使用工具代表你执行操作。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁，并负责管理二者之间的通信，例如现有 API 或 NPM 包等本地工具。每个 MCP 服务器都代表 AI 智能体可以使用的一组工具和资源。

一些常用的 MCP 服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供一组用于管理 GitHub 存储库的 API，让 AI 智能体可以创建存储库、更新现有存储库并管理议题和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：提供基于 Playwright 的浏览器自动化功能，让 AI 智能体可以访问网页、填写表单和选择按钮。

还有许多其他 MCP 服务器提供不同工具和资源。GitHub 提供了 [MCP 注册表](https://github.com/mcp)，帮助用户发现并参与生态系统。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样对待 MCP 服务器。使用前，请仔细检查其源代码、确认发布者并考虑相关安全影响。只使用可信的 MCP 服务器，并谨慎授予其访问敏感资源或执行敏感操作的权限。

## 检查 MCP 配置

`.vscode/mcp.json` 文件用于配置此 VS Code 工作区可使用的 MCP 服务器。

1. 在 Codespace 中打开 `.vscode/mcp.json`。
2. 你应能看到已经配置好的 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

项目模板中已包含 `github` 条目，因此 Copilot 能在上一练习中读取待办事项中的议题。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装；VS Code 会通过 HTTP 连接服务器，并在 Copilot 首次使用其中的工具时要求你通过 GitHub 身份验证。

## 添加 Playwright MCP 服务器

现在添加第二台服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控的浏览器，正好可以用于测试功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 条目，与 `github` 并列。文件应如下所示：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 标记让 Playwright 在没有可见窗口的情况下运行浏览器，这在 Codespace 中是必需的，因为 Codespace 没有用于显示窗口的桌面环境。

> [!NOTE]
> Tailspin Toys 项目已在端到端测试中使用 Playwright，因此通常已安装 Playwright 所需的浏览器。如果 Copilot 后续报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会**按需**启动 MCP 服务器：Copilot 首次需要某服务器的工具时，VS Code 会启动它，并要求你确认是否信任该服务器。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会显示 **Start** 操作。选择此操作立即启动服务器，并确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请检查配置并选择信任，以便服务器启动。

手动启动是可选的。如果跳过，VS Code 会在下一步中 Copilot 首次需要服务器工具时启动服务器（并询问你是否信任它）。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可操作的真实浏览器。无需亲自选择页面和检查结果，智能体可以打开页面、应用筛选条件、读取结果并汇报观察情况。这样无需离开对话，就能快速确认功能是否符合预期。

Playwright MCP 服务器在底层使用页面的[无障碍树][playwright-mcp-server]，而不是截图。因此，智能体会像辅助技术一样，根据带有标签的结构化元素（按钮、链接和列表项）进行推理。这样，快速的功能检查也能顺便验证基本的无障碍行为。

服务器已连接且应用正在运行时，让 Copilot 测试刚构建的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器，逐步执行检查并报告结果。根据议题中的验收标准检查报告；如果发现问题，请提出后续问题，或让 Copilot 修复代码后再创建拉取请求。

> [!NOTE]
> 此测试要求应用在 `http://localhost:4321` 上运行。如果已停止开发服务器，请在发送提示前重新启动。Copilot 首次使用 Playwright MCP 服务器时，可能需要下载浏览器；如果提示缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求（PR），让团队检查更改。首先发布 `filtering-vscode` 分支。

1. 在 Codespace 中打开 **Source Control** 面板并检查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![显示更改内容的 Source Control 面板截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

可以通过多种方式创建拉取请求，例如使用 github.com 或 GitHub 命令行界面（CLI）。既然已在使用 GitHub Copilot，就让它来创建 PR！它可以查找相关议题并将其关联到 PR。

1. 前往 Copilot Chat 面板并选择 **New Chat**，开始新会话。
2. 确认在智能体下拉列表中选择了 **Agent**，这样 Copilot 才能使用 GitHub 工具。
3. 让 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 收集信息并执行操作。首次使用 GitHub 工具时，系统可能会要求你进行 GitHub 身份验证；按照提示完成操作。
5. 留意 Copilot 如何搜索议题、找到正确议题并创建 PR。
6. 选择 Copilot 生成的链接检查拉取请求，但**暂时不要合并**。

## 总结和后续步骤

恭喜！本练习中，你：

- 了解了 Model Context Protocol (MCP) 以及 MCP 服务器如何使用新工具扩展 Copilot。
- 向工作区添加了 Playwright MCP 服务器。
- 在发布前使用它在浏览器中手动测试了筛选功能。
- 发布了分支，并为筛选功能创建了拉取请求。

你已使用 MCP 服务器测试功能，但 MCP 只是扩展 Copilot 的一种方式。下一步将[创建自定义智能体][next-lesson]，简化无障碍审查等专门任务。

## 资源

- [MCP 究竟是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
