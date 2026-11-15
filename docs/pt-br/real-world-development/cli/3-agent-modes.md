---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Use Plan para combinar uma abordagem, Autopilot para implementar a filtragem de uma issue e Interactive para revisar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Começamos adicionando uma pequena funcionalidade ao projeto. Alterações maiores exigem um processo mais robusto. O GitHub Copilot CLI foi criado para se integrar ao fluxo de trabalho da organização e ajudar a desenvolver o que é necessário da maneira adequada. Nesta lição, você inicia um fluxo comum de desenvolvimento com agentes: parte de uma issue, gera uma funcionalidade, verifica o código, valida o comportamento e, por fim, mescla a alteração.

> [!NOTE]
> Você usará a mesma conversa e o mesmo branch durante todo o fluxo da funcionalidade. Em geral, equipes usam branches ou PRs diferentes para tipos distintos de alteração, mas faremos um atalho para focar nos conceitos principais.

Nesta lição, você vai:

- iniciar uma conversa do Copilot a partir de uma issue do GitHub.
- definir requisitos no modo Plan.
- implementar a funcionalidade no modo Autopilot.
- revisar o código.
- validar manualmente a funcionalidade em um navegador encaminhado.

Ao continuar este trabalho, você atualizará as instruções do repositório, personalizará a skill existente `quality-checks`, adicionará validação MCP, criará um agente de QA e abrirá a PR da funcionalidade.

## Cenário

O catálogo da Tailspin Toys está crescendo, e os visitantes precisam filtrar os jogos por categoria e editora. A issue do backlog descreve a funcionalidade, mas detalhes como combinar categorias precisam ser acordados antes de escrever código. Use o modo Plan para decidir esses pontos e, em seguida, autorize uma implementação com escopo definido no modo Autopilot.

## Contexto

A introdução de agentes de programação com IA ao fluxo de desenvolvimento não muda os fundamentos. Na prática, eles ficam ainda mais importantes. Um fluxo comum é:

1. Começar por uma issue registrada que descreve o trabalho.
2. Planejar o que precisa ser criado.
3. Desenvolver e revisar o código.
4. Executar os testes para validar o código.
5. Validar manualmente a nova funcionalidade.
6. Criar uma pull request (PR).
7. Depois que o código for revisado e as verificações de integração contínua forem aprovadas, mescle-o.

> [!NOTE]
> Os detalhes variam conforme a equipe e a organização, mas a maioria dos fluxos segue uma variação dessas etapas.

Seguir essa abordagem ajuda a garantir que o código gerado por IA atenda aos requisitos e passe pelo mesmo processo de revisão do código escrito manualmente.

## Modos de conversa

O **modo de conversa** controla o nível de autonomia do agente. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre os modos:

- **Interactive**: você e o agente trabalham em conjunto. O agente sugere alterações e aguarda sua orientação antes de continuar.
- **Plan**: o agente cria primeiro um plano e não pode editar arquivos do projeto.
- **Autopilot**: o agente trabalha de forma autônoma, escreve código, executa testes e itera até considerar a tarefa concluída.

Comece no modo Plan, revise o plano e depois use Autopilot para implementá-lo.

## Comece pela issue

Antes de iniciar o trabalho de filtragem, volte ao Codespace e deixe o repositório e o terminal prontos.

1. Volte ao Codespace. Se estiver parado, reinicie-o.
2. Confirme que a PR das avaliações por estrelas foi mesclada.
3. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Atualize `main` e crie um branch para a filtragem:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicie o Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pressione <kbd>Tab</kbd> duas vezes para abrir a guia **Issues**.
7. Pressione <kbd>A</kbd> para exibir todas as issues.
8. Use as setas para destacar a issue **Allow users to filter games by category and publisher**.
9. Pressione <kbd>C</kbd> para adicionar a issue ao prompt e voltar à guia **Session**.

Observe que o prompt começa com `#7` (ou um número semelhante). O caractere `#` permite adicionar ao contexto uma issue ou pull request do GitHub.

## Planeje a filtragem

O planejamento permite definir a abordagem antes de delegar ao Copilot a implementação de uma funcionalidade ou tarefa. É recomendável reservar um tempo para planejar trabalhos complexos. Alterne para o modo Plan e peça que o Copilot crie o plano.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar para o modo Plan. Confirme que o indicador abaixo do prompt mostra **Plan**.
2. Depois da referência à issue adicionada na etapa anterior, digite:

   ```plaintext
   Crie um plano para implementar esta funcionalidade.
   ```

O Copilot explora o projeto e determina a melhor abordagem.

3. O Copilot pode perguntar como a filtragem deve funcionar. Responda de acordo com suas preferências.
4. Quando o plano estiver pronto, pressione <kbd>Control</kbd>+<kbd>E</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandi-lo.
5. Percorra o plano e revise-o.
6. Peça ao Copilot que revise qualquer parte que não corresponda às suas decisões.

## Aprove o Autopilot

Com o plano escrito e revisado, implemente-o usando o Autopilot. Esse modo permite que o Copilot itere até considerar o problema resolvido.

1. Selecione **Accept plan and build on autopilot (recommended)** ou a opção com rótulo semelhante na versão instalada.
2. Confirme que o indicador abaixo do prompt mostra **Autopilot**.
3. Acompanhe o Copilot enquanto ele segue o plano, gera código e executa testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, portanto revise o plano antes. Se o Copilot informar que faltam dependências ou que há conflito de porta, resolva o problema de configuração antes de considerar as verificações concluídas.

## Revise e verifique a implementação

Revise o código e execute o site para confirmar que tudo funciona antes da mesclagem.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar no modo Interactive. Confirme que o indicador não mostra **Plan** nem **Autopilot**.
2. Digite `/diff` e inspecione a implementação da filtragem e os testes.
3. Compare o resultado com a issue e com as decisões do planejamento.
4. Depois da revisão, pressione <kbd>Esc</kbd> para sair da visualização do diff.
5. Revise a saída das verificações do projeto e peça ao Copilot que resolva as falhas.

## Explore a nova funcionalidade

Agora que o código parece correto, inicie o aplicativo e abra o site pela porta encaminhada do Codespaces.

1. Peça ao Copilot que inicie o aplicativo:

   ```plaintext
   Inicie o aplicativo para que eu possa testar a filtragem no navegador. Informe a URL e deixe o servidor em execução.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Teste os filtros por categoria, por editora e as combinações acordadas no plano.
4. Confirme que a redefinição e o comportamento sem resultados correspondem à issue e às suas decisões.
5. Volte ao Codespace e peça ao Copilot que pare o servidor de desenvolvimento iniciado.

## Resumo e próximas etapas

Você usou diferentes modos de conversa para criar e revisar uma funcionalidade. Nesta lição, você:

- iniciou uma conversa do Copilot a partir de uma issue do GitHub.
- definiu requisitos no modo Plan.
- implementou a funcionalidade no modo Autopilot.
- revisou o código.
- validou manualmente a funcionalidade em um navegador encaminhado.

Em seguida, aprofunde-se na geração de código e garanta que ela siga as práticas documentadas [usando instruções personalizadas][next-lesson].

## Recursos

- [Autopilot no GitHub Copilot CLI][autopilot]
- [Referência de comandos do Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
