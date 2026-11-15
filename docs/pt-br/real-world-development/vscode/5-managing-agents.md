---
title: "Exercício 5 - Acompanhe e gerencie agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Agentes personalizados][previous-lesson] |
|:--|

Quando você usa o GitHub Copilot no modo de agente, ele trabalha de forma autônoma: explora a base de código, propõe alterações, edita arquivos e executa comandos. Como esse trabalho ocorre em tempo real na sua máquina, o Copilot Chat no VS Code mostra cada chamada de ferramenta e edição de arquivo à medida que acontece. Você pode revisar cada diff no chat, aceitar ou rejeitar alterações individuais e orientar a conversa com prompts de acompanhamento sem sair do editor.

Neste exercício, você vai acompanhar o trabalho do agente de acessibilidade, revisar e aceitar as edições propostas e orientá-lo para acrescentar um controle de modo claro junto ao modo de alto contraste.

## Cenário

O agente personalizado de acessibilidade do [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Durante o trabalho, a equipe de design também solicita um modo claro. Oriente a sessão existente para ampliar a funcionalidade sem perder o contexto.

## Acompanhe o progresso do Copilot

Cada edição aparece no chat com um diff e os controles **Keep** e **Undo**.

1. Volte à visualização Copilot Chat do Exercício 4.
2. Acompanhe as chamadas de ferramentas, leituras de arquivos, edições e comandos de terminal.
3. Em cada diff, selecione **Keep** para aceitar a alteração ou **Undo** para desfazê-la e permitir outra tentativa.
4. Abra **Files changed** na parte superior do chat para revisar os arquivos alterados na sessão.
5. Aguarde o agente concluir o modo de alto contraste e apresentar um resumo.

> [!NOTE]
> Se uma ferramenta solicitar permissão, por exemplo, para executar um comando de terminal, o agente mostrará **Allow**. Aprove apenas comandos que você considere seguros.

## Oriente o agente para adicionar o modo claro

Continue a mesma conversa para que o agente preserve o contexto dos componentes, dos estilos e do padrão de armazenamento local que já criou.

1. Envie este prompt no Copilot Chat:

   ```plaintext
   Bom trabalho! Agora adicione um controle de modo claro junto ao controle de alto contraste. O modo claro deve seguir o mesmo padrão: persistir no armazenamento local e estar disponível na mesma área da interface. O usuário deve poder ativar o alto contraste e o modo claro de forma independente.
   ```

2. Revise as alterações propostas, selecionando **Keep** ou **Undo** em cada diff.
3. Se o agente seguir uma direção indesejada, selecione **Stop** ao lado da entrada do chat e envie um esclarecimento.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que começar de novo. Use prompts de acompanhamento para pedir ajustes ("simplifique o CSS — use variáveis CSS em vez de duplicar seletores"), corrigir o rumo ("não adicione um terceiro controle; integre o modo claro ao painel de configurações existente") ou ampliar o escopo ("adicione também atalhos de teclado para os dois controles").

## Revise o conjunto de alterações

Antes de confirmar o trabalho, revise tudo que o agente alterou.

1. Abra a visualização **Source Control** no VS Code.
2. Revise os arquivos alterados e alguns diffs. Confirme se há atributos ARIA, navegação por teclado, HTML semântico e persistência com armazenamento local.
3. Quando estiver satisfeito, prepare e confirme as alterações no painel Source Control. Você as publicará na próxima lição.

## Resumo e próximas etapas

Você usou o modo de agente do Copilot Chat para acompanhar e orientar uma sessão em andamento: revisou cada edição quando foi proposta, aceitou ou rejeitou alterações e enviou prompts de acompanhamento para ampliar o trabalho sem perder o contexto.

Nesta lição, você explorou:

- acompanhar o trabalho do agente em tempo real.
- aceitar e rejeitar edições individuais de arquivos.
- orientar uma sessão em andamento com prompts de acompanhamento.

Na [próxima lição][next-lesson], publique o trabalho de acessibilidade como uma pull request e revise as alterações produzidas pelo agente personalizado local durante o laboratório.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Itere sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
