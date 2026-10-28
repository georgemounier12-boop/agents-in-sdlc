---
title: "Exercício 1 - Instruções personalizadas (agente na nuvem)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Agente na nuvem do Copilot →][next-lesson]

O contexto é fundamental ao trabalhar com IA generativa. Se uma tarefa precisar ser feita de uma maneira específica — ou houver informações de contexto que o Copilot deva conhecer —, esse contexto precisa estar disponível. Os [arquivos de instruções][instruction-files] fornecem essas orientações para que o Copilot entenda não só *o que* você quer que ele faça, mas também *como*.

Neste exercício, você vai:

- explorar como o contexto específico do projeto, as diretrizes de codificação e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e de arquivos de instruções com escopo de caminho.
- adicionar um novo padrão para todo o repositório em `.github/copilot-instructions.md`.

> [!NOTE]
> Ao contrário dos roteiros do VS Code e do CLI, aqui você não usará um prompt *antes e depois*. O agente na nuvem do Copilot trabalha de forma assíncrona com issues do GitHub, por isso é mais difícil demonstrar o impacto lado a lado, em tempo real. Você verá a influência do arquivo de instruções mais adiante neste roteiro, ao revisar as pull requests produzidas pelo agente na nuvem.

## Arquivos de instruções

### Cenário

Como toda boa equipe de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para suas práticas de desenvolvimento. Entre eles:

- A camada de dados sempre precisa de testes de unidade.
- A interface do usuário deve usar o modo escuro e ter uma aparência moderna.
- A documentação deve ser adicionada ao código na forma de comentários TSDoc.
- Um bloco de comentários no início de cada arquivo deve descrever o propósito do arquivo.

Com os arquivos de instruções, você garantirá que o Copilot tenha as informações necessárias para executar as tarefas de acordo com essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor seu estilo de codificação e seus requisitos. Esse recurso permite orientar o Copilot para gerar sugestões e trechos de código mais relevantes. Você pode especificar convenções de codificação, bibliotecas e até os tipos de comentários que prefere. É possível criar instruções para todo o repositório ou para tipos específicos de arquivo, como contexto da tarefa.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`, um único arquivo de instruções enviado ao Copilot em **todas** as solicitações do repositório. Esse arquivo deve conter informações sobre o projeto — contexto relevante para a maioria das solicitações enviadas pelo chat ou pela CLI. Isso pode incluir a pilha de tecnologia, uma visão geral do que está sendo criado, práticas recomendadas e outras orientações globais.
- Arquivos `.github/instructions/*.instructions.md`, que podem ser criados para tarefas ou tipos de arquivo específicos. Você pode usá-los para fornecer diretrizes para linguagens específicas, como TypeScript ou Astro, ou para tarefas como criar um componente de interface ou um conjunto de novos testes de unidade.

> [!NOTE]
> No IDE, os arquivos de instruções são usados somente para a geração de código no Copilot Chat — não para conclusões de código nem sugestões de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente na nuvem do Copilot usam tanto o arquivo de instruções do repositório quanto os arquivos `*.instructions.md` com frontmatter `applyTo`.
>
> Por fim, o Copilot [oferece suporte a arquivos de instruções que usam outros padrões][custom-instructions-support], como arquivos AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

Uma conversa completa sobre a criação de arquivos de instruções está fora do escopo deste workshop. No entanto, os exemplos do projeto demonstram uma abordagem representativa. Em linhas gerais:

- Mantenha o arquivo `copilot-instructions.md` focado em orientações para o projeto, como uma descrição do que está sendo criado, a estrutura do projeto e os padrões globais de codificação.
- Use arquivos `*.instructions.md` para tipos de arquivo (testes de unidade, componentes Astro, camada de dados) ou tarefas específicas.
- Use linguagem natural e mantenha as orientações claras. Inclua exemplos de como o código deve (e não deve) ser escrito.

Não há uma única maneira de criar arquivos de instruções ou de usar IA. Com a experimentação, você descobrirá o que funciona melhor para seu projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter um conjunto abrangente de arquivos de instruções. Ao explorar os arquivos deste projeto, você poderá encontrar instruções para vários tipos de tarefa, incluindo [atualizações de interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada interface apresenta esse recurso de uma maneira (por exemplo, **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI). A lição correspondente mostrará como usá-lo.
>
> Para encontrar modelos ou um ponto de partida, explore o [awesome-copilot][awesome-copilot], um repositório com arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar os arquivos de instruções personalizadas deste projeto

Reserve um momento para ler os arquivos de instruções que acompanham este repositório: há um arquivo principal, `copilot-instructions.md`, e um conjunto de arquivos `*.instructions.md` para várias tarefas. Abra esses arquivos no editor ou na interface Web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Explore o arquivo e observe a breve descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas de **GitHub Actions Workflows**. Elas se aplicam a todas as interações com o Copilot.
3. Abra a pasta `.github/instructions` e explore os arquivos. Observe as instruções para arquivos Astro, a camada de dados Drizzle, testes e muito mais.
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início do arquivo. Ele define um glob (relativo à raiz do repositório) que determina os arquivos aos quais as instruções se aplicam. Neste caso, qualquer arquivo de teste TypeScript — por exemplo, um que corresponda a `**/*.test.ts` — será incluído.
5. Observe as instruções específicas para a criação de testes de unidade neste projeto.
6. Por fim, abra `.github/instructions/drizzle.instructions.md` e role até o final. Observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes do projeto. Assim, você pode dividir conjuntos maiores de instruções em arquivos menores e reutilizáveis e indicar ao Copilot exemplos para seguir durante a geração de código. Os caminhos nesse arquivo são relativos ao próprio arquivo de instruções, e não à raiz do repositório.

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de codificação do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentários nos arquivos.

## Adicionar um novo padrão para o repositório

Agora você fará a única edição deste exercício: adicionar ao padrão do projeto a exigência de comentários TSDoc e um cabeçalho de comentários no nível do arquivo. O agente na nuvem usará essa orientação ao trabalhar nas issues que você atribuir mais adiante neste roteiro.

Antes de editar, crie uma ramificação (o agente na nuvem lerá as instruções da ramificação de destino da issue, mas você também fará commit das alterações de forma organizada):

1. No terminal do codespace, crie e alterne para uma nova ramificação:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como mencionado anteriormente, `.github/copilot-instructions.md` fornece informações do projeto ao Copilot. Vamos documentar os padrões de codificação do repositório para melhorar as sugestões de código.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Observe que ela documenta os padrões de codificação, mas ainda não há uma regra de documentação no código. Por isso, o auxiliar gerado não tinha comentários de documentação.
3. Adicione as linhas Markdown a seguir logo abaixo dos padrões existentes, para orientar o Copilot a adicionar cabeçalhos de comentários aos arquivos e comentários TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, os arquivos de instruções podem ser criados no nível do repositório (`.github/copilot-instructions.md`) para orientações globais ou como arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas. O arquivo do repositório é o local adequado para padrões gerais, como a regra sobre comentários de documentação que você acabou de adicionar.

## Fazer commit, enviar por push e mesclar a atualização das instruções

O agente na nuvem lê os arquivos de instruções da ramificação de destino da issue. Ao atribuir uma issue ao Copilot no próximo exercício, ele criará uma ramificação baseada em `main`. Portanto, para que o agente na nuvem use suas alterações, elas precisam estar em `main`.

1. Prepare e faça commit:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envie a ramificação:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. No github.com, abra uma pull request de `custom-instructions-cloud` para `main` e faça a mesclagem. Assim, o agente na nuvem poderá usar essas instruções nas issues atribuídas a ele no próximo exercício.

> [!TIP]
> Se preferir trabalhar diretamente em `main` durante este workshop, ignore as etapas da ramificação e faça o commit diretamente em `main`. A etapa de ramificação demonstra como você faria esse trabalho em um projeto real.

## Resumo e próximas etapas

Você explorou como o Copilot obtém contexto dos arquivos de instruções deste projeto e adicionou um novo padrão para todo o repositório em `.github/copilot-instructions.md`. Você verá esse padrão nas pull requests geradas pelo agente na nuvem ao longo do restante deste roteiro.

Em seguida, você [atribuirá sua primeira issue ao agente na nuvem do Copilot][next-lesson].

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções personalizadas melhores para o Copilot][copilot-instructions-five-tips]
- [Awesome Copilot — uma coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
