---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Agentes personalizados][previous-lesson] |
|:--|

Quando você coloca o GitHub Copilot no modo de agente, ele trabalha de forma autônoma: explora a base de código, propõe alterações, edita arquivos e executa comandos. Como esse trabalho acontece em tempo real na sua máquina, o Copilot Chat no VS Code mostra cada chamada de ferramenta e edição de arquivo à medida que ocorre. Você pode revisar cada diferença no próprio chat, aceitar ou rejeitar alterações individuais e orientar a conversa com prompts de acompanhamento para refinar ou ampliar o trabalho sem sair do editor.

Neste exercício, você vai:

- monitorar em tempo real o trabalho do agente de acessibilidade.
- revisar e aceitar as edições de arquivo propostas.
- orientar o agente durante a sessão para adicionar um controle do modo claro junto com o modo de alto contraste.

## Cenário

O agente personalizado de acessibilidade usado no [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Enquanto o trabalho está em andamento, a equipe de design fez uma solicitação adicional: também gostaria de um controle do modo claro para usuários que consideram o tema padrão escuro demais. Em vez de começar uma nova conversa, você orientará a sessão existente para ampliar o trabalho.

## Monitorar o progresso do Copilot

A visualização Copilot Chat mostra cada etapa do agente: os arquivos que ele explora, os comandos que executa e as edições propostas. Cada edição aparece na conversa com uma diferença e os controles **Keep** e **Undo**.

1. Volte à visualização Copilot Chat do Exercício 4.
2. Enquanto o agente trabalha, percorra a conversa e acompanhe cada chamada de ferramenta. Você verá leituras e edições de arquivos e os comandos de terminal que ele solicitará para executar.
3. Quando o agente propuser uma edição, a diferença aparecerá na conversa:
   - Selecione **Keep** para aceitar a alteração.
   - Selecione **Undo** para reverter a alteração e permitir que o agente tente novamente.
4. Você também pode abrir o indicador **Files changed**, na parte superior do chat, para ver todos os arquivos modificados pelo agente durante a sessão.

> [!NOTE]
> Se uma chamada de ferramenta solicitar permissão (por exemplo, para executar um comando de terminal), o agente será pausado e exibirá uma solicitação **Allow**. Aprove somente os comandos que você considera seguros.

5. Aguarde até que o agente termine de implementar o modo de alto contraste. Uma mensagem final deverá resumir as alterações realizadas.

## Orientar o agente para adicionar o modo claro

Agora que o modo de alto contraste está pronto, você ampliará a mesma conversa para adicionar um controle do modo claro. Como você continuará na mesma sessão de chat, o agente manterá o contexto do trabalho anterior, incluindo os componentes alterados, a abordagem de estilo e o padrão de armazenamento local.

1. No campo de entrada do Copilot Chat, envie o seguinte prompt de acompanhamento:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. O agente continuará o trabalho, explorando as alterações existentes e propondo adições para o modo claro.
3. Revise cada nova edição à medida que ela aparecer, assim como fez com o modo de alto contraste. Use **Keep** ou **Undo** em cada diferença.
4. Se o agente seguir uma direção indesejada — por exemplo, se tentar refatorar os controles de modo de uma maneira que você não prefere —, pare-o com o botão **Stop** ao lado do campo de entrada do chat e envie uma mensagem de acompanhamento para esclarecer o que deseja.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que recomeçar. Use prompts de acompanhamento para solicitar ajustes ("simplifique o CSS — use variáveis CSS em vez de duplicar seletores"), corrigir o rumo ("não adicione um terceiro controle; integre o modo claro ao painel de configurações existente") ou ampliar o escopo ("adicione também atalhos de teclado para os dois controles").

## Revisar o conjunto de alterações

Antes de fazer commit do trabalho, revise rapidamente tudo o que o agente alterou.

1. Abra a visualização **Source Control** no VS Code.
2. Revise a lista completa de arquivos alterados. Você deverá ver atualizações nos componentes Astro, nos estilos e em testes relacionados.
3. Abra alguns dos arquivos alterados e examine as diferenças. Confirme se as práticas de acessibilidade do agente personalizado estão presentes: atributos ARIA, navegação pelo teclado, HTML semântico e persistência por meio do armazenamento local.
4. Quando estiver satisfeito, prepare e faça commit das alterações pela visualização Source Control. Você as publicará na [próxima lição][next-lesson].

## Resumo e próximas etapas

Você usou o modo de agente do Copilot Chat para monitorar e orientar uma sessão em andamento: revisou cada edição, aceitou ou rejeitou alterações e enviou prompts de acompanhamento para ampliar o trabalho sem perder o contexto.

Nesta lição, você explorou:

- como monitorar o trabalho de um agente em tempo real.
- como aceitar e rejeitar edições de arquivo individuais.
- como orientar uma sessão em andamento usando prompts de acompanhamento.

Na [próxima lição][next-lesson], você publicará seu trabalho de acessibilidade como uma pull request e revisará as alterações produzidas pelo agente personalizado local durante o laboratório.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
