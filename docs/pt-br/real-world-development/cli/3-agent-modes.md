---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Use o Plan para definir uma abordagem, o Autopilot para implementar a filtragem com base em uma issue e o modo Interactive para revisar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Começamos adicionando uma pequena funcionalidade ao projeto. Alterações maiores, porém, exigem um processo mais robusto. Felizmente, o GitHub Copilot CLI foi desenvolvido para trabalhar com o fluxo existente de uma organização e garantir que as soluções certas sejam criadas da maneira certa. Esta é a primeira de várias lições em que você seguirá um processo de desenvolvimento típico com agentes: começará por uma issue para criar uma funcionalidade e garantirá que o código seja válido, que a funcionalidade se comporte como esperado e que seja mesclada com sucesso ao projeto.

> [!NOTE]
> Você usará a mesma conversa e ramificação ao longo do fluxo de trabalho da funcionalidade. Em geral, você usaria ramificações ou PRs diferentes para os vários tipos de arquivo, mas faremos um atalho para nos concentrarmos nos conceitos principais.

Nesta lição, você vai:

- iniciar uma conversa do Copilot a partir de uma issue do GitHub.
- definir requisitos no modo Plan.
- implementar a funcionalidade com o modo Autopilot.
- revisar o código.
- validar manualmente a funcionalidade em um navegador encaminhado.

Ao dar continuidade à funcionalidade, você atualizará as instruções do repositório, personalizará a skill existente `quality-checks`, adicionará validação MCP, criará um agente de QA e abrirá a PR da funcionalidade.

## Cenário

O catálogo da Tailspin Toys está crescendo e as pessoas visitantes precisam filtrar os jogos por categoria e editora. A issue do backlog descreve a funcionalidade, mas detalhes como combinar categorias precisam ser definidos antes da codificação. Você usará o modo Plan para tomar essas decisões e, em seguida, autorizará uma implementação com escopo definido pelo Autopilot.

## Contexto

Introduzir agentes de programação de IA no fluxo de desenvolvimento não muda os fundamentos. Na verdade, eles se tornam ainda mais importantes! A maioria das pessoas desenvolvedoras segue um fluxo semelhante a este:

1. Começar por uma issue registrada que descreva o trabalho necessário.
2. Planejar o que precisa ser criado.
3. Criar e revisar o código.
4. Executar os testes para validar o código.
5. Validar manualmente a nova funcionalidade.
6. Criar uma pull request (PR).
7. Depois da revisão do código e do sucesso da integração contínua, mesclar o código.

> [!NOTE]
> Os detalhes exatos variam conforme a equipe e a organização, mas a maioria dos fluxos segue alguma variação do processo acima.

Ao seguir essa abordagem padrão, você garante que o código gerado por IA atenda aos requisitos e passe pelo mesmo processo de verificação que o código escrito manualmente.

## Modos de conversa

O **modo de conversa** controla o nível de autonomia do agente. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre os modos:

- **Interactive**: você e o agente trabalham em conjunto. O agente sugere alterações e aguarda sua orientação antes de continuar.
- **Plan**: o agente cria um plano primeiro e não pode editar arquivos do projeto.
- **Autopilot**: o agente trabalha de forma autônoma — escreve código, executa testes e itera até concluir a tarefa.

Comece no modo Plan, revise o plano e, em seguida, use o Autopilot para implementá-lo.

## Começar pela issue

Antes de iniciar o trabalho de filtragem, volte ao Codespace e confirme que o repositório e o terminal estão prontos.

1. Volte ao Codespace. Se ele estiver parado, reinicie-o antes de continuar.
2. Confirme que a PR das avaliações por estrelas foi mesclada.
3. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Atualize `main` e crie uma ramificação para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicie o Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pressione <kbd>Tab</kbd> duas vezes para abrir a guia **Issues**.
7. Pressione <kbd>A</kbd> para exibir todas as issues.
8. Use as setas para destacar a issue intitulada **Allow users to filter games by category and publisher**.
9. Pressione <kbd>C</kbd> para adicionar a issue ao prompt e voltar à guia **Session**.

Observe que o prompt agora começa com `#7` (ou um número semelhante). O caractere `#` permite trazer uma issue ou pull request (PR) do GitHub para o contexto.

## Planejar a filtragem

O planejamento permite definir a abordagem de implementação de uma funcionalidade ou de execução de uma tarefa antes de delegá-la ao Copilot. Para qualquer trabalho complexo, é uma boa ideia reservar um tempo para planejar. Vamos mudar para o modo Plan e pedir ao Copilot que crie o plano.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para mudar para o modo Plan. Confirme que o indicador abaixo do prompt exibe **Plan**.
2. Depois da referência à issue adicionada na etapa anterior, digite este prompt:

   ```plaintext
   Create a plan for implementing this feature.
   ```

O Copilot começará a elaborar o plano: primeiro explorará o projeto e, em seguida, determinará a melhor abordagem.

3. Durante o processo, o Copilot poderá perguntar como as opções de filtragem devem funcionar. Responda de acordo com suas preferências. Não há respostas erradas!
4. Quando o plano estiver pronto, pressione <kbd>Control</kbd>+<kbd>E</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandi-lo.
5. Role a tela para revisar o plano.
6. Peça ao Copilot que revise qualquer parte que não corresponda às suas decisões.

## Aprovar o Autopilot

Com o plano escrito e revisado, é hora de implementá-lo! Vamos deixar o Copilot trabalhar usando o Autopilot.

O Autopilot permite que o Copilot itere até considerar a tarefa concluída.

1. Selecione **Accept plan and build on autopilot (recommended)** ou a opção com rótulo semelhante na versão instalada.
2. Confirme que o indicador de modo abaixo do prompt exibe **Autopilot**.
3. Acompanhe o Copilot enquanto ele segue o plano estabelecido, gera código e executa testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, portanto revise o plano antes. Se o Copilot informar que faltam dependências ou que há conflito de portas, resolva o problema de configuração antes de considerar as verificações concluídas.

## Revisar e verificar a implementação

Depois que o código for gerado, ele precisa ser revisado antes de ser mesclado, como qualquer outro código. Vamos revisar o código e executar o site para confirmar que tudo funciona.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar no modo Interactive. Confirme que o indicador de modo não mostra **Plan** nem **Autopilot**.
2. Digite `/diff` e inspecione a implementação da filtragem e os testes.
3. Compare o resultado com a issue e as decisões tomadas durante o planejamento.
4. Depois de revisar o código, pressione <kbd>Esc</kbd> para sair da visualização de diff.
5. Revise a saída das verificações do projeto e peça ao Copilot que resolva eventuais falhas.

## Explorar a nova funcionalidade

Tudo parece certo no código — mas será que funciona? Vamos iniciar o aplicativo como antes e abrir o site pela porta encaminhada do Codespaces.

1. Peça ao Copilot que inicie o aplicativo:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Teste os filtros por categoria e editora, bem como as combinações acordadas no plano.
4. Confirme que o comportamento de redefinição e de resultados vazios corresponde à issue e às decisões tomadas.
5. Volte ao Codespace e peça ao Copilot que pare o servidor de desenvolvimento iniciado.

## Resumo e próximos passos

Você usou diferentes modos de conversa para criar e revisar uma funcionalidade. Nesta lição, você:

- iniciou uma conversa do Copilot a partir de uma issue do GitHub.
- definiu requisitos no modo Plan.
- implementou a funcionalidade com o modo Autopilot.
- revisou o código.
- validou manualmente a funcionalidade em um navegador encaminhado.

Em seguida, vamos nos aprofundar em como o código é gerado e garantir que siga as práticas documentadas, [usando instruções personalizadas][next-lesson].

## Recursos

- [Autopilot no GitHub Copilot CLI][autopilot]
- [Referência de comandos do Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
