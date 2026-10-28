---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você pediu ao agente na nuvem do Copilot que realizasse três tarefas distintas para melhorar a experiência do usuário e adicionar funcionalidades. Embora o agente na nuvem tenha sido criado para trabalhar de forma assíncrona e autônoma, ainda é importante monitorar essas tarefas.

Há várias ferramentas para gerenciar as tarefas atribuídas ao agente na nuvem, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse centro de controle, você vê todas as tarefas do agente que têm pull requests (PRs) abertas. Você pode explorar as operações realizadas e até direcionar uma sessão em andamento para orientá-la.

Nesta lição, você vai:

- explorar a página de agentes para monitorar as tarefas do agente na nuvem.
- direcionar uma sessão em andamento para solicitar funcionalidades adicionais.

## Cenário

Depois de atribuir ao agente a tarefa de criar um modo de alto contraste, a equipe percebeu que seria um bom momento para adicionar também um modo claro. Como o trabalho já envolvia atualizar o estilo do site e adicionar a funcionalidade de alternância, pareceu lógico incluir esse recurso. Você orientará o agente para garantir que ele adicione o modo claro junto com o modo de alto contraste.

## Revisar as tarefas do agente na nuvem do Copilot

Vamos verificar o status atual de todas as tarefas atribuídas ao agente na nuvem do Copilot.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe a lista de tarefas no painel principal e no painel esquerdo. Você deverá ver as tarefas atribuídas ao Copilot, incluindo:
   - atualizar a documentação da base de código.
   - gerar APIs para modificar jogos.
   - adicionar um modo de alto contraste ao site.
3. Selecione uma das tarefas em andamento. Revise as operações realizadas pelo Copilot. Elas podem incluir:
   - obter o código do repositório.
   - criar o ambiente para o Copilot trabalhar.
   - configurar servidores MCP.
   - realizar várias etapas para concluir a tarefa.

> [!NOTE]
> As etapas listadas variam de acordo com o status do trabalho do Copilot e a abordagem que ele adotou.

4. Observe também o painel de pull request (PR), que aparece à direita. Ele permite ver a PR e os arquivos alterados para acompanhar o trabalho.

## Direcionar o agente na nuvem

Agora que você viu as tarefas em andamento, vamos pedir ao Copilot que inclua uma opção de modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão que trata da adição do modo de alto contraste. O título exato varia de acordo com o nome usado pelo Copilot e o status atual do trabalho.

    ![Sessão de acessibilidade no centro de controle](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até que ela indique que a configuração foi concluída e o trabalho começou. Você saberá que isso aconteceu quando aparecerem mensagens semelhantes às abaixo.
3. Na caixa de diálogo **Steer active session while Copilot is working**, adicione o seguinte prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de tela da tarefa do agente na nuvem na página de agentes, com a caixa de diálogo steer active session while copilot is working destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe como o Copilot confirma o recebimento do prompt e o inclui no fluxo de trabalho.

## Deixe o Copilot trabalhar

Como antes, o Copilot começará a trabalhar na tarefa atualizada. Ele incorporará a nova solicitação ao fluxo assim que concluir a etapa que estava executando quando você enviou a mensagem.

Como essa tarefa levará alguns minutos, aproveite para fazer uma pausa e refletir sobre o que aprendeu até agora.

## Resumo e próximas etapas

Nesta lição, você explorou a página de agentes do Copilot, seu centro de controle para monitorar e orientar as tarefas do agente na nuvem do GitHub Copilot. Nesse espaço de controle, você pode acompanhar todas as tarefas ativas e concluídas, revisar o trabalho realizado e redirecionar tarefas em andamento para ajustar o escopo ou fornecer orientações adicionais.

Você explorou estes conceitos:

- explorar o centro de controle e a página de agentes para monitorar tarefas do agente na nuvem.
- redirecionar uma sessão em andamento para solicitar funcionalidades adicionais.

Enquanto o Copilot conclui o trabalho nos recursos de acessibilidade, vamos [iterar sobre as pull requests criadas pelo Copilot][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
