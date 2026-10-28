---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Modo de agente →][next-lesson]

O contexto é fundamental ao trabalhar com IA generativa. Se uma tarefa precisar ser feita de uma maneira específica — ou houver informações de contexto que o Copilot deva conhecer —, esse contexto precisa estar disponível. Há várias maneiras de compartilhar informações específicas com o Copilot. Uma das principais são os [arquivos de instruções][instruction-files], que fornecem orientações sobre a geração de código.

Neste exercício, você vai:

- explorar como o contexto específico do projeto, as diretrizes de codificação e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e de arquivos de instruções com escopo de caminho.
- gerar a primeira parte dos dados para a filtragem (um auxiliar de editoras) usando as instruções *atuais*.
- adicionar um novo padrão para todo o repositório a `.github/copilot-instructions.md`.
- executar novamente o mesmo prompt e observar o código gerado adotar o novo padrão.
- fazer commit das atualizações das instruções e da parte da filtragem em `main` para que o Copilot possa usar as orientações atualizadas no próximo exercício.

> [!CAUTION]
> O código gerado pode não seguir alguns dos padrões definidos. O Copilot é não determinístico. O objetivo deste exercício é observar a *tendência* de mudança no comportamento após a atualização das instruções, não obter resultados idênticos caractere por caractere.

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

- `.github/copilot-instructions.md`, um único arquivo de instruções enviado ao Copilot em **todas** as solicitações do repositório. Esse arquivo deve conter informações sobre o projeto — contexto relevante para a maioria das solicitações feitas ao Copilot por chat ou pela CLI. Isso pode incluir a pilha de tecnologia, uma visão geral do que está sendo criado, práticas recomendadas e outras orientações globais.
- Arquivos `.github/instructions/*.instructions.md`, que podem ser criados para tarefas ou tipos de arquivo específicos. Você pode usá-los para fornecer diretrizes para linguagens como TypeScript ou Astro, ou para tarefas como criar um componente de interface ou um novo conjunto de testes de unidade.

> [!NOTE]
> No IDE, os arquivos de instruções são usados somente para a geração de código no Copilot Chat — não para conclusões de código nem sugestões de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente na nuvem do Copilot usam tanto o arquivo de instruções do repositório quanto os arquivos `*.instructions.md` com frontmatter `applyTo`.
>
> Por fim, o Copilot [oferece suporte a arquivos de instruções que usam outros padrões][custom-instructions-support], como arquivos AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

Uma conversa completa sobre a criação de arquivos de instruções está fora do escopo deste workshop. No entanto, os exemplos do projeto demonstram uma abordagem representativa. Em linhas gerais:

- Mantenha o arquivo `copilot-instructions.md` focado em orientações para o projeto, como uma descrição do que está sendo criado, a estrutura do projeto e os padrões globais de codificação.
- Use arquivos `*.instructions.md` para tipos de arquivo específicos (testes de unidade, componentes Astro, camada de dados) ou tarefas.
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
5. Observe as instruções específicas para criar testes de unidade neste projeto.
6. Por fim, abra `.github/instructions/drizzle.instructions.md` e role até o final. Observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes do projeto. Assim, você pode dividir conjuntos maiores de instruções em arquivos menores e reutilizáveis e indicar ao Copilot exemplos para seguir durante a geração de código. Os caminhos nesse arquivo são relativos ao próprio arquivo de instruções, e não à raiz do repositório.

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de codificação do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentários nos arquivos.

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/

## Criar uma ramificação para as alterações

Vamos seguir algumas práticas recomendadas e criar uma ramificação para nossas alterações.

1. Volte ao codespace do exercício anterior.
2. Abra um novo terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie uma ramificação e mude para ela:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar o Copilot Chat *antes* de atualizar as instruções

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício — cada etapa indicará o que importa.

Para observar o impacto das instruções personalizadas, primeiro envie um prompt com o arquivo de instruções atual. Depois, você o atualizará e enviará o mesmo prompt novamente para comparar o resultado.

1. Feche as guias do editor abertas nos exercícios anteriores para que o Copilot obtenha apenas o contexto desejado.
2. Abra `src/lib/publishers.ts` para indicar onde o auxiliar deve ficar.
3. Na visualização Chat, selecione **Agent** no menu suspenso de agentes para que o Copilot possa aplicar alterações aos arquivos.

   ![Captura de tela mostrando o seletor de agente na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envie este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. O Copilot explora o projeto e aplica atualizações ao código, geralmente no arquivo auxiliar e nos testes.
6. Observe que a função proposta é tipada, recebe um cliente `db` como primeiro argumento e retorna uma matriz tipada de editoras. Essas características vêm das convenções da camada de dados em `.github/instructions/drizzle.instructions.md`, aplicáveis a `src/lib/*.ts`.
7. Observe que o código proposto **não inclui** comentários TSDoc nem um cabeçalho de comentários no nível do arquivo.

> [!CAUTION]
> Como o Copilot é probabilístico, talvez ele adicione comentários mesmo sem a instrução explícita. Tudo bem — o objetivo é observar a melhoria da *consistência* depois da atualização das instruções.

## Adicionar um novo padrão para o repositório

Como mencionado, `.github/copilot-instructions.md` fornece orientações sobre o projeto ao Copilot. Vamos documentar os padrões de codificação do repositório para melhorar as sugestões de código.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Observe que ela documenta os padrões de codificação do projeto, mas ainda não inclui uma regra de documentação no código — por isso, o auxiliar gerado não tinha comentários de documentação.
3. Adicione as linhas Markdown a seguir logo abaixo dos padrões existentes, para orientar o Copilot a adicionar cabeçalhos de comentários aos arquivos e comentários TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, os arquivos de instruções podem ser criados no nível do repositório (`.github/copilot-instructions.md`) para orientações globais ou como arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas. O arquivo do repositório é o local adequado para padrões gerais, como a regra sobre comentários de documentação que você acabou de adicionar.

## Executar novamente o prompt e observar a alteração

1. Volte ao Copilot Chat e selecione **New Chat** para limpar o histórico.
2. Selecione novamente `src/lib/publishers.ts` para que o Copilot se concentre no arquivo correto.
3. Envie o mesmo prompt de antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observe que o arquivo proposto agora começa com um bloco de comentários semelhante a este:

   ```typescript
   /**
    * Auxiliares de acesso a dados de editoras da plataforma de financiamento coletivo da Tailspin Toys.
    * Fornece funções para recuperar informações de editoras do banco de dados.
    */
   ```

5. Observe que a função proposta agora inclui um comentário TSDoc semelhante a este:

   ```typescript
   /**
    * Retorna uma lista de todas as editoras com seus IDs e nomes.
    *
    * @param db - Cliente do banco de dados Drizzle.
    * @returns Uma promessa resolvida com um array de objetos de editoras.
    */
   ```

Você acabou de orientar o Copilot a seguir um novo padrão do projeto e aplicá-lo ao código que servirá de base para o próximo exercício.

## Fazer commit das instruções e enviar a ramificação

Os arquivos de instruções são ativos do projeto como qualquer outro e devem ser gerenciados pelo controle de versão. Vamos fazer commit das alterações e enviar a ramificação ao repositório.

1. Abra uma nova janela de terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. No terminal, confirme que as atualizações das instruções e do auxiliar estão presentes:

   ```bash
   git status
   ```

3. Prepare e faça commit das atualizações das instruções e de todos os arquivos alterados pelo Copilot para criar a base do auxiliar:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Se git status mostrar outras atualizações relacionadas (por exemplo, testes), adicione esses arquivos também.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envie a ramificação ao repositório:

   ```bash
   git push -u origin custom-instructions
   ```

## Criar e mesclar uma pull request

Com a ramificação enviada, devemos criar uma pull request e associá-la à issue sobre o padrão de documentação no backlog. Em vez de fazer isso manualmente, podemos pedir ao Copilot que encontre a issue, crie a PR para fechá-la e depois a mescle.

> [!NOTE]
> Este é o primeiro exercício em que o Copilot agirá no GitHub por você. O modelo do projeto já conecta as ferramentas do GitHub ao seu espaço de trabalho, então não é necessário configurar nada — na primeira vez que o Copilot usar uma ferramenta, o VS Code talvez peça que você entre no GitHub. Siga as instruções para permitir o acesso. Você aprenderá como essa conexão funciona (Model Context Protocol) em um exercício futuro.

1. Abra o Copilot Chat no codespace.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a visualização do Copilot Chat e confirme que **Agent** está selecionado no seletor de agentes.
3. Peça ao Copilot que encontre a issue relacionada à atualização dos arquivos de instruções e crie uma PR a partir da ramificação atual, descrevendo tanto as atualizações das instruções quanto a nova base do auxiliar de editoras:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. O Copilot começará a localizar a issue e criar a PR.
5. Quando o Copilot pedir permissão para executar ações no GitHub, revise o comando e selecione **Allow** se for apropriado.
6. Depois que a PR for criada, peça ao Copilot que a mescle em `main` e retorne sua ramificação local para `main`:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Quando o Copilot pedir permissão para executar ações no GitHub e comandos do shell, revise o comando e selecione **Allow** se for apropriado.

Você acaba de criar e mesclar uma pull request com a ajuda do GitHub Copilot!

## Resumo e próximos passos

Você explorou como o Copilot obtém contexto dos arquivos de instruções deste projeto e, em seguida, usou o Copilot Chat no VS Code para:

- enviar um prompt de geração de código e observar o resultado com as instruções *existentes*.
- adicionar um novo padrão para todo o repositório a `.github/copilot-instructions.md`.
- executar novamente o mesmo prompt e observar o código proposto adotar o novo padrão.
- fazer commit das atualizações das instruções e da base do auxiliar em `main` para que o próximo exercício possa usá-las.

Em seguida, você colocará as instruções em prática no [modo de agente][next-lesson], enquanto o Copilot adiciona uma nova funcionalidade à base de código.

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções personalizadas melhores para o Copilot][copilot-instructions-five-tips]
- [Instruções pessoais personalizadas para o GitHub Copilot][personal-instructions]
- [Awesome Copilot — uma coleção de arquivos de instruções e outros recursos][awesome-copilot]

[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
