---
title: "第 7 课 - 创建并使用质量保证 (QA) 智能体"
description: "创建质量保证 (QA) 自定义智能体，结合议题需求、quality-checks 技能和 Playwright 模型上下文协议 (MCP) 服务器。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动化检查，并使用 Playwright 的模型上下文协议 (MCP) 服务器在浏览器中观察筛选体验。现在将这些能力整合到一个自定义智能体中，定义清晰的 QA 流程。

在本课中，你将：

- 探索自定义智能体如何结合指令、技能和 MCP 工具。
- 创建并检查可复用的质量保证配置文件。
- 选择 QA 智能体，并根据筛选功能议题审查其发现。

## 场景

Tailspin Toys 希望在创建拉取请求 (PR) 前，统一审查需求、代码质量、自动化检查、测试覆盖和浏览器行为。自定义智能体可以协调 QA 流程，并生成可复用的报告。

## 什么是自定义智能体？

自定义智能体是在 Markdown 配置文件中定义的 Copilot 专用版本。配置文件说明智能体的用途、指令和可用工具。在本研讨会中，你将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

你已使用的自定义项各有不同用途：存储库指令说明团队标准；`quality-checks` 技能封装可重复执行的检查；Playwright MCP 提供浏览器工具；QA 配置文件则指导 Copilot 如何使用这些能力评估需求并报告发现。它不会取代这些功能，也不需要另开对话。

## 创建 QA 配置文件

创建功能 PR 前，先让 Copilot 创建一个可复用的 QA 配置文件。配置文件将定义 QA 执行的检查及其必须遵循的边界。

1. 返回 Codespace，确认筛选功能对话处于 Interactive 模式。
2. 要求默认智能体创建新的自定义智能体：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置文件

使用新智能体前，先确认 Copilot 已记录预期的 QA 流程和边界。这样可避免配置不完整或范围过宽，导致只想验证功能时却修改了实现。

1. 输入 `/diff`，打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 为必填项；`name` 为可选项，但添加后会显示明确的名称。
3. 阅读配置文件指令，确认 QA 会从需求出发、遵循存储库指令、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持证据、修改实现代码前会征求同意，并且不会提交更改或创建拉取请求。
5. 如果生成的配置文件缺少上述职责或边界，请要求默认智能体修改后再继续。

## 根据议题运行 QA

Copilot CLI 会在对话启动时加载项目智能体。创建配置文件后，恢复同一筛选功能对话并选择 QA，这样它便可使用已有的议题和规划决策。

1. 输入以下提示词启用智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 新创建的智能体可能不会显示在可用智能体列表中，但仍可通过上述命令启用。

2. 使用以下提示词要求 QA 智能体审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. 等待 QA 智能体执行检查。
4. 完成后阅读它提供的报告。

## 总结和后续步骤

你为工作流添加了可复用的专用角色，并审查了它的工作。在本课中，你：

- 探索了自定义智能体如何结合指令、技能和 MCP 工具。
- 创建并检查了可复用的质量保证配置文件。
- 选择了 QA 智能体，并根据筛选功能议题审查了它的发现。

现在，功能实现、技能更新、QA 配置文件、测试和验证报告都已准备好审查。接下来，你将[整合这些更改并创建功能 PR，再使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
