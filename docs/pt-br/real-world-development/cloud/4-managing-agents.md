---
title: "Exercício 4 - Acompanhe e gerencie agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos exercícios anteriores, você atribuiu três tarefas ao agente de nuvem para melhorar a experiência do usuário e adicionar funcionalidades. Embora o agente trabalhe de forma assíncrona e autônoma, é importante acompanhar essas tarefas.

A [página de agentes][agents-page] do GitHub.com permite ver tarefas com pull requests abertas, inspecionar as operações e orientar uma sessão em andamento.

Neste exercício, você vai:

- explorar a página de agentes para acompanhar as tarefas do agente de nuvem.
- orientar uma sessão em andamento para solicitar outra funcionalidade.

## Cenário

Depois de atribuir o modo de alto contraste, a equipe também decidiu adicionar um modo claro. Como o agente já está alterando estilos e adicionando controles, oriente o trabalho para incluir o novo modo.

## Revise as tarefas do agente de nuvem

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Examine as tarefas tanto no painel principal quanto no painel à esquerda. Você deverá ver as tarefas atribuídas ao Copilot, incluindo:
   - atualizar a documentação da base de código.
   - gerar APIs para modificar jogos.
   - adicionar um modo de alto contraste ao site.
3. Selecione uma tarefa em execução e examine as operações realizadas pelo Copilot. Elas podem incluir:
   - obter o código do repositório.
   - criar o ambiente de trabalho.
   - configurar servidores MCP.
   - executar várias etapas para concluir a tarefa atribuída.
4. Examine o painel de pull request à direita para acompanhar a PR e os arquivos alterados.

> [!NOTE]
> As etapas exibidas variam conforme o estado do trabalho e a abordagem do Copilot.

## Oriente o agente de nuvem

Peça ao Copilot que adicione um controle de modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão relacionada ao modo de alto contraste. O título depende do nome escolhido pelo Copilot e do estado do trabalho.

   ![Sessão de acessibilidade no painel de agentes](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até que a configuração seja concluída e o agente comece a trabalhar. Você verá mensagens semelhantes às mostradas abaixo.
3. Na caixa **Steer active session while Copilot is working**, envie:

   ```plaintext
   Enquanto trabalhamos no modo de alto contraste, vamos adicionar também um modo claro. Deve haver também um controle para esse modo, que permita aos usuários selecionar o modo de exibição desejado.
   ```

   ![Tarefa do agente de nuvem com a caixa de orientação da sessão ativa destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Confirme que o Copilot reconhece o pedido e o inclui no fluxo de trabalho.

## Deixe o Copilot trabalhar

Assim como antes, o Copilot trabalhará na tarefa atualizada. Ele incorporará a nova solicitação ao fluxo depois de concluir a etapa que estava executando quando você enviou a mensagem.

Isso levará alguns minutos; aproveite o intervalo para refletir sobre o que aprendeu e explorou até aqui.

## Resumo e próximas etapas

Esta lição apresentou a página de agentes do Copilot, seu centro para acompanhar e orientar tarefas do agente de nuvem do GitHub Copilot. Nela, você pode acompanhar todas as tarefas ativas e concluídas, revisar o trabalho em andamento e redirecionar uma tarefa para ajustar o escopo ou fornecer outras orientações.

Você explorou estes conceitos:

- explorou o centro de controle e a página de agentes para acompanhar tarefas do agente de nuvem.
- redirecionou uma sessão em andamento para solicitar outra funcionalidade.

Enquanto o Copilot conclui o trabalho nos recursos de acessibilidade, [itere sobre as pull requests criadas pelo Copilot][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Itere sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
