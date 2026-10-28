---
title: "Ejercicio 3 - Prueba la funcionalidad con el servidor Playwright MCP"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo Agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Acabas de crear la funcionalidad de filtrado con el modo Agente. Antes de abrir una solicitud de cambios, confirma que funciona en el navegador. En vez de probar la aplicación manualmente, conectarás el **servidor Playwright MCP** y dejarás que Copilot use un navegador real para probarla. Después publicarás la rama y abrirás la solicitud de cambios.

En este ejercicio:

- aprenderás qué es Model Context Protocol (MCP) y cómo amplían los servidores MCP las herramientas de Copilot.
- añadirás el servidor Playwright MCP al espacio de trabajo.
- pedirás a Copilot que lo use para probar manualmente la funcionalidad de filtrado en un navegador.
- publicarás la rama y abrirás una solicitud de cambios para el trabajo de filtrado.

## ¿Qué es Model Context Protocol (MCP)?

El modo Agente es mucho más potente cuando puede acceder a recursos externos al editor. Model Context Protocol (MCP) permite que Copilot lo haga: es un estándar que permite al agente comunicarse con herramientas y servicios externos.

![Diagrama del funcionamiento del modo Agente y su interacción con el contexto, el LLM y las herramientas, incluidas las de servidores MCP y extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite a los agentes de IA comunicarse con herramientas y servicios externos. Así pueden acceder a información actualizada mediante recursos y realizar acciones en tu nombre mediante herramientas.

Los servidores MCP dan acceso a estos recursos y herramientas. Actúan como puente entre el agente de IA y los servicios externos, como las API existentes o herramientas locales como los paquetes de NPM. Cada servidor ofrece un conjunto distinto de herramientas y recursos.

Algunos servidores MCP populares:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: proporciona acceso a API para administrar repositorios, incidencias y solicitudes de cambios de GitHub.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: proporciona automatización del navegador con Playwright, por ejemplo, para visitar páginas, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP que ofrecen herramientas y recursos diferentes. GitHub aloja un [registro de MCP](https://github.com/mcp) para facilitar su descubrimiento y fomentar las contribuciones.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa su código fuente, comprueba quién lo publica y ten en cuenta sus implicaciones de seguridad. Usa únicamente servidores de confianza y sé prudente al conceder acceso a recursos u operaciones sensibles.

## Revisa la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en tu Codespace.
2. Verás que ya hay un servidor `github` configurado:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

La plantilla del proyecto incluye esta entrada `github`, por lo que Copilot pudo leer la lista de incidencias en el ejercicio anterior. Usa el [servidor GitHub MCP remoto][remote-github-mcp-server], así que no hay que instalarlo localmente. VS Code se conecta mediante HTTP y la primera vez que Copilot usa una de sus herramientas te autenticas con GitHub.

## Añade el servidor Playwright MCP

Ahora añadirás otro servidor. El [servidor Playwright MCP][playwright-mcp-server] proporciona a Copilot un navegador controlable que podrás usar para probar la funcionalidad.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github` para que el archivo quede así:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. Guarda el archivo.

A diferencia del servidor remoto `github`, `playwright` es local: VS Code lo inicia en el ordenador mediante `npx @playwright/mcp@latest`. La opción `--headless` indica a Playwright que ejecute el navegador sin una ventana visible, algo necesario en un Codespace, que no tiene escritorio.

> [!NOTE]
> El proyecto Tailspin Toys ya usa Playwright para las pruebas de extremo a extremo, así que normalmente el navegador necesario ya está instalado. Si Copilot indica que falta un navegador, pídele que ejecute `npx playwright install chromium` y vuelva a intentarlo.

## Inicia el servidor y confía en él

VS Code inicia los servidores MCP cuando se necesitan: la primera vez que Copilot solicita sus herramientas, VS Code los inicia y te pregunta si confías en ellos.

1. En `.vscode/mcp.json`, aparecerá la acción **Start** sobre la entrada `playwright`. Selecciónala para iniciar el servidor y confirmar que la configuración es correcta.
2. Si VS Code te pregunta si confías en el servidor, revisa la configuración y confirma que confías en él para que se inicie.

Iniciarlo manualmente es opcional. Si no lo haces, VS Code lo iniciará y te pedirá confirmación la primera vez que Copilot necesite sus herramientas en el paso siguiente.

## Prueba la funcionalidad de filtrado

El [servidor Playwright MCP][playwright-mcp-server] proporciona a Copilot un navegador real. En vez de probar la aplicación manualmente, el agente puede abrir una página, navegar por ella, aplicar filtros y leer los resultados para resumir lo que observa. Es una forma rápida de confirmar que la funcionalidad funciona sin salir de la conversación.

Internamente, el servidor Playwright MCP usa el [árbol de accesibilidad][playwright-mcp-server] de la página en lugar de capturas de pantalla. El agente trabaja con elementos estructurados y etiquetados (botones, enlaces y elementos de lista), como las tecnologías de asistencia. Así, una prueba funcional también sirve como comprobación básica de accesibilidad.

Con el servidor conectado y la aplicación en ejecución, pide a Copilot que pruebe la funcionalidad de filtrado:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot abrirá un navegador mediante el servidor Playwright MCP, seguirá los pasos y te informará de lo que encuentre. Compara el resultado con los criterios de aceptación de la incidencia. Si algo no funciona bien, haz preguntas de seguimiento o pide que corrija el código antes de abrir una solicitud de cambios.

> [!NOTE]
> La aplicación debe estar en ejecución en `http://localhost:4321`. Si detuviste el servidor de desarrollo, vuelve a iniciarlo antes de enviar la solicitud. La primera vez que Copilot use el servidor Playwright MCP, quizá tenga que descargar un navegador. Si indica que falta uno, pídele que ejecute `npx playwright install chromium` y vuelva a intentarlo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publica la rama y crea una solicitud de cambios

Ahora que has confirmado que la funcionalidad funciona, puedes abrir una solicitud de cambios (PR) para que el equipo la revise. Primero publicarás la rama `filtering-vscode`.

1. Abre el panel **Source Control** del Codespace y revisa los cambios de Copilot.
2. Prepara los cambios seleccionando el icono **+**.
3. Genera un mensaje de confirmación con el botón **Sparkle**.

   ![Captura de pantalla del panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para enviar la rama al repositorio.

## Crea la solicitud de cambios

Hay varias formas de crear una solicitud de cambios, por ejemplo, en github.com o con la interfaz de línea de comandos (CLI) de GitHub. Como ya estás trabajando con GitHub Copilot, deja que la cree por ti: puede encontrar la incidencia correspondiente y vincularla a la solicitud.

1. Ve al panel Copilot Chat y selecciona **New Chat** para iniciar una sesión nueva.
2. Comprueba que **Agent** está seleccionado para que Copilot pueda usar las herramientas de GitHub.
3. Pídele a Copilot que cree una solicitud:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Si es necesario, selecciona **Continue** para permitir que Copilot recopile información y realice operaciones. La primera vez que use una herramienta de GitHub, puede que tengas que autenticarte. Sigue las instrucciones para autorizarlo.
5. Observa cómo Copilot busca las incidencias, encuentra la correcta y crea la solicitud.
6. Selecciona el enlace de Copilot para revisar la solicitud, pero **no la combines todavía**.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio:

- has aprendido qué es Model Context Protocol (MCP) y cómo amplían los servidores MCP las herramientas de Copilot.
- has añadido el servidor Playwright MCP al espacio de trabajo.
- lo has usado para probar la funcionalidad de filtrado en un navegador.
- has publicado la rama y abierto una solicitud de cambios.

Has usado un servidor MCP para *probar* una funcionalidad, pero MCP es solo una forma de ampliar Copilot. A continuación, [crearás un agente personalizado][next-lesson] para agilizar tareas concretas como las revisiones de accesibilidad.

## Recursos

- [Qué es MCP y por qué todo el mundo habla de ello][mcp-blog-post]
- [Servidor Playwright MCP de Microsoft][playwright-mcp-server]
- [Servidor GitHub MCP][github-mcp-server]
- [Registro de GitHub MCP][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
