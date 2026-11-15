---
title: "Exercício 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] permitem criar assistentes de IA especializados em tarefas ou domínios do desenvolvimento. Arquivos Markdown em `.github/agents` fornecem instruções, práticas recomendadas, padrões de código e conhecimento específico. As equipes podem codificar sua experiência em agentes reutilizáveis de acessibilidade, segurança ou testes.

Os agentes são definidos por arquivos Markdown em `.github/agents` ou globalmente em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos `name` e `description`, seguido de um prompt Markdown que define o comportamento e as instruções.

### Agentes personalizados e skills de agente

Agentes personalizados e [skills de agente][agent-skills-concept] são definidos principalmente em arquivos Markdown. Uma distinção útil: o **agente personalizado** é o executor; as **skills** são ferramentas. Agentes têm sua própria janela de contexto e podem orquestrar skills ou outros agentes. Neste laboratório, um agente de acessibilidade revisará o site e poderá usar skills de fluxo de pull requests ou de testes.

> [!NOTE]
> Não há uma única maneira correta de criar um agente. Teste e itere conforme os requisitos do ambiente e do cenário.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Cenário

A Tailspin Toys está comprometida em tornar a plataforma acessível a todas as pessoas, independentemente de suas habilidades ou preferências visuais. Comentários recentes de usuários indicaram que algumas pessoas têm dificuldade para ler o tema escuro atual por causa do contraste insuficiente entre as cores do texto e do plano de fundo. Para resolver essa questão de acessibilidade, a equipe de design solicitou a implementação de um modo de alto contraste que possa ser ativado e desativado.

Como a acessibilidade é fundamental, queremos implementar esse recurso o mais rápido possível. Use um agente personalizado para gerar a funcionalidade.

Neste exercício, você vai explorar como os agentes personalizados são definidos e atribuir uma tarefa a um deles.

## Revise o agente personalizado de acessibilidade

O projeto já tem um agente de acessibilidade. Volte ao Codespace e revise o arquivo:

1. Abra `.github/agents/accessibility.md`.
2. Observe os campos `name` e `description` no frontmatter YAML.
3. Revise as responsabilidades, práticas recomendadas, exemplos de HTML, CSS e JavaScript e os problemas comuns.

> [!CAUTION]
> O frontmatter com `name` e `description` é obrigatório para agentes personalizados.

## Crie e atribua uma issue

A página de agentes é o local central para trabalhar com agentes no seu ambiente. Nela, você pode atribuir tarefas ao agente de nuvem do Copilot, acompanhar as tarefas e redirecionar o trabalho ou fornecer orientações adicionais. Atribua ao Copilot a tarefa de criar o modo de alto contraste.

1. Acesse seu repositório.
2. Selecione a guia **Issues** e depois **New issue**.
3. Selecione **Blank issue**.
4. Defina **Title** como `Adicione o modo de alto contraste ao site`.
5. Em **Description**, insira:

   ```plaintext
   Precisamos de um modo de alto contraste para o site. Deve haver um controle que o usuário possa ativar ou desativar. A configuração deve ser armazenada no armazenamento local do navegador.
   ```

6. Selecione **Create** e depois **Assign to Copilot** no painel à direita.
7. Selecione **Accessibility agent** na lista de agentes personalizados.

   ![Caixa de atribuição do agente de nuvem com o agente de acessibilidade destacado](../../../_images/ex5-select-custom-agent.png)

8. Selecione **Assign**.

9. O Copilot começará a trabalhar na tarefa em segundo plano.

## Resumo e próximas etapas

Esta lição apresentou os [agentes personalizados][custom-agents] do GitHub Copilot, assistentes especializados em tarefas ou domínios específicos. Com eles, você pode codificar a experiência e os padrões da equipe em agentes reutilizáveis que orientam o Copilot.

Você explorou estes conceitos:

- como agentes personalizados são definidos.
- como atribuir uma tarefa a um agente personalizado.

Enquanto o Copilot implementa o modo de alto contraste, [acompanhe e oriente a sessão][next-lesson] na página de agentes.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Preparar a organização para usar agentes personalizados][org-custom-agents]
- [Preparar a empresa para usar agentes personalizados][enterprise-custom-agents]

| [← Lição anterior: Agente de nuvem do GitHub Copilot][previous-lesson] | [Próxima lição: Acompanhe e gerencie agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
