---
title: "Lição 10 - Encerramento e próximas etapas"
description: "Recapitule o fluxo de trabalho com o Copilot CLI, as duas pull requests, as personalizações reutilizáveis e outros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Você usou o GitHub Copilot CLI em um fluxo contínuo de trabalho na Tailspin Toys. Você:

- preparou um Codespace, instalou o Copilot CLI, explorou o projeto e encontrou a issue de filtragem.
- adicionou avaliações por estrelas, revisou o resultado em um navegador encaminhado e mesclou manualmente sua primeira pull request (PR).
- começou pela issue de filtragem, definiu a abordagem no modo Plan, implementou no modo Autopilot e revisou no modo Interactive.
- orientou o agente com instruções personalizadas, personalizou a skill existente `quality-checks` e a usou para executar testes unitários, lint e verificações de tipos.
- adicionou o servidor Playwright Model Context Protocol (MCP) e o usou para explorar a filtragem em um navegador real.
- criou e selecionou um agente personalizado de garantia de qualidade (QA) para avaliar requisitos, cobertura, resultados da skill e evidências do navegador.
- revisou toda a alteração de filtragem e autorizou o Agent Merge para a PR.
- explorou comandos slash para contexto, modelos, compartilhamento e delegação opcional à nuvem.

## O que você entregou

O workshop tem duas etapas de PR, cada uma em seu próprio branch baseado em `main` atualizado:

1. **Avaliações por estrelas:** exiba `starRating` existente e um estado explícito para jogos sem avaliação nos cartões.
2. **Filtragem e fluxo de qualidade:** implemente a filtragem, atualize as instruções e aplique-as à funcionalidade, personalize o relatório de `quality-checks`, crie um perfil de QA e inclua os testes relacionados.

Do planejamento da filtragem até a abertura da PR, você usou a mesma conversa e o mesmo branch. Reunimos esse trabalho em uma PR para simplificar o workshop.

## Diferentes tipos de verificação

Você verificou o código de várias formas: testes automatizados, inspeção manual no navegador e exploração do navegador pelo Copilot via MCP. A skill `quality-checks` executou testes unitários, lint e verificações de tipos, apresentando os resultados no novo formato. Antes da PR, o QA reuniu esses resultados com a revisão dos requisitos e da cobertura dos testes.

Os testes adicionados devem preencher lacunas reais; uma execução de QA que conclua que nenhum teste novo é necessário também pode estar correta. Revise o código e as evidências antes de autorizar a mesclagem e atualize as evidências afetadas após alterações.

## Práticas recomendadas

O contexto e as ferramentas fornecidos ao Copilot moldam o trabalho. Neste workshop, você atualizou instruções, personalizou uma skill, criou um perfil de QA e configurou um servidor MCP. Reutilize essas personalizações entre conversas e ajuste-as conforme as necessidades da equipe. Instruções definem padrões, skills descrevem tarefas repetíveis, agentes personalizados definem funções especialistas e servidores MCP conectam ferramentas externas. Revise as alterações e os resultados das ferramentas, não apenas o resumo do agente.

Escolha o **modo e o modelo** adequados à tarefa. Use **Plan** para pensar na abordagem antes de começar, **Interactive** para acompanhar alterações focadas e **Autopilot** para tarefas com escopo bem definido. Escolha um modelo mais rápido para edições rotineiras e um modelo mais capaz para trabalhos complexos.

O contexto continua sendo tão importante quanto a infraestrutura. Descrever com clareza *o que* você quer criar, *por que* e *como* altera significativamente o resultado.

## Explore mais

Você concluiu o fluxo principal. Alguns recursos adicionais do CLI que vale a pena conhecer:

- `/review` para pedir ao agente de revisão de código que analise as alterações.
- `/rubber-duck` para conversar sobre um problema e obter outra perspectiva.
- `/fleet` para coordenar subtarefas independentes em paralelo.
- `/worktree` para isolar uma tarefa.
- `/delegate` para enviar uma tarefa ao agente de nuvem do Copilot.

## Próximas etapas

A melhor maneira de melhorar com qualquer ferramenta é continuar usando-a. Use-a em código de produção, projetos pessoais ou naquele pequeno aplicativo que você quer criar há anos. Compartilhe o que aprendeu com a equipe e aprenda com os outros. E, como sempre, explore a documentação.

Para ampliar a Tailspin Toys no terminal, continue com a série opcional [Foundry Backer Concierge][foundry]. Para comparar outros ambientes, explore os workshops de [VS Code][vscode], do [aplicativo GitHub Copilot][app] ou do [agente de nuvem do Copilot][cloud].

## Recursos

- [Sobre o GitHub Copilot CLI][about-cli]
- [Referência de comandos do Copilot CLI][cli-reference]
- [Personalizar o Copilot CLI][customize-cli]
- [Gerenciar pull requests com o Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
