---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

É provável que quase todas as organizações enfrentem dívida técnica. Isso pode incluir problemas de segurança não resolvidos, código legado que precisa de atualização ou solicitações de funcionalidades que ficaram no backlog por falta de tempo para implementá-las. O agente de nuvem do GitHub Copilot foi criado para executar tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Ao concluir o trabalho, ele gera uma pull request de rascunho para revisão de um desenvolvedor. Isso permite delegar tarefas tediosas, acelerar o desenvolvimento e liberar os desenvolvedores para se concentrarem em questões mais amplas.

Neste exercício, você explorará:

- como personalizar o ambiente para gerar código.
- como garantir que as operações sejam executadas com segurança.
- a importância de issues com escopo claro.
- como atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem dívida técnica para resolver. Os prestadores de serviço contratados inicialmente para criar a primeira versão do site deixaram a documentação em estado inadequado — na verdade, você verá que ela está completamente ausente. Como primeiro passo, a equipe quer comentários TSDoc em todas as funções exportadas do aplicativo.

Além disso, a equipe de design quer melhorar a descoberta de jogos. Cada página de detalhes deve mostrar outros jogos da mesma categoria para que as pessoas continuem navegando. Ainda não é necessário um resultado refinado; a equipe quer algo que possa usar para testes de aceitação da experiência do usuário (UX). Essa tarefa está bloqueada no momento, mas há issues de prioridade maior. São bons exemplos de tarefas que podem ser adiadas e atribuídas ao agente de nuvem do Copilot, que trabalha de forma assíncrona enquanto o desenvolvedor cuida de outras atividades e depois revisa o resultado.

## Conheça o agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) executa tarefas em segundo plano de forma semelhante a um desenvolvedor. Assim como no trabalho com um desenvolvedor, há várias maneiras de atribuir tarefas, inclusive [atribuir uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois da atribuição, o Copilot cria uma pull request de rascunho para acompanhar o progresso, configura um ambiente e começa a tarefa. Você pode inspecionar a sessão enquanto o trabalho está em andamento ou depois de concluído. Quando a solução estiver pronta para revisão, o Copilot marcará você na pull request.

## A importância de instruções bem delimitadas

Não há mágica no GitHub Copilot: você não pode deixar de pensar. Mesmo tarefas aparentemente simples ficam complexas quando examinadas em detalhe; por isso, [defina com cuidado o escopo das tarefas do agente de nuvem][cloud-agent-best-practices]. Trate-o como um parceiro de programação com IA: trabalhe por etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a IA generativa.

## Instruções personalizadas neste repositório

Exercícios anteriores apresentaram instruções personalizadas e como elas orientam o Copilot. Se você já fez o exercício prático sobre instruções personalizadas, viu `.github/copilot-instructions.md` em ação; caso contrário, este é um bom momento para examiná-lo. Antes de atribuir trabalho ao agente de nuvem, faça uma leitura dos arquivos de instruções fornecidos no repositório, sem alterá-los, para depois identificar seus efeitos.

Abra os arquivos a seguir na interface Web do GitHub ou em um Codespace já iniciado:

- `.github/copilot-instructions.md`: examine **Code standards**, em especial as convenções de TypeScript e comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observe o frontmatter `applyTo`, que aplica as instruções a arquivos `**/*.test.ts`.

Ao atribuir a issue `O código está sem documentação` ao agente de nuvem na próxima seção, observe na pull request os comentários TSDoc, as convenções de TypeScript e os cabeçalhos de comentário. Esses elementos são orientados pelos arquivos de instruções. Voltaremos a esse ponto ao revisar as PRs em um exercício posterior.

## Configure o ambiente de desenvolvimento do agente de nuvem

A criação de código normalmente exige um ambiente específico e a execução de scripts de configuração para garantir que tudo esteja em boas condições. Isso também se aplica às tarefas atribuídas ao Copilot, que trabalha de forma semelhante a um engenheiro de software (SWE).

O agente de nuvem usa o [GitHub Actions][github-actions] para configurar o ambiente em que trabalhará. Você pode personalizar esse ambiente criando um [fluxo de configuração especial][setup-workflow] no arquivo `.github/workflows/copilot-setup-steps.yml`, que será executado antes de o agente começar a trabalhar. Assim, ele terá acesso às ferramentas e dependências de desenvolvimento necessárias. Esse fluxo já foi configurado para este laboratório a fim de facilitar as atividades e a experiência prática; ele disponibiliza Node.js, dependências do projeto, binários de navegador para testes de ponta a ponta e o banco de dados SQLite migrado e populado do aplicativo Astro único:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões no nível mínimo necessário para *suas etapas*. O Copilot receberá seu próprio token para as operações dele.
    permissions:
      # Se quiser clonar o repositório como parte das etapas de configuração, por exemplo, para instalar dependências, será necessária a permissão `contents: read`. Se não clonar o repositório nessas etapas, o Copilot fará isso automaticamente depois que elas forem concluídas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do front-end/aplicativo - Node.js (o aplicativo inteiro agora usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migre e preencha o banco de dados SQLite local para que compilações e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo se parece com qualquer fluxo de trabalho do GitHub, mas tem alguns pontos importantes:

- Ele contém um único job chamado `copilot-setup-steps`, executado no GitHub Actions antes de o Copilot começar a trabalhar na pull request.
- O gatilho `workflow_dispatch` permite executar o fluxo manualmente na guia **Actions** do repositório. Isso é útil para verificar se ele funciona, sem esperar o Copilot iniciá-lo.

## Adicione documentação ao código

A documentação pode estar desatualizada ou completamente ausente em muitos projetos. Essa dívida técnica reduz a produtividade, dificulta a manutenção da base de código e torna mais difícil integrar novos desenvolvedores à equipe. O Copilot é eficaz na criação de documentação, por isso essa é uma issue adequada para atribuir ao agente de nuvem. Ele trabalhará em segundo plano para produzir a documentação necessária; em um exercício futuro, você voltará para revisar o resultado. Atribua ao agente a tarefa de adicionar comentários TSDoc às funções exportadas.

1. Em uma nova guia, acesse o repositório no github.com.
2. Selecione **Issues** e depois **New issue**.
3. Selecione **Blank issue**.
4. Defina **Title** como `O código está sem documentação`.
5. Em **Description**, insira:

   ```plaintext
   Nossa organização exige que funções e métodos tenham comentários TSDoc quando forem úteis. Infelizmente, as atualizações recentes não seguiram esse padrão. Precisamos atualizar o código existente para incluir comentários quando eles ajudarem a esclarecer o comportamento.
   ```

6. Selecione **Create** para criar a issue.
7. No painel à direita, selecione **Assign to Copilot** para abrir a caixa de atribuição.

   ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

8. Selecione **Assign**.

   ![Detalhes da atribuição do Copilot](../../../_images/ex4-assign-copilot-details.png)

9. Selecione a guia **Pull Requests**.
10. Abra a pull request recém-gerada, com título semelhante a `[WIP]: O código está sem documentação`. Se ela ainda não aparecer na lista, aguarde um ou dois minutos e atualize a janela do navegador.
11. Depois de alguns minutos, procure a lista de tarefas criada pelo Copilot.

> [!NOTE]
> A lista de tarefas do Copilot pode levar vários minutos para aparecer na PR. O Copilot está preparando o ambiente (executando o fluxo mencionado), analisando o projeto e definindo a melhor abordagem.

12. Revise a lista e as tarefas que o Copilot pretende executar.
13. Role a linha do tempo da pull request e procure uma atualização informando que o Copilot começou a trabalhar na issue.
14. Selecione o botão **View session**.

   ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

15. Percorra a sessão ao vivo e observe como o Copilot resolve o problema: explora o código e o estado atual, faz uma pausa para pensar e definir um plano e também cria código.

O trabalho provavelmente levará alguns minutos. Um dos objetivos principais do agente de nuvem é executar tarefas de forma assíncrona, liberando você para se concentrar em outras atividades. Aproveite esse recurso atribuindo outra tarefa ao agente e, em seguida, volte sua atenção para adicionar funcionalidades ao aplicativo.

## Adicione uma seção de jogos relacionados

Uma das vantagens do agente de nuvem do GitHub Copilot é dividir o trabalho: você pode se concentrar em um conjunto de tarefas enquanto ele cuida de outro. Embora adicionar uma seção de jogos relacionados para a equipe de design talvez não leve muito tempo, esse tempo pode ser usado em outras tarefas. Atribua essa tarefa ao agente de nuvem.

1. Volte ao repositório no github.com.
2. Selecione **Issues**, depois **New issue** e **Blank issue**.
3. Defina **Title** como `Exiba jogos relacionados na página de detalhes do jogo`.
4. Em **Description**, insira:

   ```markdown
   Queremos ajudar as pessoas a descobrir mais jogos, mostrando jogos relacionados em cada página de detalhes. A equipe de design quer explorar a experiência do usuário (UX) e fazer alguns testes de aceitação. Nossos requisitos são:

   - Adicione em `src/lib/` um helper de acesso a dados que retorne outros jogos da mesma categoria de um jogo, excluindo o próprio jogo.
   - Mostre na página de detalhes do jogo uma seção "Jogos relacionados" que use o helper.
   - Trate o caso em que um jogo não tenha jogos relacionados.
   - Crie testes unitários para o novo helper.
   - Antes de criar a pull request, confirme que todos os testes passam.
   ```

5. Selecione **Create** para criar a issue.
6. No painel à direita, selecione **Assign to Copilot** para abrir a caixa de atribuição.
7. Selecione **Assign**.

Pouco depois, você verá uma sequência de 👀 no primeiro comentário da issue, indicando que o Copilot começou a trabalhar.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot agora está trabalhando na segunda solicitação. O agente de nuvem funciona de forma semelhante a um engenheiro de software; não é necessário acompanhá-lo ativamente. Revise o resultado quando estiver pronto. Enquanto isso, volte sua atenção à criação e ao uso de agentes personalizados.

## Resumo e próximas etapas

Esta lição apresentou o [agente de nuvem do GitHub Copilot][copilot-agents]. Com ele, você pode atribuir issues ao Copilot para execução assíncrona. Use-o para resolver dívida técnica, criar funcionalidades ou ajudar na migração de código entre frameworks.

Você explorou estes conceitos:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam executadas com segurança.
- entender a importância de issues com escopo claro.
- atribuir issues ao Copilot.

Enquanto o agente trabalha em segundo plano, volte sua atenção à criação e ao uso de agentes personalizados. O [agente de nuvem do Copilot também pode usar servidores MCP][cloud-agent-mcp] e tem acesso a instruções personalizadas, que foram abordadas em módulos anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas do fluxo de configuração do agente de nuvem][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
