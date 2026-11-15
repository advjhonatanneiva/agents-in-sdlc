---
title: "Lição 2 - Adicione avaliações por estrelas: uma vitória rápida"
description: "Use o Copilot CLI para fazer uma pequena alteração nos cartões de jogos, revisar em um navegador encaminhado e mesclar como sua primeira pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Agora que você instalou o Copilot CLI e experimentou uma conversa, faça sua primeira alteração no projeto. Mantenha-a pequena: os dados dos jogos já têm uma avaliação por estrelas, mas os cartões da página inicial ainda não a exibem. Peça ao agente que mostre a avaliação, revise a alteração e mescle-a como sua primeira pull request.

Nesta lição, você vai:

- iniciar uma conversa focada do Copilot em um branch de funcionalidade.
- pedir ao agente que faça uma pequena alteração no projeto.
- revisar a alteração com `/diff`.
- executar o aplicativo e confirmar a alteração em um navegador encaminhado.
- abrir e mesclar sua primeira pull request.

## Cenário

Cada jogo da Tailspin Toys pode ter uma avaliação por estrelas, que já aparece na página de detalhes. Os cartões da página inicial mostram apenas o título, a categoria, a editora e a descrição. Como aquecimento, peça ao agente que exiba a avaliação existente em cada cartão: uma alteração pequena e independente, ideal para sua primeira sessão.

## Anatomia de uma conversa

Uma **conversa** é onde você trabalha com o Copilot CLI em uma tarefa. Diferentemente do aplicativo Copilot, uma conversa normal do CLI usa o repositório e o branch atualmente selecionados no terminal, em vez de criar um worktree dedicado. As conversas salvas permitem retomar a discussão depois, enquanto os arquivos e o branch continuam como estado comum do Git no disco.

Em uma conversa, você verá seus prompts e as respostas do agente, a atividade das ferramentas enquanto ele explora e edita arquivos e as alterações que pode inspecionar com `/diff`.

## Inicie uma conversa e solicite a alteração

Inicie uma conversa para começar a implementar a funcionalidade.

1. Volte ao seu Codespace.
2. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Se o Copilot ainda não estiver em execução, use este comando:

   ```bash
   copilot --yolo
   ```

4. Inicie uma sessão nova com o comando slash `/new` e pressione <kbd>Enter</kbd>.
5. Envie este prompt:

   ```plaintext
   Mostre `starRating` de cada jogo de 0 a 5 nos cartões da lista de jogos. Se a avaliação for `null`, mostre "No rating yet". Mantenha o layout atual dos cartões, adicione testes e execute as verificações pertinentes.
   ```

O Copilot explora o projeto, localiza os arquivos que exibem os detalhes dos jogos e cria o código necessário. Você adicionou uma funcionalidade com o Copilot CLI.

## Revise o diff

Revise todas as alterações geradas por IA antes de mesclá-las, mesmo quando forem pequenas.

1. Digite `/diff` e inspecione todos os arquivos alterados.
2. Confirme que o cartão mostra a avaliação numérica quando ela existe e `No rating yet` quando `starRating` é `null`.
3. Confirme que os testes cobrem os dois estados.
4. Revise os resultados das verificações executadas pelo Copilot e peça que ele corrija eventuais falhas.
5. Ao terminar a revisão, pressione <kbd>Esc</kbd> para sair da tela de diff.

> [!NOTE]
> Como todas as ferramentas de IA generativa, o Copilot é probabilístico, não determinístico. O código exato pode variar. Revise o comportamento em vez de esperar uma implementação idêntica.

## Verifique as alterações

Não basta ler o código e presumir que funciona. Peça ao Copilot que inicie o site para você examinar a interface do usuário (UI) atualizada no navegador encaminhado pelo Codespaces.

1. Peça ao Copilot que inicie o aplicativo:

   ```plaintext
   Inicie o aplicativo para que eu possa inspecionar no navegador a alteração das avaliações por estrelas. Informe a URL e deixe o servidor em execução.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Confirme que os cartões dos jogos exibem as avaliações de zero a cinco estrelas.
4. Volte ao Copilot e peça que encerre o servidor iniciado:

   ```plaintext
   Encerre o servidor de desenvolvimento que você iniciou.
   ```

## Abra e mescle sua primeira pull request

Agora que criou a funcionalidade, abra uma pull request (PR) para mesclar o novo código ao projeto.

1. Peça ao agente padrão que confirme a alteração:

   ```plaintext
   Faça commit das alterações revisadas das avaliações por estrelas com uma mensagem de commit adequada.
   ```

2. Digite `/pr create`. O Copilot CLI envia o commit existente ao criar a PR e exibe a URL dela.
3. Abra a PR mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando a URL exibida pelo Copilot CLI.
4. Revise os arquivos alterados e as verificações.
5. Quando estiver tudo pronto, selecione **Merge pull request** e confirme a mesclagem.
6. Volte ao Codespace e saia do Copilot CLI com `/exit`.
7. Atualize seu `main` local:

   ```bash
   git checkout main
   git pull
   ```

## Resumo e próximas etapas

Parabéns! Você entregou sua primeira alteração com o GitHub Copilot CLI. Você:

- iniciou uma conversa focada do Copilot em um branch de funcionalidade.
- orientou o agente a fazer uma pequena alteração nos cartões dos jogos.
- revisou a alteração com `/diff`.
- executou o aplicativo e confirmou a avaliação em um navegador encaminhado.
- abriu e mesclou sua primeira pull request.

Em seguida, [comece pela issue de filtragem e use os modos Plan e Autopilot][next-lesson] para criar uma funcionalidade maior.

## Recursos

- [Sobre o GitHub Copilot CLI][about-copilot-cli]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
