---
title: "Lição 8 - Criar e mesclar a PR da funcionalidade"
description: "Revise em conjunto a filtragem, as instruções, a atualização da skill, o perfil de QA e os testes; depois crie uma PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

A implementação da filtragem, as atualizações das instruções, a atualização da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma única ramificação. É hora de revisar tudo em conjunto e abrir uma pull request. Você mesmo mesclou a pull request (PR) das avaliações por estrelas; desta vez, permitirá que o **Agent Merge** gerencie o processo.

> [!NOTE]
> Em geral, dividiríamos a funcionalidade, as atualizações das instruções, a atualização da skill e o agente de QA em algumas PRs separadas. Para simplificar o workshop, mantivemos todo o fluxo de filtragem e qualidade em uma conversa e ramificação, e incluiremos todo esse trabalho nesta PR.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de vida da mesclagem.
- inspecionar toda a alteração da funcionalidade e as evidências de verificação.
- criar a PR de filtragem.
- habilitar o Agent Merge somente depois da revisão e confirmar que a PR foi mesclada.

## Cenário

Ao longo do fluxo de filtragem, você usou o Copilot para planejar, implementar e verificar uma funcionalidade. Agora a Tailspin Toys quer automatizar o trabalho restante da PR, mantendo a autorização para mesclar sob o controle de quem desenvolve.

## Conhecer o Agent Merge

O **Agent Merge** automatiza o trabalho restante necessário para concluir uma pull request. Quando você o habilita, o Copilot resolve o que estiver impedindo a PR — corrige verificações de integração contínua (CI) com falha, responde a comentários de revisão e faz rebase quando necessário — e, em seguida, habilita a mesclagem automática do GitHub quando o repositório permite.

Até agora, você selecionou **Merge pull request** manualmente. O Agent Merge pode assumir essa responsabilidade. Revise o trabalho e confirme que está pronto antes de habilitar o Agent Merge.

## Usar o Agent Merge para gerenciar a PR

Com todo o código criado, vamos revisá-lo em conjunto, criar a PR e deixar o Agent Merge gerenciar o restante do processo.

1. Volte ao Codespace.
2. Abra a caixa de diálogo do agente digitando `/agent`.
3. Na lista de opções, selecione **Default** e pressione <kbd>Enter</kbd>.
4. Crie uma nova PR com o comando `/pr create`.
5. Ative o Agent Merge com o comando `/pr agentmerge`.
6. O Copilot acompanhará a integração contínua na PR. Quando tudo for aprovado, ele fará a mesclagem.
7. Confirme que você recebeu uma mensagem do Copilot semelhante a "PR #14 was squash-merged successfully."

> [!IMPORTANT]
> O Agent Merge não ignora aprovações obrigatórias, proteção de ramificações, filas de mesclagem, configurações do repositório nem permissões ausentes. Se o processo for bloqueado, leia o motivo informado e conclua manualmente a mesclagem revisada quando o repositório permitir.

## Resumo e próximos passos

Você automatizou várias partes do processo de desenvolvimento, incluindo a geração de código, os testes e a validação, e agora o processo de pull request. Você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de vida da mesclagem.
- inspecionou toda a alteração da funcionalidade e as evidências de verificação.
- criou a PR de filtragem.
- habilitou o Agent Merge somente depois da revisão e confirmou que a PR foi mesclada.

Em seguida, você [explorará outros comandos de barra úteis do Copilot CLI][next-lesson] para contexto, modelos, compartilhamento e delegação opcional à nuvem.

## Recursos

- [Gerenciar pull requests com o Copilot CLI][manage-prs]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
