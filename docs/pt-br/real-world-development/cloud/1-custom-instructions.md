---
title: "Exercício 1 - Instruções personalizadas (agente de nuvem)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Agente de nuvem do Copilot →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Os [arquivos de instruções][instruction-files] fornecem orientação para que o Copilot entenda não só *o que* fazer, mas também *como* fazer.

Neste exercício, você vai explorar como o contexto do projeto, as diretrizes de código e os padrões de documentação chegam ao Copilot por meio das instruções do repositório e dos arquivos com escopo de caminho, além de adicionar um padrão geral a `.github/copilot-instructions.md`.

> [!NOTE]
> Diferentemente dos ambientes VS Code e CLI, você não fará uma comparação de prompt antes e depois. O agente de nuvem trabalha de forma assíncrona em issues do GitHub; você observará a influência das instruções ao revisar as pull requests geradas.

## Arquivos de instruções

### Cenário

A Tailspin Toys exige testes unitários na camada de dados, modo escuro e visual moderno na interface, comentários TSDoc no código e um bloco de comentários no início de cada arquivo que explique sua finalidade. Use instruções para orientar o Copilot.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor o estilo e os requisitos de programação. Elas podem orientar convenções, bibliotecas e comentários desejados, para todo o repositório ou para tipos específicos de arquivo. É uma forma poderosa de direcionar o Copilot a gerar sugestões e trechos de código mais relevantes.

- `.github/copilot-instructions.md` é um único arquivo enviado ao Copilot em **todas** as solicitações do repositório. Ele deve conter informações relevantes para a maioria das solicitações, como a stack, uma visão geral do que está sendo criado, práticas recomendadas e orientações gerais.
- Arquivos `.github/instructions/*.instructions.md` podem ser criados para tarefas ou tipos de arquivo específicos. Use-os para linguagens como TypeScript e Astro ou tarefas como criar componentes de interface e testes unitários.

> [!NOTE]
> No IDE, as instruções se aplicam à geração de código no Copilot Chat, não a complementações nem sugestões de próxima edição. O Copilot Chat, o Copilot CLI e o agente de nuvem usam instruções do repositório e arquivos `*.instructions.md` com frontmatter `applyTo`. Também há suporte a formatos como AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

Uma discussão completa sobre como criar arquivos de instruções está fora do escopo deste exercício, mas os exemplos do projeto mostram uma abordagem representativa:

- Mantenha `copilot-instructions.md` focado nas orientações gerais do projeto, como uma descrição do que está sendo criado, a estrutura do projeto e os padrões globais de código.
- Use `*.instructions.md` para instruções específicas de tipos de arquivo (testes unitários, componentes Astro, camada de dados) ou tarefas.
- Use linguagem natural e mantenha as orientações claras. Inclua exemplos de como o código deve ou não deve ser.

Não há uma maneira única de criar arquivos de instruções, assim como não há uma única maneira de usar IA. Experimente para descobrir o que funciona para o projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter um conjunto robusto de arquivos de instruções. Ao explorar os arquivos deste projeto, você encontrará orientações para várias tarefas, incluindo [atualizações de interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode gerar arquivos de instruções. Cada ambiente oferece essa opção de forma diferente, por exemplo, **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI. O exercício referente a cada ambiente indicará o recurso adequado.
>
> Para modelos e pontos de partida, explore o [awesome-copilot][awesome-copilot], um repositório com arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explore os arquivos de instruções do projeto

Antes de atribuir trabalho ao agente de nuvem, examine os arquivos existentes em modo somente leitura para identificar sua influência futura.

1. Abra `.github/copilot-instructions.md`.
2. Examine seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**, incluindo a orientação de **GitHub Actions Workflows**.
3. Explore `.github/instructions`, que contém orientações para Astro, a camada de dados Drizzle, testes e outros tópicos.
4. Abra `.github/instructions/unit-tests.instructions.md` e observe o campo `applyTo` no início do arquivo. Ele define um padrão glob relativo à raiz do repositório. Neste projeto, arquivos de teste TypeScript, como `**/*.test.ts`, correspondem a esse padrão.
5. Observe as instruções específicas para testes unitários.
6. Abra `.github/instructions/drizzle.instructions.md` e role até o final. Observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes do projeto. Assim, você pode dividir conjuntos grandes em arquivos reutilizáveis menores e indicar exemplos para o Copilot. Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.

> [!NOTE]
> A seção **Code formatting requirements** descreve os padrões, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará comentários TSDoc e cabeçalhos de arquivo.

## Adicione um padrão do repositório

Acrescente uma regra geral para comentários TSDoc e cabeçalhos de arquivo. O agente de nuvem lerá essa instrução ao trabalhar nas issues atribuídas mais adiante.

1. No terminal do Codespace, crie um branch:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. Reabra `.github/copilot-instructions.md`.
3. Localize **Code formatting requirements**, próxima à linha 27.
4. Adicione estas regras abaixo dos padrões existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. Salve o arquivo.

> [!TIP]
> Use `.github/copilot-instructions.md` para padrões gerais e `*.instructions.md` para tipos de arquivo e tarefas específicas.

## Confirme, envie e mescle a atualização

O agente de nuvem lê as instruções do branch de destino da issue. Como ele criará branches a partir de `main`, mescle as instruções em `main` para que sejam carregadas no próximo exercício.

1. Prepare e confirme a alteração:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envie o branch:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. No github.com, abra uma pull request de `custom-instructions-cloud` para `main` e mescle-a.

> [!TIP]
> Se preferir trabalhar diretamente em `main` neste workshop, ignore o branch e confirme a alteração em `main`. O uso do branch reflete o fluxo de um projeto real.

## Resumo e próximas etapas

Você explorou como o Copilot obtém contexto dos arquivos de instruções e adicionou um padrão ao `.github/copilot-instructions.md`. Em seguida, [atribua sua primeira issue ao agente de nuvem do Copilot][next-lesson].

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para instruções personalizadas][instructions-best-practices]
- [Cinco dicas para escrever instruções melhores para o Copilot][copilot-instructions-five-tips]
- [Awesome Copilot — coleção de arquivos de instruções][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
