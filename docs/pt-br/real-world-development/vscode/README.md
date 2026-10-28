---
slug: pt-br/real-world-development/vscode
title: "Visual Studio Code"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

O **[GitHub Copilot Chat](https://code.visualstudio.com/docs/copilot/chat/copilot-chat)** no Visual Studio Code (VS Code) traz o GitHub Copilot para o editor de código que você já usa. No VS Code e no GitHub Codespaces, você usará o Copilot Chat no modo de agente, conectará ferramentas externas por Model Context Protocol (MCP) e contará com agentes personalizados — tudo sem sair do ambiente de desenvolvimento integrado (IDE), onde o Copilot tem acesso aos arquivos, ao terminal e aos problemas do projeto.

Você começará adicionando instruções personalizadas e observando o Copilot segui-las; depois, usará o modo de agente para criar uma funcionalidade de filtragem na interface, na camada de dados e nos testes. Em seguida, conectará o servidor MCP do Playwright e deixará o Copilot controlar um navegador para testar o recurso antes de abrir um pull request. Por fim, revisará e usará um agente personalizado para acessibilidade e acompanhará, orientará e ajustará as alterações do Copilot — tudo sem sair do editor.

## Exercícios

| Exercício | Tópico | Descrição |
|----------|-------|-------------|
| [0. Pré-requisitos][ex0] | Configuração | Crie seu repositório e codespace |
| [1. Instruções personalizadas][ex1] | Contexto | Adicione e verifique instruções personalizadas no VS Code |
| [2. Modo de agente][ex2] | Geração de código | Crie um recurso de filtragem com o modo de agente |
| [3. MCP com Playwright][ex3] | Ferramentas externas | Teste o recurso em um navegador com o servidor MCP do Playwright |
| [4. Agentes personalizados][ex4] | Agentes especializados | Revise e use agentes personalizados |
| [5. Gerenciar agentes][ex5] | Monitoramento | Acompanhe e oriente sessões de agentes |
| [6. Iterar][ex6] | Revisão | Revise localmente o trabalho do Copilot e escolha os próximos passos |
| [Opcional: Incorporar o Foundry][foundry-toolkit] | Agentes de IA | Prepare um modelo, implante um agente e conecte-o ao site em três módulos com o VS Code e o Foundry Toolkit |

## Pré-requisitos

Antes de participar deste workshop, verifique se você tem:

- [ ] Uma conta do GitHub com um plano ativo **Copilot Student, Pro, Pro+, Business ou Enterprise**
- [ ] Acesso ao GitHub Codespaces

> [!TIP]
> Não tem um plano pago? Estudantes verificados podem obter o GitHub Copilot gratuitamente por meio do [GitHub Education][callout-student-plan-education]. O plano **Copilot Student** inclui o agente, MCP, revisão de código e o Copilot CLI, a interface de linha de comando (CLI) do GitHub Copilot, usados neste workshop — portanto, você pode concluir todos os percursos com ele.

[callout-student-plan-education]: https://github.com/education/students

## Comece

**[Comece pelo Exercício 0: Pré-requisitos →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-agent-mode/
[ex3]: 3-mcp/
[ex4]: 4-custom-agents/
[ex5]: 5-managing-agents/
[ex6]: 6-iterating/
[foundry-toolkit]: 7-foundry-toolkit/
