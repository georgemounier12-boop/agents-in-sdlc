---
title: "Lição 12 - Publique o site com o GitHub Pages"
description: "Use o aplicativo GitHub Copilot para configurar um fluxo de trabalho do Pages, revisar a pull request e verificar o Space Quiz publicado."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Dê ao Space Quiz um endereço público publicando-o com o GitHub Pages. O Copilot pode preparar o fluxo de trabalho e orientar a implantação, mas você continua responsável por revisar as alterações e confirmar que o site está no ar.

Nesta lição, você vai:

- pedir ao Copilot que configure a implantação no GitHub Pages.
- revisar o fluxo de trabalho e a pull request antes da mesclagem.
- acompanhar a implantação e verificar o site publicado.

## Cenário

O Space Quiz já está em um repositório do GitHub, mas as pessoas ainda precisam de uma maneira simples de experimentá-lo. Publicar o site de arquivo único com o GitHub Pages disponibiliza o site em uma URL pública, sem adicionar um servidor ou dependência de runtime.

## Peça ao Copilot para publicar o site

Na sessão do aplicativo Copilot para `space-quiz`, envie este prompt:

```plaintext
Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![Diagrama em três etapas: uma pull request do space-quiz mesclada; uma execução bem-sucedida de Deploy website no GitHub Actions, com as etapas Build website, Upload artifact e Deploy to Pages; e o Space Quiz no endereço yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> Este prompt solicita que o agente crie e mescle uma pull request e altere as configurações do Pages do repositório. Revise as diferenças, as permissões de implantação e todas as solicitações de aprovação do GitHub antes de continuar.

## Revise o fluxo de trabalho e verifique a implantação

1. Confirme se o README foi adicionado somente quando o repositório ainda não tinha um.
2. Revise as alterações da pull request, incluindo as etapas de compilação e implantação do fluxo de trabalho.
3. Faça a mesclagem somente depois que as verificações obrigatórias da pull request forem aprovadas.
4. Acompanhe a execução de implantação do GitHub Actions até que ela seja concluída com êxito.
5. Abra a URL informada e confirme que o Space Quiz carrega.

Se o agente não puder habilitar o Pages ou mesclar a pull request, siga as instruções específicas que ele fornecer. Não considere a tarefa concluída até que a implantação seja bem-sucedida e o site esteja acessível.

## Resumo e próximas etapas

Agora o Space Quiz está disponível como um site publicado, e você verificou isso em vez de confiar apenas no relato do agente. Continue para [Lição 13: Revisão e próximas etapas][next-lesson].

[next-lesson]: ../13-review/
