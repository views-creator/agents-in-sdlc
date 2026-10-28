---
title: "Lição 2 - Adicione avaliações por estrelas: uma vitória rápida"
description: "Use o Copilot CLI para fazer uma pequena alteração nos cards de jogos, revisar a mudança em um navegador encaminhado e mesclá-la como seu primeiro pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Agora que você instalou o GitHub Copilot CLI, uma ferramenta de interface de linha de comando (CLI), e iniciou uma conversa, faça sua primeira alteração no projeto. Mantenha o escopo pequeno: os jogos já têm uma avaliação em estrelas nos dados, mas os cards da página inicial ainda não a exibem. Peça ao agente que mostre a avaliação, revise a alteração e faça o merge do primeiro pull request.

Nesta lição, você vai:

- iniciar uma conversa específica do Copilot em uma branch de funcionalidade;
- pedir ao agente que faça uma pequena alteração no projeto;
- revisar a alteração com `/diff`;
- executar o aplicativo e confirmar a mudança em um navegador encaminhado;
- abrir e mesclar seu primeiro pull request.

## Cenário

Cada jogo da Tailspin Toys pode ter uma avaliação em estrelas, que já aparece na página de detalhes. Os cards da página inicial mostram apenas o título, a categoria, a editora e a descrição. Como aquecimento, peça ao agente que exiba a avaliação existente em cada card: uma alteração pequena e independente, ideal para sua primeira sessão.

## Estrutura de uma conversa

Uma **conversa** é onde você trabalha com o Copilot CLI em uma tarefa. Diferentemente do aplicativo Copilot, uma conversa normal da CLI usa o repositório e a branch atualmente selecionados no terminal, em vez de criar uma worktree dedicada. As conversas salvas permitem retomar a mesma discussão depois, enquanto os arquivos e a branch continuam sendo o estado comum do Git no disco.

Em uma conversa, você verá os prompts e as respostas, a atividade das ferramentas usadas pelo agente para explorar e editar arquivos e as alterações que poderá inspecionar com `/diff`.

## Inicie uma conversa e solicite a alteração

Inicie uma conversa para começar a implementar o recurso.

1. Volte ao Codespace.
2. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Se o Copilot ainda não estiver em execução, use o comando:

   ```bash
   copilot --yolo
   ```

4. Inicie uma sessão nova com o comando de barra `/new` e pressione <kbd>Enter</kbd>.
5. Solicite a alteração com o prompt:

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

O Copilot explorará o projeto, localizará os arquivos que exibem os detalhes dos jogos e criará o código necessário. Você adicionou um recurso com o Copilot CLI.

## Revise o diff

Revise todas as alterações geradas por IA antes de mesclá-las, mesmo quando forem pequenas.

1. Insira `/diff` e examine todos os arquivos alterados.
2. Confirme se o card mostra a avaliação numérica quando ela está disponível e `No rating yet` quando `starRating` é `null`.
3. Confirme se os testes cobrem os dois estados.
4. Revise os resultados das verificações executadas pelo Copilot e peça que ele corrija as falhas.
5. Quando terminar a revisão, pressione <kbd>Esc</kbd> para sair da tela de diff.

> [!NOTE]
> Como todas as ferramentas de IA generativa, o Copilot é probabilístico, não determinístico. O código exato pode variar. Revise o comportamento esperado, não uma implementação específica.

## Verifique as alterações

Não basta ler o código e presumir que funciona. Peça ao Copilot que inicie o site para que você possa examinar a interface no navegador encaminhado pelo Codespaces.

1. Peça ao Copilot que inicie o aplicativo:

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Confirme se os cards dos jogos mostram as avaliações de cinco estrelas.
4. Volte ao Copilot e peça que pare o servidor iniciado:

   ```plaintext
   Stop the development server you started.
   ```

## Abra e mescle seu primeiro pull request

Você criou um recurso. Agora, crie um pull request (PR) para mesclar o novo código ao projeto.

1. Peça ao agente padrão que faça commit da alteração:

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. Insira `/pr create`. Ao criar o PR, o Copilot CLI enviará o commit existente e mostrará o localizador uniforme de recursos (URL) do PR.
3. Abra o PR mantendo <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) pressionado e selecionando a URL exibida pelo Copilot CLI.
4. Revise os arquivos alterados e as verificações.
5. Quando estiver tudo pronto, selecione **Merge pull request** e confirme o merge.
6. Volte ao Codespace e saia do Copilot CLI com `/exit`.
7. Atualize sua cópia local de `main`:

   ```bash
   git checkout main
   git pull
   ```

## Resumo e próximos passos

Você publicou sua primeira alteração usando o GitHub Copilot CLI. Nesta lição, você:

- iniciou uma conversa específica do Copilot em uma branch de funcionalidade;
- orientou o agente a fazer uma pequena alteração nos cards de jogos;
- revisou a alteração com `/diff`;
- executou o aplicativo e confirmou a avaliação em um navegador encaminhado;
- abriu e mesclou seu primeiro pull request.

Em seguida, [comece pela issue de filtragem e use os modos Plan e Autopilot][next-lesson] para criar um recurso maior.

## Recursos

- [Sobre o GitHub Copilot CLI][about-copilot-cli]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
