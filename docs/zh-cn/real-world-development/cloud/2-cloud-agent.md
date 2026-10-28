---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都会遇到技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云智能体可以自主执行代码更新和功能添加等任务。任务完成后，它会创建草稿拉取请求 (PR)，供开发人员审查。这样可以将繁琐任务交给智能体处理，加快开发进度，让开发人员专注于更重要的工作。

本练习将介绍如何使用 Copilot 云智能体：

- 自定义代码生成环境。
- 确保操作安全。
- 了解明确限定议题范围的重要性。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些需要处理的技术债务。最初为网站开发的承包商没有留下完善的文档，事实上，代码完全缺少文档。团队希望首先为应用中所有导出的函数添加 TypeScript 文档注释 (TSDoc)。

此外，设计团队希望改进游戏发现体验。他们想在每款游戏的详情页显示相关游戏，也就是同一类别中的其他游戏，让用户可以继续浏览。目前不要求实现经过完善的功能，只需提供可用于用户体验验收测试的版本。此需求目前是阻塞项，但优先级低于其他议题。

这两项都是容易被推迟的任务，适合分配给 Copilot 云智能体。它可以异步处理任务，让开发人员继续处理其他工作，之后再回来审查 Copilot 的成果。

## 了解 GitHub Copilot 云智能体

[GitHub Copilot 云智能体][copilot-agents]可以像人类开发人员一样在后台执行任务。你可以通过多种方式与它协作，例如[将 GitHub 议题分配给 Copilot][assign-issue]。分配任务后，Copilot 会创建草稿拉取请求以跟踪进度、设置工作环境并开始处理任务。你可以在会话进行中或结束后查看具体过程。解决方案准备好供你审查时，Copilot 会在拉取请求中通知你。

## 明确限定任务范围的重要性

GitHub Copilot 并非魔法，使用它仍然需要思考。即使看似简单的操作，深入分析后也可能包含复杂情况，因此需要[谨慎限定交给 Copilot 云智能体的任务范围][cloud-agent-best-practices]。将它视为人工智能 (AI) 配对编程伙伴：分阶段工作、学习、试验并持续调整。引入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义指令

前面的练习介绍了自定义指令及其如何引导 Copilot。如果已经完成自定义指令练习，就已看到 `.github/copilot-instructions.md` 的作用；否则，现在可以快速查看。分配任务给 Copilot 云智能体前，先只读浏览存储库中的指令文件，之后便能观察它们的影响。在 GitHub 网页界面或正在运行的 Codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：查看 `applyTo` frontmatter，了解这些指令如何应用于 `**/*.test.ts` 文件。

下一节将 `Code lacks documentation` 议题分配给云智能体时，注意查看生成的拉取请求中是否包含 TSDoc 文档注释、TypeScript 约定和文件头注释。这些内容来自上述指令文件，后续练习还会再次检查。

## 为 Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要准备特定环境并运行一些设置脚本，确保开发环境正常。将任务分配给 Copilot 时也一样，因为它会以类似软件工程师 (SWE) 的方式处理任务。

云智能体使用 [GitHub Actions][github-actions] 作为工作环境。你可以创建[专用设置工作流][setup-workflow]，并将其配置在 `.github/workflows/copilot-setup-steps.yml` 中，让工作流在智能体开始工作前运行，以便提供所需的开发工具和依赖项。实验已预先配置此工作流，以便顺利完成练习。它会准备 Node.js、项目依赖项、端到端测试所需的浏览器，以及这款单 Astro 应用使用的迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 可在存储库的“Actions”选项卡中测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 为这些步骤设置所需的最低权限。Copilot 会获得自己的令牌来执行操作。
    permissions:
      # 如果设置步骤需要克隆存储库（例如安装依赖项），就需要 contents: read 权限。如果未在设置步骤中克隆存储库，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在都是 Astro + Drizzle/libSQL）
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

      # 迁移并填充本地 SQLite 数据库。
      - name: Set up the database
        run: npm run db:setup
```

该文件看起来与其他 GitHub 工作流类似，但有几点值得注意：

- 它包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会运行此作业。
- `workflow_dispatch` 触发器允许你在存储库的 **Actions** 选项卡中手动运行工作流。这有助于测试设置流程，而不必等到 Copilot 执行任务时才发现问题。

## 添加代码文档

许多项目的文档已经过时，或者完全缺失，尽管大家都知道文档的重要性。这类技术债务常常无人处理，会拖慢工作效率、增加代码维护难度，也让新开发人员更难加入团队。Copilot 擅长生成文档，因此很适合将这项工作分配给云智能体。它会在后台生成所需文档，你将在后续练习中审查结果。

1. 在新的浏览器选项卡中访问 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的拉取请求 (PR)，其标题类似于 `[WIP]: Code lacks documentation`。如果列表中尚未显示新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会创建待办清单。

> [!NOTE]
> Copilot 可能需要几分钟才会在 PR 中创建待办清单。它正在创建环境（运行前面提到的工作流）、分析项目并确定处理任务的方案。

13. 审查清单和 Copilot 即将执行的任务。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的状态指示器。

16. 浏览实时会话，查看 Copilot 如何处理问题，包括探索代码和理解当前状态、暂停以思考并确定合适方案，以及编写代码。

这可能需要几分钟。Copilot 云智能体的一项主要优势是能够异步执行任务，让开发人员腾出时间处理其他工作。接下来利用这一优势，再分配一项任务给云智能体，然后将注意力转向为应用编写新功能。

## 在游戏详情页添加相关游戏部分

如前所述，GitHub Copilot 云智能体的一大优势是可以拆分工作：你专注于一组任务，它同时处理另一组任务。即使为设计团队添加相关游戏部分不会花太久，也可以把时间用于其他工作。现在将此任务分配给云智能体。

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

   ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

稍后你会在议题的第一条评论旁看到一组 👀，表示 Copilot 已开始处理。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求。云智能体的工作方式类似软件工程师，因此你不需要一直监控它，完成后再审查即可。接下来创建并使用自定义智能体。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。使用云智能体可以将议题分配给 Copilot 异步处理。你可以用它处理技术债务、创建新功能，或协助将代码从一个框架迁移到另一个框架。

本课涉及以下内容：

- 自定义代码生成环境。
- 确保操作安全。
- 了解明确限定议题范围的重要性。
- 将议题分配给 Copilot。

云智能体正在后台处理任务，现在可以开始创建和使用自定义智能体。[Copilot 云智能体也支持模型上下文协议 (MCP) 服务器][cloud-agent-mcp]，并且可使用前面练习中介绍的自定义指令。

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
