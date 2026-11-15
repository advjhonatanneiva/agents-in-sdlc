---
title: "Exercício 0: Pré-requisitos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de começar os exercícios do VS Code, prepare o ambiente. Crie uma cópia do repositório Tailspin Toys, inicie um [Codespace][codespaces] e confirme que o GitHub Copilot Chat está funcionando no editor.

## Configure o repositório do laboratório

Crie uma cópia do repositório usando o [modelo][template-repository]. A cópia conterá os arquivos necessários para o laboratório.

1. Em uma nova janela do navegador, acesse o [modelo da Tailspin Toys][tailspin-template].
2. Na página do repositório do laboratório, selecione **Use this template** e depois **Create a new repository**.

   ![Botão Use this template](../../../_images/ex0-use-template.png)

3. Se estiver participando de um evento do GitHub ou da Microsoft, siga as instruções dos mentores. Caso contrário, crie o repositório em uma organização na qual você tenha acesso ao GitHub Copilot.

   ![Configurações do repositório criado a partir do modelo](../../../_images/ex0-repository-settings.png)

4. Anote o caminho do repositório criado (**organization-or-user-name/repository-name**), pois você precisará dele mais adiante.

> [!NOTE]
> **O backlog está pronto**
>
> Ao criar o repositório a partir do modelo, um backlog de issues do GitHub é criado automaticamente. Você trabalhará com essas issues durante o workshop; não precisa criar nenhuma.

## Crie um Codespace

Use um Codespace para concluir os exercícios do laboratório.

[GitHub Codespaces][codespaces] é um ambiente de desenvolvimento na nuvem que permite escrever, executar e depurar código no navegador. Ele oferece um IDE completo com suporte a várias linguagens, extensões e ferramentas.

1. Acesse o repositório que acabou de criar.
2. Selecione o botão verde **Code**.

   ![Botão Code](../../../_images/ex0-code-button.png)

3. Selecione a guia **Codespaces** e o botão **+** para criar um Codespace.

   ![Criar um Codespace](../../../_images/ex0-create-codespace.png)

A criação pode levar alguns minutos, embora seja bem mais rápida do que instalar todos os serviços manualmente. Enquanto isso, explore outros recursos do GitHub Copilot. Deixe o Codespace aberto em uma guia do navegador; você voltará a ele em um exercício futuro.

> [!NOTE]
> Este workshop foi criado para ser executado em um Codespace ou [contêiner de desenvolvimento][dev-containers] local. Ambos fornecem os pré-requisitos necessários para uma experiência tranquila. Para trabalhar localmente, abra o repositório clonado no VS Code e selecione **Reopen in Container** quando solicitado. O VS Code criará o mesmo contêiner de desenvolvimento usado pelo Codespace.

## Use o GitHub Copilot Chat e o modo de agente

Para acessar o modo de agente do GitHub Copilot Chat, instale a extensão GitHub Copilot Chat no IDE. Ela já deve estar instalada se você estiver usando um Codespace.

> [!TIP]
> Se a extensão não estiver instalada, [instale-a pelo Marketplace do Visual Studio Code][copilot-chat-extension] ou abra a visualização Extensions, pesquise **GitHub Copilot Chat** e selecione **Install**.

Talvez também seja necessário autenticar sua conta do GitHub.

1. Volte ao Codespace.
2. Se o Copilot Chat não aparecer à direita do editor, selecione o ícone **Copilot Chat** na parte superior do Codespace.
3. Digite uma mensagem, como "Hello world", na janela do Copilot Chat e pressione <kbd>Enter</kbd>.
4. Se for solicitado que você entre na conta do GitHub, siga as instruções de autenticação.

   ![Exemplo de solicitação de autenticação do Copilot Chat](../../../_images/ex1-copilot-authentication.png)

5. Após a autenticação, a janela do Copilot Chat deverá aparecer.

## Resumo

Você criou uma cópia do repositório do laboratório e iniciou a criação do Codespace que usará para desenvolver.

## Próxima etapa

Agora, [comece a usar o Copilot][next-lesson] e ensine a ele as convenções do projeto.

## Recursos

- [Visão geral do GitHub Codespaces][codespaces]
- [Criar um repositório a partir de um modelo][template-repository]
- [Introdução ao Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers
