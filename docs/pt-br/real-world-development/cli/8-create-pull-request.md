---
title: "Lição 8 - Crie e mescle a PR da funcionalidade"
description: "Revise a filtragem, as instruções, a skill, o perfil de QA e os testes; depois crie uma PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

A implementação da filtragem, as instruções atualizadas, a skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em um único branch. Revise tudo em conjunto e abra uma pull request (PR). Você mesmo mesclou a PR das avaliações por estrelas; desta vez, deixe o **Agent Merge** gerenciar o processo.

> [!NOTE]
> Em geral, dividiríamos a funcionalidade, as atualizações de instruções, a skill e o agente de QA em algumas PRs. Para simplificar o workshop, mantivemos todo o fluxo de filtragem e qualidade em uma conversa e um branch.

Nesta lição, você vai:

- conhecer o Agent Merge e como ele automatiza o ciclo de vida da mesclagem.
- inspecionar toda a alteração da funcionalidade e as evidências de verificação.
- criar a PR de filtragem.
- habilitar o Agent Merge somente depois da revisão e confirmar a mesclagem.

## Cenário

Durante o fluxo de filtragem, você usou o Copilot para planejar, implementar e verificar uma funcionalidade. Agora a Tailspin Toys quer automatizar o trabalho restante da PR, mantendo a autorização de mesclagem sob controle do desenvolvedor.

## Conheça o Agent Merge

O **Agent Merge** automatiza o trabalho restante para concluir uma pull request. Quando habilitado, o Copilot resolve o que estiver bloqueando a PR — corrigindo verificações de integração contínua (CI) com falha, respondendo a comentários de revisão e fazendo rebase quando necessário — e então habilita a mesclagem automática do GitHub quando o repositório permite.

Até agora, você selecionou **Merge pull request** manualmente. O Agent Merge pode assumir essa responsabilidade. Revise o trabalho e confirme que está pronto antes de habilitar o recurso.

## Use o Agent Merge para gerenciar a PR

Revise o código, crie a PR e autorize o Agent Merge a gerenciar as etapas seguintes.

1. Volte ao Codespace.
2. Abra a caixa de diálogo do agente digitando `/agent`.
3. Na lista, selecione **Default** e pressione <kbd>Enter</kbd>.
4. Crie uma PR com o comando `/pr create`.
5. Ative o Agent Merge com `/pr agentmerge`.
6. O Copilot acompanhará a integração contínua na PR e fará a mesclagem quando tudo for aprovado.
7. Confirme que o Copilot exibiu uma mensagem semelhante a "PR #14 was squash-merged successfully."

> [!IMPORTANT]
> O Agent Merge não ignora aprovações obrigatórias, proteção de branch, filas de mesclagem, configurações do repositório nem permissões ausentes. Se houver um bloqueio, leia o motivo informado e conclua manualmente a mesclagem revisada quando o repositório permitir.

## Resumo e próximas etapas

Você automatizou várias etapas do desenvolvimento, incluindo geração de código, testes, validação e o processo da pull request. Nesta lição, você:

- conheceu o Agent Merge e como ele automatiza o ciclo de vida da mesclagem.
- inspecionou a alteração completa da funcionalidade e as evidências de verificação.
- criou a PR de filtragem.
- habilitou o Agent Merge somente depois da revisão e confirmou que a PR foi mesclada.

Em seguida, [explore outros comandos slash úteis do Copilot CLI][next-lesson] para contexto, modelos, compartilhamento e delegação opcional ao agente de nuvem.

## Recursos

- [Gerenciar pull requests com o Copilot CLI][manage-prs]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
