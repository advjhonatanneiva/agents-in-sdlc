---
title: "Exercício 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot permitem criar assistentes de IA especializados em tarefas ou domínios do fluxo de desenvolvimento. Arquivos Markdown em `.github/agents` fornecem instruções, práticas recomendadas, padrões de código e conhecimento específico. As equipes podem codificar sua experiência em agentes reutilizáveis, como agentes de acessibilidade que seguem os critérios [WCAG][wcag], agentes de segurança ou agentes de testes.

Os agentes são definidos por arquivos Markdown em `.github/agents` ou globalmente em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos `name` e `description`, seguido de um prompt Markdown que define o comportamento, a especialidade e as instruções.

### Agentes personalizados e skills de agente

Agentes personalizados e [skills de agente][agent-skills-concept] têm algumas semelhanças: ambos usam principalmente arquivos Markdown que ensinam a IA a executar tarefas. Uma distinção útil é que o **agente personalizado** é o executor e as **skills** são ferramentas.

Agentes personalizados têm sua própria janela de contexto e podem orquestrar skills e outros agentes. Neste laboratório, o agente de acessibilidade revisará e atualizará o site conforme as diretrizes e poderá usar skills de fluxo de pull requests ou de execução de testes.

> [!NOTE]
> Não há uma única maneira correta de criar um agente. Teste e itere para encontrar o que funciona em cada ambiente e cenário.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Cenário

A Tailspin Toys quer tornar a plataforma acessível a todos. Usuários relataram dificuldade para ler o tema escuro por causa do contraste insuficiente. A equipe de design solicitou um modo de alto contraste que possa ser ativado e desativado. Use um agente personalizado para implementar a funcionalidade.

Neste exercício, você vai revisar um agente de acessibilidade existente e usá-lo no Copilot Chat.

## Revise o agente personalizado de acessibilidade

O projeto já tem um agente para acessibilidade.

1. Volte ao Codespace e crie um branch separado para o trabalho:

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. Abra `.github/agents/accessibility.md`.
3. Observe os campos `name` e `description` no frontmatter YAML.
4. Revise as responsabilidades, práticas de acessibilidade, exemplos de HTML, CSS e JavaScript e os problemas comuns descritos no arquivo.

> [!CAUTION]
> O frontmatter com `name` e `description` é obrigatório para agentes personalizados.

## Use o agente personalizado no Copilot Chat

O VS Code mostra os agentes personalizados de `.github/agents` na lista de agentes na parte inferior da visualização Copilot Chat. Você pode selecionar um agente personalizado para limitar uma sessão de chat às instruções e ferramentas desse agente.

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao Codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. A seleção de modo e modelo varia conforme o exercício; cada etapa indica a opção relevante.

1. Volte ao Codespace, abra o Copilot Chat e selecione **New Chat**.
2. Na lista de agentes, selecione **Agent**, se ainda não estiver selecionado.
3. Abra a lista de agentes na parte inferior do chat e selecione **Accessibility agent**.
4. Envie este prompt:

   ```plaintext
   Adicione ao site um modo de alto contraste com um controle que o usuário possa ativar ou desativar. A configuração deve persistir após recarregar a página, usando o armazenamento local do navegador.
   ```

5. Revise as alterações propostas à medida que o Copilot explora e modifica os arquivos do projeto.
6. Acompanhe o indicador **Files changed** para ver os arquivos alterados.

> [!NOTE]
> O processo pode levar alguns minutos. O Copilot pode alterar componentes Astro, CSS e testes relacionados. Você revisará e orientará o trabalho no próximo exercício.

## Resumo e próximas etapas

Esta lição explorou [agentes personalizados][custom-agents] do GitHub Copilot, assistentes de IA especializados em tarefas ou domínios. Com agentes personalizados, você pode codificar a experiência e os padrões da equipe em agentes reutilizáveis que orientam o Copilot.

Você explorou estes conceitos:

- como agentes personalizados são definidos.
- como usar um agente personalizado no modo de agente do Copilot Chat.

Em seguida, [acompanhe e oriente o trabalho do agente][next-lesson], revise as alterações e acrescente um controle para o modo claro à mesma sessão.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Criar agentes personalizados no IDE][creating-custom-agents-ide]
- [Agentes personalizados no VS Code][custom-agents-vscode]
- [Configuração de agentes personalizados][custom-agents-config]
- [Agentes personalizados no awesome-copilot][awesome-copilot-agents]

| [← Lição anterior: Teste sua funcionalidade com o servidor Playwright MCP][previous-lesson] | [Próxima lição: Acompanhe e gerencie agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
