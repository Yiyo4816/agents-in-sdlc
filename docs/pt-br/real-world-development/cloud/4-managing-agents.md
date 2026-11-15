---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você atribuiu ao agente de nuvem do Copilot três tarefas distintas para melhorar a experiência e adicionar funcionalidades. Embora o agente trabalhe de forma autônoma e assíncrona, ainda é importante monitorar essas tarefas.

Há várias ferramentas para gerenciar as tarefas atribuídas ao agente de nuvem, incluindo a [página de agentes][agents-page] do GitHub.com. Nesse painel de controle, você vê as tarefas com pull requests (PRs) abertos, explora as operações realizadas e orienta sessões em andamento.

Neste exercício, você vai:

- explorar a página de agentes para monitorar tarefas do agente de nuvem.
- orientar uma sessão em andamento e solicitar uma funcionalidade adicional.

## Cenário

Depois de atribuir ao agente a implementação do modo de alto contraste, a equipe percebeu que seria útil adicionar também um modo claro. Como o agente já estava atualizando o estilo do site e adicionando controles de alternância, você orientará o trabalho para incluir o modo claro além do alto contraste.

## Revisar as tarefas do agente de nuvem do Copilot

Confira o status das tarefas atribuídas ao agente de nuvem do Copilot.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe a lista de tarefas nos painéis principal e esquerdo. Ela deve incluir:
   - atualização da documentação da base de código.
   - geração de APIs para modificar jogos.
   - adição de um modo de alto contraste ao site.
3. Selecione uma tarefa em andamento e revise as operações do Copilot, que podem incluir:
   - obter o código do repositório.
   - configurar o ambiente de trabalho.
   - configurar servidores MCP.
   - executar etapas para concluir a tarefa.

> [!NOTE]
> As etapas exibidas variam de acordo com o estado do trabalho e a abordagem usada pelo Copilot.

4. Observe também o painel de pull request à direita, que mostra o PR e os arquivos alterados.

## Orientar o agente de nuvem

Agora que você conhece as tarefas ativas, peça ao Copilot que inclua o modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão que está adicionando o modo de alto contraste. O título exato varia conforme o nome usado pelo Copilot e o estado atual do trabalho.

    ![Sessão de acessibilidade no painel de controle](../../../_images/ex6-accessibility-session.png)

2. Aguarde alguns minutos até que a sessão indique que concluiu a configuração e começou a trabalhar.
3. Na caixa **Steer active session while Copilot is working**, envie este prompt:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

    ![Tarefa do agente de nuvem na página de agentes com a caixa de orientação da sessão ativa destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe como o Copilot confirma o recebimento e inclui a solicitação no fluxo de trabalho.

## Deixar o Copilot trabalhar

O Copilot continuará com a tarefa atualizada. Depois de concluir a etapa em andamento quando você enviou a mensagem, ele incorporará a nova solicitação ao fluxo.

Isso pode levar alguns minutos. Aproveite para revisar o que aprendeu até aqui.

## Resumo e próximos passos

Neste exercício, você explorou a página de agentes do Copilot, o painel de controle central para monitorar e orientar tarefas do agente de nuvem. Você pode acompanhar tarefas ativas e concluídas, revisar o trabalho e redirecionar tarefas em andamento.

Você explorou:

- como usar a página de agentes e o painel de controle para monitorar tarefas do agente de nuvem.
- como redirecionar uma sessão em andamento para solicitar funcionalidades adicionais.

Enquanto o Copilot conclui o trabalho de acessibilidade, você pode [iterar sobre os pull requests criados pelo Copilot][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
