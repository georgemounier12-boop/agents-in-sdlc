---
title: "Exercício 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot permitem criar assistentes de IA especializados para tarefas ou domínios específicos do seu fluxo de desenvolvimento. Ao definir agentes em arquivos Markdown na pasta `.github/agents` do repositório, você pode fornecer instruções direcionadas, práticas recomendadas, padrões de código e conhecimento do domínio para orientar o Copilot a realizar determinados tipos de trabalho com mais eficiência. As equipes podem registrar seu conhecimento em agentes reutilizáveis — por exemplo, um agente de acessibilidade que verifica a conformidade com as diretrizes [WCAG][wcag], um agente de segurança que segue práticas de codificação segura ou um agente de testes que mantém padrões consistentes.

Os agentes personalizados são definidos em arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos os campos `name` e `description`, seguido de um prompt Markdown que define o comportamento, a especialidade e as instruções do agente.

### Comparação entre agentes personalizados e skills de agente

Há alguma sobreposição entre agentes personalizados e [skills de agente][agent-skills-concept]. Ambos são definidos principalmente em arquivos Markdown e ensinam a IA a executar operações. A distinção mais simples é: um **agente personalizado** é o executor, enquanto as **skills** são ferramentas.

Os agentes personalizados têm sua própria janela de contexto e são criados para orquestrar skills (e até outros agentes) durante o trabalho. Neste laboratório, o agente personalizado de acessibilidade revisará e atualizará o site conforme as diretrizes de acessibilidade. Como parte desse trabalho, ele poderá chamar skills, por exemplo, uma skill de fluxo de trabalho de pull request ou outra que execute e gerencie testes.

> [!NOTE]
> Não existe uma única forma "correta" de criar um agente personalizado. Como acontece com qualquer recurso de IA, teste e itere para encontrar o que funciona nos seus ambientes e cenários.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Cenário

A Tailspin Toys está comprometida em tornar sua plataforma de financiamento coletivo acessível a todos, independentemente da capacidade visual ou das preferências de cada pessoa. Comentários recentes de usuários apontaram que o tema escuro atual é difícil de ler devido ao contraste insuficiente entre as cores do texto e do plano de fundo. Para resolver essa questão de acessibilidade, a equipe de design solicitou a implementação de um modo de alto contraste que os usuários possam ativar e desativar.

Como a acessibilidade é fundamental, queremos implementar esse recurso rapidamente. Você usará um agente personalizado para gerar a funcionalidade.

Neste exercício, você vai:

- revisar um agente personalizado de acessibilidade existente.
- usar o agente de acessibilidade no Copilot Chat para implementar um modo de alto contraste.

## Revisar o agente personalizado de acessibilidade

Um agente personalizado de acessibilidade já foi criado para você. Vamos revisar o conteúdo para entender como ele orientará o Copilot.

Volte ao codespace e, em seguida, abra um terminal e crie uma nova ramificação baseada em `main` para o trabalho de acessibilidade. Assim, a pull request de filtragem do Exercício 3 permanecerá separada:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abra `.github/agents/accessibility.md`.
2. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> O frontmatter com os campos `name` e `description` é obrigatório para agentes personalizados.

3. Examine as seções seguintes, que destacam:
   - responsabilidades principais ao gerar código para um site acessível.
   - práticas recomendadas de acessibilidade.
   - exemplos de código para HTML, CSS e JavaScript.
   - uma lista de erros comuns e armadilhas.

## Usar o agente personalizado no Copilot Chat

O VS Code mostra todos os agentes personalizados definidos em `.github/agents` no seletor de agentes, na parte inferior da visualização Copilot Chat. Você pode selecionar um agente personalizado para limitar uma sessão de chat às instruções e ferramentas desse agente.

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e a seleção de modelo variam por exercício — cada etapa indica o que escolher quando for necessário.

1. Selecione **Agent** no seletor de agentes da visualização Chat, caso ainda não esteja selecionado.

   ![Captura de tela do seletor de agente na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecione o seletor de agentes na parte inferior da visualização Chat. Ele mostra o agente ativo; por padrão, é **default**.
3. Selecione **Accessibility agent** na lista de agentes disponíveis.
4. Envie o seguinte prompt ao agente de acessibilidade:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. O Copilot Chat começará a trabalhar: explorará a base de código, proporá alterações e editará os arquivos do projeto. Cada edição aparecerá na conversa com o caminho do arquivo e uma diferença para você revisar.
6. À medida que as edições forem aplicadas, o indicador **Files changed** será atualizado para mostrar o conjunto de arquivos modificados pelo agente.

> [!NOTE]
> Esse processo provavelmente levará alguns minutos. O Copilot está fazendo alterações reais no repositório — editará arquivos existentes, como componentes Astro, CSS e testes relacionados, conforme necessário.

Você revisará e orientará esse trabalho em andamento na próxima lição.

## Resumo e próximas etapas

Nesta lição, você explorou os [agentes personalizados][custom-agents] do GitHub Copilot, assistentes de IA especializados para tarefas e domínios específicos. Com agentes personalizados, você pode registrar o conhecimento e os padrões da equipe em agentes reutilizáveis que orientam o Copilot a realizar determinados tipos de trabalho com mais eficiência.

Você explorou estes conceitos:

- como os agentes personalizados são definidos.
- como usar um agente personalizado no modo de agente do Copilot Chat.

Em seguida, você [monitorará e direcionará o trabalho do agente][next-lesson], revisando as alterações à medida que são feitas e adicionando um controle para o modo claro à mesma sessão.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Criar agentes personalizados no IDE][creating-custom-agents-ide]
- [Agentes personalizados no VS Code][custom-agents-vscode]
- [Configuração de agentes personalizados][custom-agents-config]
- [Agentes personalizados no awesome-copilot][awesome-copilot-agents]

---

| [← Lição anterior: Testar seu recurso com o servidor MCP do Playwright][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
