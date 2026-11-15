---
title: "Exercício 3 - Teste a funcionalidade com o servidor Playwright MCP"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo de agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você acabou de criar a funcionalidade de filtragem com o modo de agente. Antes de abrir uma pull request, confirme que ela realmente funciona no navegador. Em vez de navegar pelo aplicativo manualmente, conecte o **servidor Playwright MCP** e deixe o Copilot controlar um navegador real para testar a funcionalidade. Depois, publique o branch e abra a PR.

Neste exercício, você vai:

- aprender o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam os recursos do Copilot.
- adicionar o servidor Playwright MCP ao workspace.
- pedir ao Copilot que use o servidor para testar manualmente a filtragem no navegador.
- publicar o branch e abrir uma pull request para o trabalho de filtragem.

## O que é o Model Context Protocol (MCP)?

O modo de agente fica muito mais poderoso quando pode acessar recursos além do editor. O Model Context Protocol (MCP) é o padrão que permite ao agente se comunicar com ferramentas e serviços externos.

![Diagrama mostrando o funcionamento interno do modo de agente e sua interação com contexto, LLM e ferramentas, incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que agentes de IA se comuniquem em tempo real com ferramentas e serviços externos. Assim, podem acessar informações atualizadas por meio de recursos e executar ações em seu nome por meio de ferramentas.

Esses recursos e ferramentas são acessados por um servidor MCP, que faz a ponte entre o agente de IA e os serviços externos e gerencia a comunicação (por exemplo, com APIs existentes ou ferramentas locais, como pacotes npm). Cada servidor representa um conjunto diferente de ferramentas e recursos disponíveis ao agente.

Alguns servidores MCP populares:

- **[Servidor MCP do GitHub](https://github.com/github/github-mcp-server)**: oferece APIs para gerenciar repositórios do GitHub e permite, por exemplo, criar repositórios, atualizar os existentes e gerenciar issues e pull requests.
- **[Servidor Playwright MCP](https://github.com/microsoft/playwright-mcp)**: oferece automação de navegador com Playwright, permitindo navegar por páginas, preencher formulários e selecionar botões.

Há muitos outros servidores MCP para diferentes ferramentas e recursos. O GitHub mantém um [registro MCP](https://github.com/mcp) para facilitar a descoberta e as contribuições ao ecossistema.

> [!CAUTION]
> Trate servidores MCP como qualquer outra dependência. Revise o código-fonte, verifique o publicador e considere as implicações de segurança antes de conceder acesso a recursos ou operações sensíveis.

## Revise a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste workspace.

1. Abra `.vscode/mcp.json` no Codespace.
2. Confirme que há um servidor `github` configurado:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

Essa entrada `github` vem com o modelo do projeto e permitiu que o Copilot lesse o backlog de issues no exercício anterior. Ela usa o [servidor MCP remoto do GitHub][remote-github-mcp-server], portanto não há nada para instalar localmente: o VS Code se conecta por HTTP e você se autentica no GitHub na primeira vez que o Copilot usa uma de suas ferramentas.

## Adicione o servidor Playwright MCP

O servidor Playwright MCP permite que o Copilot controle um navegador.

1. Em `.vscode/mcp.json`, adicione `playwright` junto com `github`:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. Salve o arquivo.

O VS Code inicia o servidor `playwright` localmente com `npx @playwright/mcp@latest`. A opção `--headless` executa o navegador sem uma janela visível, necessária no Codespace.

> [!NOTE]
> O projeto já usa Playwright nos testes de ponta a ponta. Se o Copilot informar que falta um navegador, peça que execute `npx playwright install chromium` e tente novamente.

## Inicie e confie no servidor

O VS Code inicia servidores MCP sob demanda e pede confirmação de confiança na primeira utilização.

1. Em `.vscode/mcp.json`, selecione **Start** acima da entrada `playwright` para iniciar o servidor e confirmar a configuração.
2. Se o VS Code solicitar confirmação de confiança, revise a configuração e autorize o servidor.

Iniciá-lo manualmente é opcional. Se você ignorar esta etapa, o VS Code iniciará o servidor quando o Copilot usar as ferramentas.

## Teste a filtragem

Com o servidor conectado e o aplicativo em execução, envie ao Copilot este prompt:

```text
Usando o servidor Playwright MCP, abra um navegador para o aplicativo em execução em http://localhost:4321 e verifique a nova funcionalidade de filtragem de jogos:

1. Go to the games page and note how many games are listed.
2. Acesse a página de jogos, anote quantos jogos aparecem e aplique um filtro de categoria. Confirme que a lista passa a mostrar somente jogos dessa categoria.
3. Limpe o filtro e aplique um filtro de editora. Confirme que a lista passa a mostrar jogos dessa editora.
4. Combine um filtro de categoria com um de editora e confirme que os resultados correspondem aos dois filtros.

Relate o que observou em cada etapa e destaque qualquer comportamento inesperado.
```

O Copilot abrirá um navegador, aplicará os filtros e relatará o que observou. Compare o resumo com os critérios de aceitação da issue. Se algo estiver errado, faça perguntas ou peça a correção antes de abrir a PR.

O servidor [Playwright MCP][playwright-mcp-server] fornece ao Copilot um navegador real. Em vez de você navegar pelo aplicativo para verificar o trabalho, o agente pode abrir uma página, navegar, aplicar filtros, ler os resultados e resumir o que observou. É uma forma rápida de confirmar o comportamento esperado sem sair da conversa.

Nos bastidores, o servidor Playwright MCP usa a [árvore de acessibilidade][playwright-mcp-server] da página, não capturas de tela. O agente raciocina sobre elementos estruturados e rotulados, como botões, links e itens de lista, da mesma forma que a tecnologia assistiva; assim, uma verificação funcional rápida também serve como uma verificação básica de acessibilidade.

> [!NOTE]
> O aplicativo deve estar em execução em `http://localhost:4321`. Se o servidor de desenvolvimento estiver parado, inicie-o antes de enviar o prompt. Na primeira utilização, o Playwright MCP pode precisar baixar um navegador; se isso acontecer, peça ao Copilot que execute `npx playwright install chromium`.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publique o branch e crie uma pull request

Depois de confirmar que a funcionalidade funciona, publique o branch `filtering-vscode`.

1. Abra o painel **Source Control** e revise as alterações do Copilot.
2. Prepare as alterações selecionando o ícone **+**.
3. Gere uma mensagem de commit com o botão **Sparkle**.

   ![Painel Source Control mostrando as alterações](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar o branch ao repositório.

## Crie a pull request

Peça ao Copilot que encontre a issue relacionada e crie uma PR associada a ela.

1. Abra o painel Copilot Chat e selecione **New Chat**.
2. Selecione **Agent** para que o Copilot possa usar as ferramentas do GitHub.
3. Envie:

   ```plaintext
   Encontre no repositório a issue relacionada à filtragem por categoria e editora. Crie uma pull request para o branch atual e associe-a à issue correta.
   ```

4. Selecione **Continue** quando necessário para permitir que o Copilot obtenha informações e execute ações. Na primeira utilização de uma ferramenta do GitHub, talvez seja necessário autenticar.
5. Confirme que o Copilot encontra a issue correta e cria a PR.
6. Abra o link da PR para revisá-la, mas não a mescle ainda.

## Resumo e próximas etapas

Parabéns! Neste exercício, você:

- aprendeu o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam os recursos do Copilot.
- adicionou o servidor Playwright MCP ao workspace.
- usou o servidor para testar manualmente a filtragem no navegador antes de publicá-la.
- publicou o branch e abriu uma pull request para o trabalho de filtragem.

Você usou um servidor MCP para *testar* uma funcionalidade, mas o MCP é apenas uma das maneiras de ampliar o Copilot. Em seguida, [crie um agente personalizado][next-lesson] para agilizar tarefas específicas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todos estão falando sobre isso?][mcp-blog-post]
- [Servidor Microsoft Playwright MCP][playwright-mcp-server]
- [Servidor MCP do GitHub][github-mcp-server]
- [Registro MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
