---
title: "Exercício 5 - Itere sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Acompanhe e gerencie agentes][previous-lesson] |
|:--|

## Revise o trabalho

Durante este laboratório, Você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Pediu que ele documentasse o código, criasse uma funcionalidade de jogos relacionados para a equipe de design iterar e implementasse recursos de acessibilidade, incluindo controles de alto contraste e modo claro. Explore as alterações de código e, se necessário, forneça feedback para melhorá-las.

### Cenário

Como já foi destacado várias vezes, os fundamentos de design de software e DevOps não mudam com a chegada da IA generativa. Sempre revise o código gerado e siga o processo normal de DevOps. Revise as sugestões do GitHub Copilot para a documentação, a funcionalidade de jogos relacionados e os recursos de acessibilidade antes de habilitar a revisão pelo restante da equipe.

## Segurança e agente de nuvem do GitHub Copilot

O agente de nuvem trabalha de forma assíncrona e sem supervisão direta, por isso há restrições de segurança:

- O Copilot tem acesso de leitura ao repositório e pode escrever somente no branch que usa para o código.
- O agente de nuvem executa no GitHub Actions em um ambiente separado e temporário.
- Um humano precisa aprovar fluxos de trabalho do GitHub Actions antes da execução.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluindo servidores MCP.

## Revise a documentação gerada

Comece pela primeira pull request (PR), que adiciona documentação ao código.

> [!NOTE]
> A PR pode mostrar um aviso de que o Copilot está bloqueado por um firewall. Isso é esperado, pois o acesso a recursos externos, incluindo servidores MCP, é limitado por padrão. Se quiser, [personalize ou desabilite o firewall][agent-firewall].

1. Em uma nova guia do navegador, volte ao repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra a PR intitulada algo como **Add missing documentation** ou com um título mais descritivo.
4. Se o Copilot ainda estiver trabalhando, a PR terá a marca **[WIP]**. Aguarde a conclusão; isso pode levar alguns minutos.
5. Quando a PR estiver pronta, selecione a guia **Files changed** e revise as alterações.

   ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

6. Examine o código atualizado, que inclui comentários TSDoc e outra documentação. As alterações exatas podem variar. Procure comentários TSDoc, convenções de TypeScript e cabeçalhos de arquivo, orientados pelas instruções personalizadas que você revisou no início do Exercício 2.
7. Quando terminar a revisão, volte à guia **Conversation** e role a página para baixo.
8. Procure o indicador de que há fluxos de trabalho aguardando aprovação.
9. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

10. Confirme que os fluxos de trabalho foram adicionados à seção de verificações da PR. Se tudo correr bem, as verificações do projeto para o aplicativo Astro único passarão. Isso pode levar alguns minutos.

> [!NOTE]
> Se o trabalho ainda estiver em andamento, a PR terá a marca **[WIP]**. Aguarde alguns minutos até que o Copilot conclua a tarefa.

## Peça alterações ao GitHub Copilot

Trabalhar com o Copilot em uma PR não é uma via de mão única. Você pode marcá-lo em comentários da PR ou em comentários de linha do código, como faria com qualquer integrante da equipe. O Copilot verá esses comentários e iniciará outra sessão para atendê-los. Como os resultados são não determinísticos, não há um texto prescritivo para solicitar. Por exemplo, você pode pedir:

- que adicione cabeçalhos com uma breve descrição no início de cada arquivo de código.
- que adicione comentários TSDoc a arquivos TypeScript e Astro.
- que crie um README explicando a estrutura do aplicativo Astro.

1. Adicione um comentário solicitando uma alteração na documentação gerada e marque **@copilot**, como faria com qualquer usuário. Use uma das ideias acima ou outra sugestão de documentação que gostaria de ver na base de código.
2. Selecione **View Session** para acompanhar o trabalho do Copilot e observar que ele inicia uma nova sessão para fazer as atualizações.
3. Selecione **Back to pull request** para voltar à PR.
4. Quando o Copilot terminar, confirme que há um novo commit na PR.
5. Selecione **Files changed** para revisar as alterações.

Continue iterando até ficar satisfeito. Depois, converta a PR de rascunho em pronta para revisão e mescle-a ao branch principal.

![Marcar PR como pronta para revisão](../../../_images/ex7-ready-for-review.png)

## Revise a funcionalidade de jogos relacionados

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Abra a PR intitulada algo como **Show related games on the game details page** ou com um título mais descritivo.
4. Selecione a guia **Files changed** para revisar o código gerado.
5. Depois da revisão, volte à guia **Conversation** e role a página para baixo.
6. Procure o indicador de fluxos de trabalho aguardando aprovação.
7. Se houver fluxos aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

8. Confirme que os fluxos foram adicionados à seção de verificações da PR e aguarde as verificações do aplicativo Astro único. Isso pode levar alguns minutos.
9. **Opcional:** teste manualmente a funcionalidade de jogos relacionados no Codespace. Abra o terminal e substitua `<branch-name>` pelo nome do branch criado pelo Copilot, por exemplo, `copilot/fix-8`:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

O Copilot criou a funcionalidade de jogos relacionados. Você pode iterar com o agente de nuvem pedindo, por exemplo, que ajuste o número de jogos relacionados ou acrescente cabeçalhos e comentários TSDoc. Lembre-se: essa tarefa foi atribuída **antes** da atualização das instruções personalizadas. Para solicitar alterações, adicione um comentário à guia **Conversation**; o Copilot o verá e iniciará outra sessão.

## Revise os recursos de acessibilidade

Por fim, revise a PR do agente personalizado de acessibilidade. Ela deve incluir o modo de alto contraste solicitado no Exercício 3 e o modo claro pedido no Exercício 4.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Abra a PR intitulada algo como **Add high contrast mode to website**.
4. Se o Copilot ainda estiver trabalhando, a PR terá a marca **[WIP]**. Aguarde a conclusão, que pode levar alguns minutos.
5. Selecione a guia **Files changed** para revisar o código gerado.
6. Revise a implementação, prestando atenção especial a:
   - componentes de interface dos controles para alternar os modos.
   - uso do armazenamento local para persistir as preferências do usuário.
   - alterações de CSS ou estilos para os modos de alto contraste e claro.
   - atributos de acessibilidade (rótulos ARIA, navegação por teclado etc.).
   - código JavaScript/TypeScript que gerencia a alternância dos modos.
7. Depois da revisão, volte à guia **Conversation** e role a página para baixo.
8. Procure o indicador de fluxos de trabalho aguardando aprovação.
9. Se houver fluxos aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

10. Confirme que os fluxos foram adicionados à seção de verificações da PR e aguarde as verificações do aplicativo Astro único.
11. **Opcional:** teste manualmente os recursos de acessibilidade no Codespace:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   Inicie o aplicativo e teste os controles de alto contraste e modo claro no navegador, incluindo a persistência após recarregar a página.

Se identificar problemas de acessibilidade, marque **@copilot** em um comentário e peça as alterações.

## Exercício opcional: continue delegando

Crie outras issues e atribua-as ao agente de nuvem. Por exemplo, solicite um formulário de interesse de apoiadores, paginação ou validação de entrada e tratamento de erros nos helpers de acesso a dados.

## Resumo

Você concluiu o ambiente do agente de nuvem. Ao longo das lições, você:

- **Inspecionou os arquivos de instruções personalizadas do repositório** para observar como influenciam o trabalho do agente de nuvem.
- **Atribuiu issues ao agente de nuvem do Copilot** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona.
- **Criou e usou um agente personalizado** de acessibilidade para adicionar controles de alto contraste e modo claro.
- **Usou a página de agentes do Copilot como centro de controle** para acompanhar e orientar a sessão de acessibilidade em andamento.
- **Revisou e iterou sobre as pull requests do agente de nuvem**, marcando `@copilot` para solicitar alterações e aprovando fluxos de trabalho.

## Revisão e próximas etapas

O ambiente de agente de nuvem está concluído. Para ampliar o que praticou, explore:

- 🖥️ **[Ambiente VS Code](../../vscode/)** — use o modo de agente do Copilot Chat e a integração MCP no IDE.
- 💻 **[Ambiente CLI](../../cli/)** — entregue duas alterações revisadas no terminal com Plan, Autopilot, skills, um agente de QA, Playwright MCP, Agent Merge e delegação opcional.

No próprio repositório, experimente atribuir uma issue de refatoração ou cobertura de testes, criar um agente personalizado para outro domínio ou configurar `copilot-setup-steps.yml` para outra stack.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre os agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas do fluxo de configuração do agente de nuvem][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem][agent-firewall]

| [← Lição anterior: Acompanhe e gerencie agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
