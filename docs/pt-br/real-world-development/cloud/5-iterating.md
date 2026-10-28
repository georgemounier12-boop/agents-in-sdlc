---
title: "Exercício 5: Iterar sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revisar o trabalho

Durante este laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Você pediu ao Copilot que documentasse o código, criasse um recurso de jogos relacionados para a equipe de design iterar e implementasse recursos de acessibilidade, incluindo controles para os modos de alto contraste e claro. Vamos explorar as alterações de código e, se necessário, enviar comentários ao Copilot para melhorar o resultado.

### Cenário

Como já destacamos várias vezes, os fundamentos de design de software e DevOps não mudam com a chegada da IA generativa. Sempre queremos revisar o código gerado e seguir nosso processo normal de DevOps. Com isso em mente, vamos revisar as sugestões do GitHub Copilot para criar a documentação, o recurso de jogos relacionados e os recursos de acessibilidade antes de habilitar a revisão para o restante da equipe.

## Segurança e o agente na nuvem do GitHub Copilot

Como o agente na nuvem do Copilot executa tarefas de forma assíncrona e sem supervisão, existem algumas restrições de segurança para manter o ambiente protegido. Entre elas:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação **somente** à ramificação usada para o código.
- O agente na nuvem é executado no GitHub Actions, que cria um ambiente separado e temporário para o trabalho.
- Todos os fluxos de trabalho do GitHub Actions precisam ser aprovados por uma pessoa antes de serem executados.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluindo servidores MCP.

## Revisar a documentação gerada

Vamos começar pela primeira pull request (PR) criada pelo agente na nuvem do GitHub Copilot: adicionar documentação ao código. Você fará isso na interface padrão de PRs do GitHub.com.

> [!NOTE]
> Ao explorar a PR, você poderá ver um aviso de que o GitHub Copilot foi bloqueado por um firewall. Isso é **esperado**, pois o Copilot tem acesso limitado a recursos externos por padrão, incluindo chamadas a servidores MCP externos. Se quiser, você pode [personalizar ou desabilitar o firewall do agente na nuvem do Copilot][agent-firewall].

1. Volte ao repositório no github.com em uma nova guia do navegador.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra a pull request com um título semelhante a **Add missing documentation** ou outro título mais específico.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, a pull request conterá a indicação **[WIP]**. Nesse caso, aguarde até que o Copilot termine. Isso pode levar alguns minutos, então faça uma pausa ou reflita sobre o que aprendeu até agora.

4. Quando a pull request estiver pronta, selecione a guia **Files changed** e revise as alterações.

    ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Explore o código atualizado, que inclui os comentários TSDoc recém-criados e outra documentação. As alterações exatas variam.

    Ao examinar as alterações, procure comentários TSDoc, convenções de TypeScript e cabeçalhos de comentários. Eles foram definidos nos arquivos de instruções personalizadas que você revisou no início do Exercício 2.

6. Depois de revisar as atualizações e confirmar que estão corretas, volte à guia **Conversation** e role a página para baixo.
7. Você deverá ver um indicador de que alguns fluxos de trabalho estão aguardando aprovação.
8. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

9. Os fluxos de trabalho deverão entrar na fila da seção de verificações da pull request. Se tudo estiver correto, as verificações do projeto para o aplicativo Astro único serão aprovadas. Isso pode levar alguns minutos.

## Solicitar alterações ao GitHub Copilot

Trabalhar com o Copilot em uma pull request não é uma via de mão única. Você também pode mencionar o Copilot em comentários da PR — como faria com outra pessoa da equipe — ou em comentários de linha no código. O Copilot verá esses comentários e iniciará outra sessão para atendê-los. Como os resultados não são determinísticos, não podemos fornecer um texto prescritivo. Algumas sugestões do que pedir ao Copilot:

- Adicionar cabeçalhos de comentários no início de cada arquivo de código, com uma breve descrição do que ele faz.
- Adicionar comentários TSDoc a arquivos TypeScript e Astro.
- Criar um README que descreva a estrutura do aplicativo Astro.

1. Adicione um comentário solicitando uma alteração na documentação gerada e mencione **@copilot**, como faria com qualquer pessoa. Use uma das ideias acima ou outra sugestão de documentação que gostaria de ver na base de código.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Observe como ele inicia uma nova sessão para fazer as atualizações.
3. Você pode selecionar **Back to pull request** para voltar à pull request.

    ![Voltar à pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot terminar as alterações, deverá aparecer um novo commit na pull request.
5. Selecione a guia **Files changed** para revisar as alterações.

Continue iterando até ficar satisfeito. Quando estiver tudo pronto, você poderá converter a PR de rascunho em pronta para revisão e mesclá-la na ramificação principal.

![Converter PR em pronta para revisão](../../../_images/ex7-ready-for-review.png)

## Revisar o recurso de jogos relacionados

Vamos voltar à PR criada pelo Copilot para resolver a issue sobre exibir jogos relacionados na página de detalhes de um jogo.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione a PR com um título semelhante a **Show related games on the game details page** ou outro título mais específico.
4. Selecione a guia **Files changed** para revisar o código gerado.
5. Depois de revisar as atualizações e confirmar que estão corretas, volte à guia **Conversation** e role a página para baixo.
6. Você deverá ver um indicador de que alguns fluxos de trabalho estão aguardando aprovação.
7. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

8. Os fluxos de trabalho deverão entrar na fila da seção de verificações da pull request. Se tudo estiver correto, as verificações do projeto para o aplicativo Astro único serão aprovadas. Isso pode levar alguns minutos.
9. **Opcional:** Você pode alternar para essa ramificação no Codespace para testar manualmente o recurso de jogos relacionados. Acesse o Codespace, abra o terminal e execute os seguintes comandos (substitua `<branch-name>` pelo nome da ramificação criada pelo Copilot, por exemplo, **copilot/fix-8**):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

O Copilot criou o recurso de jogos relacionados! Como antes, você pode trabalhar de forma iterativa com o agente na nuvem para solicitar atualizações. Por exemplo, talvez você queira ajustar a quantidade de jogos relacionados exibidos ou garantir que os cabeçalhos de comentários e os comentários TSDoc sejam adicionados. Lembre-se: essa tarefa foi atribuída **antes** de você atualizar as instruções personalizadas. Como antes, envie essas solicitações em um novo comentário na guia **Conversation**; o Copilot verá o comentário e iniciará uma nova sessão.

## Revisar os recursos de acessibilidade

Por fim, vamos revisar os recursos de acessibilidade implementados usando o agente personalizado de acessibilidade. Esta PR deverá incluir o modo de alto contraste atribuído no Exercício 3 e o modo claro solicitado no centro de controle no Exercício 4.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione a PR com um título semelhante a **Add high contrast mode to website** ou outro título mais específico.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, a pull request conterá a indicação **[WIP]**. Nesse caso, aguarde até que o Copilot termine. Isso pode levar alguns minutos.

4. Selecione a guia **Files changed** para revisar o código gerado.
5. Revise a implementação e preste atenção especial a:
   - componentes de interface para alternar entre os modos.
   - uso do armazenamento local para manter as preferências do usuário.
   - alterações de CSS ou estilo para os modos de alto contraste e claro.
   - atributos de acessibilidade, como rótulos ARIA e navegação por teclado.
   - código JavaScript/TypeScript que controla a alternância entre modos.

6. Depois de revisar as atualizações e confirmar que estão corretas, volte à guia **Conversation** e role a página para baixo.
7. Você deverá ver um indicador de que alguns fluxos de trabalho estão aguardando aprovação.
8. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

9. Os fluxos de trabalho deverão entrar na fila da seção de verificações da pull request. Se tudo estiver correto, as verificações do projeto para o aplicativo Astro único serão aprovadas. Isso pode levar alguns minutos.
10. **Opcional:** Você pode alternar para essa ramificação no Codespace para testar manualmente os recursos de acessibilidade. Acesse o Codespace, abra o terminal e execute os comandos abaixo (substitua `<branch-name>` pelo nome da ramificação criada pelo Copilot):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Em seguida, inicie o aplicativo e teste os controles dos modos de alto contraste e claro no navegador. Confirme se eles funcionam e se as preferências persistem após recarregar a página.

Observe como o agente personalizado de acessibilidade ajudou a orientar o Copilot para implementar esses recursos seguindo as práticas recomendadas de acessibilidade. Se notar algum problema de acessibilidade ou oportunidade de melhoria, mencione **@copilot** em um comentário para solicitar atualizações, como fez com as outras PRs.

## Exercício opcional — continue delegando

O agente na nuvem funciona melhor quando você pode delegar itens reais do backlog e se dedicar a outras atividades. Para desenvolver esse hábito, crie mais algumas issues no seu repositório e atribua-as ao Copilot. Algumas ideias:

- Criar um formulário de interesse de apoiadores na página de detalhes do jogo.
- Implementar paginação na página de listagem de jogos.
- Adicionar validação de entrada e tratamento de erros aos auxiliares de acesso a dados.

## Resumo

Você concluiu o roteiro do agente na nuvem. Nestas lições, você:

- **Inspecionou os arquivos de instruções personalizadas do repositório** para ver posteriormente o efeito deles nos resultados do agente na nuvem.
- **Atribuiu issues ao agente na nuvem do Copilot** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona do trabalho.
- **Criou e usou um agente personalizado** para acessibilidade, adicionando controles dos modos de alto contraste e claro.
- **Usou a página de agentes do Copilot como centro de controle** para monitorar e orientar a sessão de acessibilidade durante a execução.
- **Revisou e iterou sobre as pull requests do agente na nuvem**, mencionando `@copilot` para solicitar alterações e aprovando fluxos de trabalho.

## Revisão e próximas etapas

Você concluiu o roteiro do agente na nuvem. Para continuar explorando, os outros roteiros complementam o que praticou:

- 🖥️ **[Roteiro do VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração com MCP diretamente no IDE.
- 💻 **[Roteiro do CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal com os modos Plan e Autopilot, skills de agente, um agente de QA personalizado, MCP do Playwright, Agent Merge e uma ponte `/delegate` opcional para o agente na nuvem.

No seu próprio repositório, experimente estas ideias:

- Atribua ao agente na nuvem uma issue de refatoração ou cobertura de testes.
- Crie um agente personalizado para outro domínio.
- Configure `copilot-setup-steps.yml` para outra pilha de tecnologia.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre os agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o fluxo de configuração do agente na nuvem do Copilot][cloud-agent-best-practices]
- [Configurar o firewall do agente na nuvem do Copilot][agent-firewall]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
