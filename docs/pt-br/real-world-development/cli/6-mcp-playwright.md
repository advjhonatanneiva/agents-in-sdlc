---
title: "Lição 6 - Valide a funcionalidade com Playwright MCP"
description: "Inspecione ou configure o Playwright MCP no Copilot CLI e use-o para explorar a filtragem no navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Escrever código envolve mais do que apenas programar: é preciso trabalhar com dados, serviços externos e automações adicionais disponíveis ao Copilot. Os servidores MCP permitem que o Copilot vá além dos recursos integrados ao CLI e acesse outras ferramentas e serviços.

Nesta lição, você vai:

- entender o Model Context Protocol (MCP) e como o Copilot CLI o usa.
- adicionar o servidor Playwright MCP se ele ainda não estiver disponível.
- pedir ao agente que controle um navegador e explore a filtragem.

## Cenário

Testes unitários e de ponta a ponta são importantes, mas validar alterações na interface exige interagir com ela. Você quer permitir que o Copilot use o site como um usuário usaria, para automatizar a verificação de que as alterações funcionam como esperado.

## O que é o Model Context Protocol (MCP)?

O [Model Context Protocol (MCP)][mcp-blog-post] permite que agentes de IA se comuniquem com ferramentas e serviços externos. Assim, eles podem acessar informações atualizadas e executar ações em seu nome.

Essas ferramentas e recursos são acessados por um servidor MCP, que atua como ponte entre o agente e as ferramentas ou serviços externos. Cada servidor MCP representa um conjunto diferente de ferramentas e recursos disponíveis ao agente.

Alguns servidores MCP conhecidos:

- **[Servidor MCP do GitHub][github-mcp]**: dá acesso a APIs para gerenciar repositórios, issues e pull requests do GitHub.
- **[Servidor Playwright MCP][playwright-mcp-server]**: oferece recursos de automação de navegador com Playwright.

Há muitos outros servidores MCP. O GitHub mantém um [registro MCP][mcp-registry] para facilitar a descoberta e as contribuições ao ecossistema.

> [!CAUTION]
> Trate servidores MCP como qualquer outra dependência do projeto. Antes de usar um servidor, revise o código-fonte, verifique o publicador e considere as implicações de segurança.

## Adicione o servidor Playwright MCP

Se o servidor Playwright MCP ainda não estiver disponível, adicione-o para que o Copilot possa interagir com o site como um usuário.

> [!NOTE]
> Se o Playwright MCP já estiver configurado no Copilot CLI, ignore as etapas de configuração e continue na próxima seção.

1. Volte ao Codespace.
2. No Copilot CLI, abra a caixa de diálogo para adicionar servidores MCP com:

   ```plaintext
   /mcp add
   ```

3. Digite `playwright` como nome e pressione <kbd>Tab</kbd>.
4. Confirme o tipo de servidor **STDIO** pressionando <kbd>Enter</kbd> e depois <kbd>Tab</kbd>.
5. Cole o comando a seguir na caixa **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pressione <kbd>Control</kbd>+<kbd>S</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para salvar o servidor MCP.
7. Pressione <kbd>Esc</kbd> para sair da caixa de diálogo MCP.

## Peça ao Copilot que explore a funcionalidade com Playwright

Você já confirmou manualmente que a funcionalidade funciona. Agora use o servidor Playwright MCP disponível no Copilot para fazer a mesma exploração.

1. Envie este prompt para pedir ao Copilot que valide a funcionalidade com o servidor Playwright MCP:

   ```plaintext
   Inicie o aplicativo e use o Playwright MCP para verificar a filtragem em relação à issue e ao nosso plano. Informe o que funciona e o que não funciona, sem fazer alterações. Quando terminar, encerre o servidor que iniciou.
   ```

> [!NOTE]
> Em geral, não é necessário dizer ao Copilot que use o servidor MCP, pois ele costuma identificá-lo sozinho. Ainda assim, indicar a ferramenta pode ajudar a obter resultados mais consistentes.

2. Acompanhe as etapas que o Copilot executa no navegador para confirmar se a funcionalidade funciona.
3. Leia o relatório e verifique se tudo se comporta como esperado.

O Copilot iniciará o servidor, usará o Playwright para interagir com o site, encerrará o servidor e apresentará um relatório.

## Resumo e próximas etapas

Parabéns! Você usou o servidor Playwright MCP para explorar sua funcionalidade em um navegador real por meio do Copilot CLI. Em resumo, você:

- aprendeu o que é o Model Context Protocol (MCP) e como o Copilot CLI o usa.
- adicionou o servidor Playwright MCP, caso ele ainda não estivesse disponível.
- pediu ao agente que controlasse um navegador e explorasse a filtragem.

Em seguida, [crie um agente personalizado de QA][next-lesson] que reúna a skill e as ferramentas do navegador em uma função especializada.

## Recursos

- [O que é MCP e por que todos estão falando sobre isso?][mcp-blog-post]
- [Servidor Microsoft Playwright MCP][playwright-mcp-server]
- [Adicionar servidores MCP ao Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
