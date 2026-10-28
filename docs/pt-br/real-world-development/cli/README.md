---
slug: pt-br/real-world-development/cli
title: "CLI do GitHub Copilot"
description: "Crie, verifique e entregue duas alterações no Tailspin Toys enquanto aprende sobre modos, personalizações, ferramentas MCP e automação de pull requests no Copilot CLI."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

O **[GitHub Copilot CLI][about-copilot-cli]** leva o GitHub Copilot ao terminal como um assistente de programação baseado em agentes. Ele explora bases de código, gera código, executa comandos e se conecta a ferramentas externas — tudo pela linha de comando, para que você continue trabalhando sem alternar para um editor gráfico.

O workshop acompanha um fluxo contínuo de trabalho no Tailspin Toys:

1. Prepare o projeto no GitHub Codespaces, instale o Copilot CLI e conheça o projeto.
2. Faça uma alteração focada nas avaliações por estrelas, revise-a no navegador e mescle manualmente sua primeira pull request (PR).
3. Comece pela issue de filtragem, defina a abordagem no modo Plan, desenvolva no modo Autopilot e revise no modo Interactive.
4. Atualize as instruções do repositório e aplique-as ao trabalho de filtragem.
5. Personalize a skill existente `quality-checks` e use-a para executar as verificações do projeto.
6. Adicione o servidor Model Context Protocol (MCP) do Playwright e use-o para explorar a filtragem em um navegador.
7. Crie um agente personalizado de garantia de qualidade (QA) e use-o para revisar requisitos, cobertura e evidências de verificação.
8. Revise toda a alteração de filtragem e use o Agent Merge na PR da filtragem.
9. Explore comandos de barra úteis para contexto, modelos, compartilhamento e delegação opcional à nuvem.

Para manter o foco do workshop, você criará duas PRs: uma para avaliações por estrelas e outra para filtragem, com as atualizações de instruções, da skill, do perfil de QA e dos testes. Comece cada uma a partir da `main` atualizada. O trabalho de filtragem e qualidade compartilha uma conversa e uma branch para que você possa aproveitar o que já fez enquanto explora cada ferramenta.

## Lições

| Lição | Tópico | Descrição |
| ------ | ----- | ----------- |
| [0. Pré-requisitos][ex0] | Configuração | Crie seu repositório e codespace |
| [1. Instalar o Copilot CLI][ex1] | Instalação | Instale e autentique o Copilot CLI e conheça os recursos |
| [2. Adicionar avaliações por estrelas: uma melhoria rápida][ex2] | Primeira alteração | Exiba as avaliações existentes e a alternativa para valores nulos e mescle sua primeira PR |
| [3. Modos de agente: Plan e Autopilot][ex3] | Modos de agente | Planeje o recurso com base na issue, desenvolva com Autopilot e revise no modo Interactive |
| [4. Orientar o Copilot com instruções personalizadas][ex4] | Contexto | Explore e atualize as instruções e aplique-as à filtragem |
| [5. Personalizar e usar uma skill de verificações de qualidade][ex5] | Verificações repetíveis | Explore a skill existente, altere o formato do relatório e execute-a |
| [6. Validar a funcionalidade com o MCP do Playwright][ex6] | Observação no navegador | Configure o MCP na CLI e examine o comportamento da filtragem |
| [7. Criar e usar um agente de QA][ex7] | Requisitos e cobertura | Crie e selecione um perfil especializado e reúna evidências finais de verificação |
| [8. Criar e mesclar a PR do recurso][ex8] | Revisão e merge | Revise toda a alteração, crie a PR e use o Agent Merge |
| [9. Comandos de barra do GitHub Copilot CLI][ex9] | Recursos da CLI | Explore contexto, modelos, compartilhamento e delegação opcional ao agente na nuvem |
| [10. Encerramento e próximos passos][ex10] | Resumo | Revise o fluxo de trabalho, as personalizações reutilizáveis e outros recursos |
| [Opcional: Incorporar o Foundry][foundry] | Agentes hospedados | Prepare um modelo, implante um agente baseado no catálogo e conecte-o ao site |

## Pré-requisitos

Antes de participar deste workshop, verifique se você tem:

- [ ] Uma conta do GitHub com um plano ativo **Copilot Student, Pro, Pro+, Business ou Enterprise**
- [ ] Permissão para criar um repositório e um codespace
- [ ] Familiaridade básica com operações de terminal ou linha de comando

> [!TIP]
> Não tem um plano pago? Estudantes com status verificado podem usar o GitHub Copilot gratuitamente pelo [GitHub Education][student-plan]. O plano **Copilot Student** inclui os recursos de agente, MCP, revisão de código e Copilot CLI usados neste workshop.

> [!NOTE]
> Se você usa o Copilot Business ou o Copilot Enterprise, verifique se o administrador habilitou o Copilot CLI para uso.

## Começar

[Comece pelos pré-requisitos →][ex0]

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
