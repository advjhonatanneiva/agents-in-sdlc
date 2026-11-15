---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Modo de agente →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa deve ser feita de uma maneira específica — ou há informações que o Copilot precisa conhecer — disponibilize esse contexto. Os [arquivos de instruções][instruction-files] fornecem orientações sobre a geração de código.

Neste exercício, você vai:

- explorar como o contexto do projeto, as diretrizes de código e os padrões de documentação chegam ao Copilot por meio das instruções do repositório e de arquivos com escopo de caminho.
- gerar a primeira parte dos dados para filtragem, um helper de editoras, usando as instruções atuais.
- adicionar um padrão geral ao `.github/copilot-instructions.md`.
- repetir o prompt e observar o código gerado adotar o novo padrão.
- confirmar a atualização das instruções e o helper de filtragem em `main` para que o Copilot use a orientação no próximo exercício.

> [!CAUTION]
> O código gerado pode não seguir todos os padrões definidos. O Copilot é não determinístico. O objetivo é observar a tendência após atualizar as instruções, não obter resultados idênticos caractere por caractere.

## Arquivos de instruções

### Cenário

A Tailspin Toys tem diretrizes de desenvolvimento: a camada de dados precisa de testes unitários; a interface deve usar modo escuro e ter aparência moderna; o código deve incluir comentários TSDoc; e cada arquivo deve começar com comentários que expliquem sua finalidade. Use arquivos de instruções para orientar o Copilot.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências sobre estilo e requisitos. Elas podem especificar convenções, bibliotecas e comentários desejados, para todo o repositório ou para tipos específicos de arquivo.

Há dois tipos de arquivos:

- `.github/copilot-instructions.md` é enviado ao Copilot em todas as solicitações do repositório e deve conter contexto e orientações gerais do projeto.
- `.github/instructions/*.instructions.md` fornece diretrizes para linguagens ou tarefas específicas, como TypeScript, Astro, componentes de interface ou testes.

> [!NOTE]
> No IDE, os arquivos de instruções são usados para gerar código no Copilot Chat, não para complementações de código ou sugestões de próxima edição. O Copilot Chat, o Copilot CLI e o agente de nuvem usam instruções do repositório e arquivos `*.instructions.md` com frontmatter `applyTo`. O Copilot também oferece suporte a outros padrões, como AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar instruções

- Mantenha `copilot-instructions.md` focado nas orientações gerais do projeto.
- Use `*.instructions.md` para tipos de arquivo ou tarefas específicas.
- Escreva instruções claras em linguagem natural e inclua exemplos do que o código deve ou não deve fazer.

Não há uma única maneira correta de criar arquivos de instruções; experimente e descubra o que funciona para o projeto.

> [!TIP]
> O projeto inclui instruções para tarefas como [alterações de interface][ui-instructions] e [Astro][astro-instructions]. O Copilot também pode ajudar a gerar arquivos de instruções. Cada ambiente oferece essa opção de forma diferente, como **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI. Para modelos, explore o [awesome-copilot][awesome-copilot].

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explore os arquivos de instruções do projeto

Leia os arquivos fornecidos pelo repositório — um `copilot-instructions.md` e vários `*.instructions.md` — no editor ou na interface Web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Observe a descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**, incluindo a orientação de **GitHub Actions Workflows**.
3. Explore `.github/instructions`, que contém orientações para Astro, a camada de dados Drizzle, testes e outros tópicos.
4. Abra `.github/instructions/unit-tests.instructions.md` e observe o campo `applyTo`, que define um glob relativo à raiz do repositório para determinar os arquivos correspondentes.
5. Observe as instruções específicas para testes unitários.
6. Abra `.github/instructions/drizzle.instructions.md` e examine os links para outros arquivos de instrução e arquivos do projeto. Esses caminhos são relativos ao arquivo de instrução.

> [!NOTE]
> A seção **Code formatting requirements** descreve os padrões do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará comentários TSDoc e cabeçalhos de arquivo.

## Crie um branch para as alterações

Crie um branch separado para o trabalho.

1. Volte ao Codespace do exercício anterior.
2. Abra um terminal com <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crie e selecione um branch:

   ```bash
   git checkout -b custom-instructions
   ```

## Use o Copilot Chat antes de atualizar as instruções

Antes de começar, volte ao Codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam; cada etapa indica a configuração necessária.

Para observar o efeito das instruções, envie um prompt com o arquivo atual. Depois, atualize-o e envie o mesmo prompt novamente.

1. Feche as guias do editor abertas em exercícios anteriores para limitar o contexto.
2. Abra `src/lib/publishers.ts` para indicar onde o helper deve ficar.
3. Na lista de agentes da visualização Chat, selecione **Agent**.
4. Envie o prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Observe o Copilot explorar o projeto e aplicar as alterações, que podem incluir o helper e os testes.
6. O helper deve receber um cliente `db` como primeiro argumento e retornar uma matriz tipada de editoras, conforme as convenções da camada de dados em `.github/instructions/drizzle.instructions.md`.
7. Observe que o código proposto não inclui comentários TSDoc nem cabeçalho de arquivo.

> [!CAUTION]
> Como o Copilot é probabilístico, ele pode gerar comentários mesmo sem a instrução. Tudo bem: o objetivo é observar a melhora de consistência após atualizar as instruções.

## Adicione um padrão ao repositório

Acrescente ao `.github/copilot-instructions.md` orientações gerais de documentação no código.

1. Reabra `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, próxima à linha 27.
3. Acrescente estas linhas logo abaixo dos padrões existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve o arquivo.

> [!TIP]
> Use `.github/copilot-instructions.md` para padrões gerais e `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas.

## Repita o prompt e observe a alteração

1. Volte ao Copilot Chat e selecione **New Chat**.
2. Selecione `src/lib/publishers.ts` para dar foco ao arquivo.
3. Envie o mesmo prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observe que o arquivo proposto começa com um comentário semelhante a:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observe que a função proposta agora inclui um comentário TSDoc semelhante a:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Você orientou o Copilot a seguir um novo padrão e aplicá-lo ao código que servirá de base para o próximo exercício.

## Confirme as instruções e envie o branch

Gerencie os arquivos de instruções pelo controle do código-fonte, como qualquer outro arquivo do projeto.

1. Abra um terminal com <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
2. Confirme que as instruções e o helper foram alterados:

   ```bash
   git status
   ```

3. Prepare e confirme as instruções e os arquivos alterados pelo Copilot:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envie o branch:

   ```bash
   git push -u origin custom-instructions
   ```

## Crie e mescle uma pull request

Com o branch publicado, crie uma PR associada à issue sobre os padrões de documentação do backlog. Os recursos do GitHub já estão conectados ao projeto; na primeira utilização, o VS Code pode pedir que você entre no GitHub.

1. Abra o Copilot Chat e selecione o agente **Agent**.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a visualização Copilot Chat.
3. Envie o prompt:

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Quando solicitado, revise o comando e selecione **Allow** para permitir ações do GitHub.
5. Depois da criação da PR, peça ao Copilot que a mescle e volte para `main`:

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. Revise os comandos e selecione **Allow** quando solicitado a permitir ações do GitHub ou comandos do shell.

## Resumo e próximas etapas

Você explorou como o Copilot obtém contexto dos arquivos de instruções e usou o Copilot Chat no VS Code para testar a geração de código antes e depois de adicionar um padrão. Também confirmou as instruções e o helper em `main`. Em seguida, use o [modo de agente][next-lesson] para que o Copilot implemente uma funcionalidade em vários arquivos.

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para instruções personalizadas][instructions-best-practices]
- [Cinco dicas para escrever instruções melhores para o Copilot][copilot-instructions-five-tips]
- [Instruções pessoais para o GitHub Copilot][personal-instructions]
- [Awesome Copilot — coleção de arquivos de instruções][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
