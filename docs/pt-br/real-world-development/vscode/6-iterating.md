---
title: "Exercício 6 - Iterar sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
next: false
---

| [← Lição anterior: Monitorar e gerenciar agentes][previous-lesson] |
| :-- |

## Revise o trabalho

Ao longo deste laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência. Usou o modo de agente para adicionar filtragem no cliente e no servidor, o servidor do Model Context Protocol (MCP) do Playwright para testar manualmente o recurso no navegador e um agente personalizado para implementar recursos de acessibilidade — controles de alto contraste e modo claro — e orientar a sessão em andamento para ampliar o trabalho. Agora, publique essas alterações locais e revise-as como sua equipe faria.

### Cenário

Os fundamentos do design de software e do DevOps não mudam com a IA generativa. Você ainda precisa de um ciclo de revisão real para tudo o que o Copilot produz. Envie as alterações de acessibilidade do codespace ao repositório remoto, abra um pull request e examine o diff antes de envolver o restante da equipe.

## Publique os recursos de acessibilidade

Os controles de alto contraste e modo claro implementados pelo agente personalizado de acessibilidade nos [Exercícios 4][exercise-4] e [5][exercise-5] estão no codespace como alterações em commits. Publique-as em uma branch no repositório remoto e abra um pull request para que a equipe possa revisá-las.

1. Volte ao codespace.
2. Abra a exibição **Source Control** no VS Code.
3. Confirme se as alterações de acessibilidade foram registradas em commits. Se houver alterações do Exercício 5 ainda sem commit, prepare-as e faça commit com uma mensagem descritiva, como `Add high-contrast and light-mode toggles`.
4. Publique a branch selecionando **Publish Branch** (ou use o menu **...** → **Push**).
5. O VS Code oferecerá a opção de abrir a nova branch no github.com. Aceite ou acesse o repositório manualmente e selecione **Compare & pull request** no banner da branch.
6. Defina um título claro, como `Add high-contrast and light-mode toggles`, e uma breve descrição do que foi feito e por quê.
7. Selecione **Create pull request**.
8. Quando o pull request (PR) estiver aberto, selecione **Files changed** para revisar as alterações de ponta a ponta. Preste atenção especial a:
   - componentes da interface para alternar entre modos;
   - uso do armazenamento local para persistir as preferências;
   - alterações de folhas de estilo em cascata (CSS) ou estilos para os modos de alto contraste e claro;
   - atributos de acessibilidade, como rótulos de Accessible Rich Internet Applications (ARIA) e navegação pelo teclado;
   - código JavaScript ou TypeScript que controla a alternância entre modos.
9. Volte à guia **Conversation**.
10. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

    ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

11. Aguarde a conclusão dos workflows. Se tudo estiver correto, eles deverão ser aprovados.

> [!TIP]
> Quer uma segunda opinião sobre o trabalho de acessibilidade? Mencione `@copilot` em um comentário no PR com uma solicitação, como "revise este PR em busca de outros problemas relacionados às Diretrizes de Acessibilidade para Conteúdo Web (WCAG)" ou "sugira melhorias para a navegação pelo teclado". O Copilot iniciará uma sessão para atender ao comentário.

## Exercício opcional - continue explorando localmente

Trabalhar de forma iterativa com um agente no ambiente de desenvolvimento integrado (IDE) é uma habilidade que se desenvolve com a prática. Algumas ideias para sessões futuras no VS Code:

- adicionar um formulário de interesse para apoiadores na página de detalhes do jogo;
- implementar paginação na página de listagem de jogos;
- adicionar validação de entrada e tratamento de erros aos auxiliares de acesso a dados em `src/lib/`;
- ampliar o escopo do agente de acessibilidade, por exemplo, para auditar a ordem de foco do teclado em todo o site.

## Resumo

Parabéns — você concluiu o percurso do VS Code. Ao longo deste laboratório, você:

- **Usou o MCP do Playwright para testar manualmente o recurso.** Você adicionou o servidor MCP do Playwright e deixou o Copilot controlar um navegador para verificar a filtragem antes de abrir um pull request.
- **Orientou o modo de agente em alterações coordenadas na pilha.** Você adicionou um recurso de filtragem que envolveu o cliente, o servidor e os testes em uma única sessão.
- **Usou um agente personalizado.** Você selecionou no seletor de agentes o agente personalizado de acessibilidade e acompanhou a implementação do modo de alto contraste no repositório.
- **Gerenciou e orientou uma sessão de agente.** Você revisou as alterações propostas no editor, aceitou as desejadas e ampliou a sessão com um prompt complementar sobre o modo claro.
- **Concluiu o ciclo com um pull request.** Você publicou o trabalho local e o revisou de ponta a ponta, como sua equipe faria.

## Revisão e próximos passos

O percurso obrigatório do VS Code está concluído. Você pode encerrar o workshop aqui.

Se quiser ampliar sua perspectiva sobre os recursos de agente do Copilot, os outros percursos abordam cenários relacionados em interfaces diferentes:

- **[Percurso da CLI](../../cli/)** — entregue duas alterações revisadas no terminal usando Plan e Autopilot, skills, um agente personalizado de controle de qualidade (QA), MCP do Playwright, Agent Merge e transferências com contexto.
- **[Percurso do agente de nuvem](../../cloud/)** — atribua issues ao agente de nuvem, acompanhe sessões na página de agentes e itere de forma assíncrona em pull requests.

Você também pode continuar desenvolvendo o que começou aqui. O [awesome-copilot][awesome-copilot] é uma ótima fonte de arquivos de instruções, agentes personalizados e skills que você pode adaptar aos seus projetos.

Como extensão opcional, [Opcional: Incorporar o Foundry][exercise-7] usa o VS Code e o Microsoft Foundry Toolkit para preparar um modelo, implantar um Backer Concierge e conectá-lo ao site.

## Recursos

- [GitHub Copilot][github-copilot]
- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[previous-lesson]: ../5-managing-agents/
[exercise-4]: ../4-custom-agents/
[exercise-5]: ../5-managing-agents/
[exercise-7]: ../7-foundry-toolkit/
[github-copilot]: https://github.com/features/copilot
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
[awesome-copilot]: https://github.com/github/awesome-copilot
