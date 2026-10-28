---
title: "Exercício 2 - Agente na nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

É provável que poucas organizações — se houver alguma — não tenham dificuldades com dívida técnica. Ela pode envolver problemas de segurança não resolvidos, código legado que precisa de atualizações ou solicitações de funcionalidades que ficaram no backlog por falta de tempo para implementá-las. O agente na nuvem do GitHub Copilot foi desenvolvido para tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando termina o trabalho, ele gera uma PR de rascunho para revisão por uma pessoa desenvolvedora. Assim, é possível delegar tarefas repetitivas, acelerar o desenvolvimento e deixar a equipe livre para se concentrar em questões mais abrangentes.

Você explorará os seguintes recursos do agente na nuvem do Copilot:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam executadas com segurança.
- entender a importância de issues com escopo claro.
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem algumas dívidas técnicas para resolver. As pessoas contratadas inicialmente para criar a primeira versão do site deixaram a documentação em um estado inadequado — na verdade, você verá que ela está completamente ausente. Como primeiro passo, a equipe quer adicionar comentários de documentação TSDoc a todas as funções exportadas do aplicativo.

Além disso, a equipe de design está pronta para melhorar a descoberta de jogos. Ela quer que a página de detalhes de cada jogo mostre jogos relacionados — outros títulos da mesma categoria — para que as pessoas possam continuar navegando. Por enquanto, não é necessária uma implementação refinada; basta algo que possa ser usado em testes de aceitação da experiência. Esse trabalho é atualmente um impedimento, mas há outras issues com prioridade maior.

Esses são exemplos de tarefas que podem facilmente perder prioridade e que são ótimas candidatas para atribuição ao agente na nuvem do Copilot. O agente pode trabalhar nelas de forma assíncrona, enquanto você se concentra em outras tarefas e depois volta para revisar o trabalho e confirmar que está tudo certo.

## Conhecer o agente na nuvem do GitHub Copilot

O [agente na nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) pode executar tarefas em segundo plano, de forma semelhante a uma pessoa desenvolvedora. Assim como no trabalho com uma pessoa desenvolvedora, há várias maneiras de usá-lo, incluindo [atribuir uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois da atribuição, o Copilot cria uma pull request de rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar. Você pode examinar a sessão enquanto o trabalho está em andamento ou depois de concluído. Quando a solução proposta estiver pronta para revisão, o agente marcará você na pull request.

## A importância de instruções com escopo bem definido

O GitHub Copilot não faz mágica: não é possível pular a etapa de raciocínio. Até operações aparentemente simples se tornam complexas quando examinadas de perto. Por isso, [defina com cuidado o escopo das tarefas atribuídas ao agente na nuvem do Copilot][cloud-agent-best-practices]. Trate-o como uma dupla de programação com IA: trabalhe por etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a inclusão da IA generativa.

## Instruções personalizadas neste repositório

Os exercícios anteriores apresentaram as instruções personalizadas e como elas orientam o Copilot. Se você já concluiu uma atividade prática sobre instruções personalizadas, viu `.github/copilot-instructions.md` em ação; se ainda não, este é um bom momento para dar uma olhada rápida. Antes de atribuir trabalho ao agente na nuvem do Copilot, leia os arquivos de instruções já incluídos neste repositório, sem alterá-los, para depois identificar seus efeitos.

Abra os arquivos a seguir na interface Web do GitHub ou em um codespace, se já houver um em execução:

- `.github/copilot-instructions.md` — revise a seção **Code standards**, principalmente as expectativas para as convenções de TypeScript e os comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md` — observe o frontmatter `applyTo`, que limita estas instruções aos arquivos `**/*.test.ts`.

Quando você atribuir ao agente na nuvem a issue `Code lacks documentation` na próxima seção, observe se a pull request resultante inclui comentários TSDoc, segue as convenções de TypeScript e adiciona cabeçalhos aos comentários. Essas orientações vêm desses arquivos de instruções. Voltaremos a esse assunto ao revisar PRs em um exercício posterior.

## Configurar o ambiente de desenvolvimento do agente na nuvem do Copilot

A criação de código normalmente exige um ambiente específico e a execução de scripts de configuração para garantir que tudo esteja pronto, independentemente de quem esteja trabalhando. O mesmo vale para tarefas atribuídas ao Copilot, que trabalha de maneira semelhante a uma pessoa engenheira de software.

O agente na nuvem usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizar esse ambiente criando um [workflow de configuração especial][setup-workflow], definido em `.github/workflows/copilot-setup-steps.yml`, para ser executado antes do início do trabalho. Assim, o agente terá acesso às ferramentas e dependências de desenvolvimento necessárias. Esse workflow já está configurado no projeto para facilitar o fluxo do laboratório e oferecer essa oportunidade de aprendizado. Ele garante que o Copilot tenha acesso ao Node.js, às dependências do projeto, aos binários de navegador para testes de ponta a ponta e ao banco de dados SQLite migrado e preenchido para o aplicativo Astro único:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina apenas as permissões mínimas necessárias para *suas etapas*. O Copilot receberá um token próprio para as operações dele.
    permissions:
      # Para clonar o repositório como parte das etapas de configuração, por exemplo, para instalar dependências, você precisará da permissão `contents: read`. Se não fizer isso, o Copilot clonará o repositório automaticamente após a conclusão das etapas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do frontend/aplicativo - Node.js (agora, todo o aplicativo usa Astro + Drizzle/libSQL)
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

O arquivo parece qualquer outro workflow do GitHub, mas tem alguns pontos importantes:

- Ele contém um único job chamado `copilot-setup-steps`. Esse job é executado no GitHub Actions antes de o Copilot começar a trabalhar na pull request.
- Observe o gatilho `workflow_dispatch`, que permite executar o workflow manualmente na guia **Actions** do repositório. Isso é útil para verificar se ele funciona, sem esperar que o Copilot o execute.

## Adicionar documentação

Embora todo mundo entenda a importância da documentação, a maioria dos projetos tem informações desatualizadas ou não tem documentação. Esse tipo de dívida técnica muitas vezes fica sem solução, reduz a produtividade e dificulta a manutenção da base de código e a integração de pessoas desenvolvedoras. Felizmente, o Copilot é muito bom em criar documentação, e esta é uma issue perfeita para atribuir ao agente na nuvem. Ele trabalhará em segundo plano para gerar a documentação necessária. Você voltará em um exercício futuro para revisar o trabalho.

1. Abra uma nova guia do navegador e acesse o repositório em github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para criar uma issue em branco.
5. Defina o **Title** como `Code lacks documentation`.
6. Defina a **Description** como:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

  ![Detalhes da atribuição ao Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecione a guia **Pull Requests**.
11. Abra a nova pull request (PR), cujo título será semelhante a `[WIP]: Code lacks documentation`. Se a PR não aparecer na lista, aguarde um ou dois minutos e atualize a janela do navegador.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas do Copilot pode levar vários minutos para aparecer na PR. O Copilot está configurando o ambiente (executando o workflow mencionado anteriormente), analisando o projeto e definindo a melhor abordagem para a tarefa.

13. Revise a lista e as tarefas que o Copilot pretende executar.
14. Role a linha do tempo da pull request para baixo. Você verá uma atualização informando que o Copilot começou a trabalhar na issue.
15. Selecione o botão **View session**.

  ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Observe que você pode percorrer a sessão em tempo real e acompanhar como o Copilot resolve o problema. Ele explora o código e entende o estado do projeto, faz pausas para pensar e definir o plano adequado e também cria código.

O processo provavelmente levará alguns minutos. Um dos principais objetivos do agente na nuvem do Copilot é executar tarefas de forma assíncrona, liberando você para trabalhar em outras atividades. Vamos aproveitar esse recurso atribuindo outra tarefa ao agente na nuvem do Copilot e, em seguida, voltando nossa atenção à criação de funcionalidades para o aplicativo.

## Adicionar uma seção de jogos relacionados à página de detalhes

Como já mencionamos, uma das grandes vantagens do agente na nuvem do GitHub Copilot é a possibilidade de dividir o trabalho: você pode se concentrar em um conjunto de tarefas enquanto ele trabalha em outro. Adicionar uma seção de jogos relacionados para a equipe de design talvez não leve muito tempo, mas esse tempo poderia ser usado em outras tarefas. Vamos atribuir o trabalho ao agente na nuvem do Copilot!

1. Volte ao repositório em github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para usar o modelo em branco.
5. Defina o **Title** como `Show related games on the game details page`.
6. Defina a **Description** como:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, você verá vários 👀 no primeiro comentário da issue, indicando que o Copilot está trabalhando nela!

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot agora está trabalhando na sua segunda solicitação! O agente na nuvem do Copilot trabalha de maneira semelhante a uma pessoa engenheira de software, portanto não é necessário acompanhá-lo o tempo todo; basta revisar o resultado quando estiver pronto. Agora vamos nos dedicar à criação e ao uso de agentes personalizados.

## Resumo e próximos passos

Esta lição apresentou o [agente na nuvem do GitHub Copilot][copilot-agents]. Com o agente na nuvem, você pode atribuir issues ao Copilot para execução assíncrona. É possível pedir ao Copilot que resolva dívidas técnicas, crie funcionalidades ou ajude a migrar código de uma estrutura para outra.

Você explorou estes conceitos:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam executadas com segurança.
- entender a importância de issues com escopo claro.
- atribuir issues ao Copilot.

Enquanto o agente na nuvem trabalha em segundo plano, podemos nos concentrar na criação e no uso de agentes personalizados. O [agente na nuvem do Copilot também pode usar servidores MCP][cloud-agent-mcp] e tem acesso a instruções personalizadas, que exploramos nos módulos anteriores.

## Recursos

- [Sobre o agente na nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente na nuvem do Copilot][cloud-agent-best-practices]

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
