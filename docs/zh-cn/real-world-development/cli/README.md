---
slug: zh-cn/real-world-development/cli
title: "GitHub Copilot CLI"
description: "了解 Copilot CLI 模式、自定义项、MCP 工具和拉取请求自动化，同时构建、验证并交付 Tailspin Toys 的两项更改。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** 将 GitHub Copilot 作为智能体式编码助手带入终端。它可以探索代码库、生成代码、运行命令并连接外部工具，全部通过命令行完成；无需切换到图形界面编辑器，也能保持工作流畅。

本研讨会围绕 Tailspin Toys 的连续工作流展开：

1. 在 GitHub Codespaces 中准备项目、安装 Copilot CLI 并了解项目。
2. 修改星级评分，在浏览器中检查结果，并手动合并第一个拉取请求（PR）。
3. 从筛选功能议题开始，在 Plan 模式中确定方案，在 Autopilot 模式中构建功能，然后在 Interactive 模式中检查。
4. 更新存储库说明，并将其应用到筛选功能。
5. 自定义现有的 `quality-checks` 技能，并使用它运行项目检查。
6. 添加 Playwright Model Context Protocol (MCP) 服务器，并在浏览器中体验筛选功能。
7. 创建质量保证（QA）自定义智能体，检查需求、覆盖率和验证证据。
8. 检查完整的筛选功能更改，并对筛选 PR 使用 Agent Merge。
9. 探索用于管理上下文、模型、共享和可选云委派的实用斜杠命令。

为了使研讨会重点明确，你将创建两个 PR：先提交星级评分，再提交筛选功能及说明更新、技能更新、QA 配置文件和测试。筛选和质量工作流共用一个对话和分支，以便在探索各项工具时基于已有成果继续构建。

## 课程

| 课程 | 主题 | 说明 |
| ------ | ----- | ----------- |
| [0. 先决条件][ex0] | 设置 | 创建存储库和 Codespace |
| [1. 安装 Copilot CLI][ex1] | 安装 | 安装并验证 Copilot CLI，然后了解项目 |
| [2. 添加星级评分：快速上手][ex2] | 首次更改 | 显示现有评分和空值回退状态，然后合并第一个 PR |
| [3. 智能体模式：Plan 和 Autopilot][ex3] | 智能体模式 | 根据议题规划功能，使用 Autopilot 构建，再在 Interactive 模式中检查 |
| [4. 使用自定义说明引导 Copilot][ex4] | 上下文 | 了解并更新说明，再将其应用到筛选功能 |
| [5. 自定义并使用 quality-checks 技能][ex5] | 可重复检查 | 了解现有技能、更改报告格式并运行技能 |
| [6. 使用 Playwright MCP 验证功能][ex6] | 浏览器观察 | 在 CLI 中配置 MCP 并检查筛选行为 |
| [7. 创建并使用 QA 智能体][ex7] | 需求与覆盖率 | 创建并选择专用配置文件，然后收集最终验证证据 |
| [8. 创建并合并功能 PR][ex8] | 检查与合并 | 检查完整更改、创建 PR 并使用 Agent Merge |
| [9. GitHub Copilot CLI 中的斜杠命令][ex9] | CLI 功能 | 探索上下文、模型、共享和可选云委派 |
| [10. 总结和后续步骤][ex10] | 总结 | 回顾工作流、可重复使用的自定义项和更多资源 |
| [可选：集成 Foundry][foundry] | 托管智能体 | 准备模型、部署由目录数据支持的智能体并将其连接到网站 |

## 先决条件

参加本研讨会前，请确保具备以下条件：

- [ ] 拥有 GitHub 帐户，并订阅有效的 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划
- [ ] 有权创建存储库和 Codespace
- [ ] 熟悉终端或命令行操作

> [!TIP]
> 没有付费计划？通过 [GitHub Education][student-plan] 验证身份的学生可免费使用 GitHub Copilot。**Copilot Student** 计划包含本研讨会使用的智能体、MCP、代码审查和 Copilot CLI 功能。

> [!NOTE]
> 如果使用 Copilot Business 或 Copilot Enterprise，请确认管理员已为你启用 Copilot CLI。

## 开始

**[从先决条件开始 →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
