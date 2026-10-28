---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几项练习中，你让 Copilot 云智能体处理了三项改进用户体验和添加功能的任务。云智能体能够异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个控制面板中，可以查看所有带有未关闭拉取请求 (PR) 的智能体任务，检查已执行的操作，还可以引导正在进行的会话。

在本课中，你将：

- 探索智能体页面，监控云智能体任务。
- 引导正在进行的会话，请求添加其他功能。

## 场景

将创建高对比度模式的任务分配给智能体后，团队发现也应该同时添加浅色模式。由于智能体已经在更新网站样式并添加切换功能，因此顺便实现浅色模式很合适。你将引导智能体，同时添加浅色和高对比度模式。

## 查看 Copilot 云智能体任务

先查看分配给 Copilot 云智能体的所有任务的当前状态。

1. 访问 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主区域和左侧区域中的任务列表。列表中应包含你分配给 Copilot 的任务，例如：
   - 为代码库补充文档。
   - 生成用于修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，包括：
   - 从存储库检出代码。
   - 创建 Copilot 工作环境。
   - 设置模型上下文协议 (MCP) 服务器。
   - 执行完成任务所需的其他步骤。

> [!NOTE]
> 具体显示的步骤因 Copilot 工作进度和执行方案而异。

4. 查看右侧显示的拉取请求面板，以便进一步监控 PR 和更改的文件。

## 引导云智能体

你已了解当前任务的状态。现在在智能体处理高对比度模式时，要求 Copilot 同时添加浅色模式切换开关。

1. 选择与添加高对比度模式相关的会话。会话标题因 Copilot 使用的名称和当前工作状态而异。

   ![控制面板中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到会话显示设置已完成且任务已开始。你会看到类似下方的消息。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示词：

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![智能体页面中的云智能体任务，突出显示 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示词。
5. 查看 Copilot 如何确认收到提示词并将其纳入工作流程。

## 等待 Copilot 完成任务

与之前一样，Copilot 会继续处理更新后的任务。它会先完成收到消息时正在执行的步骤，再将新请求加入工作流程。

这通常需要几分钟。等待期间，可以回顾目前学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot 智能体页面，它是监控和引导 GitHub Copilot 云智能体任务的中心位置。你可以通过该控制面板跟踪所有进行中和已完成的任务、审查智能体正在执行的工作，也可以重定向正在运行的任务以调整范围或提供额外指导。

本课涉及以下内容：

- 探索控制面板和智能体页面，以监控云智能体任务。
- 重定向正在进行的会话，请求添加其他功能。

Copilot 正在完成无障碍功能，接下来将[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
