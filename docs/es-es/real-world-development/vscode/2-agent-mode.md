---
title: "Ejercicio 2 - Añade funcionalidad con el modo Agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y operaciones como ejecutar pruebas. El flujo habitual de desarrollo implica localizar todos los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo falta, actualizarlo y volver a empezar.

El modo Agente de Copilot está diseñado para trabajar de forma más autónoma en el IDE. Se comporta de manera parecida a un desarrollador: primero explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Veamos cómo usarlo para añadir funcionalidad al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo Agente y el agente en la nube ofrecen experiencias distintas. El modo Agente trabaja en el IDE y permite ciclos de respuesta rápidos e interacción. El agente en la nube trabaja de forma asíncrona como un miembro del equipo y se comunica contigo mediante incidencias y solicitudes de cambios.

En este ejercicio aprenderás:

- cómo el modo Agente de Copilot explora el proyecto, identifica los archivos relevantes y coordina cambios.
- cómo puede implementar funcionalidades en la interfaz de usuario y la capa de datos.
- cómo revisar los cambios y las pruebas generados por el modo Agente antes de combinarlos en la base de código.

## Escenario

A medida que crece la lista de juegos, quieres permitir que las personas filtren por categoría y editorial. En el ejercicio anterior ya añadiste una función auxiliar para obtener editoriales; ahora completarás con el modo Agente de Copilot el resto del trabajo de datos, interfaz y pruebas.

## Ejecuta el sitio Tailspin Toys

Antes de hacer cambios, exploremos el sitio Tailspin Toys para conocer su funcionalidad actual.

El sitio es una plataforma de financiación colectiva de juegos de mesa con un tema de desarrollo. Permite mostrar juegos y consultar sus detalles. Es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local en el frontmatter mediante Drizzle ORM. No hay una API de backend independiente ni un framework de interfaz en el cliente. Las funciones reutilizables para acceder a los datos están en `src/lib/`; la interactividad se añade mediante un pequeño `<script>` de Astro con ámbito limitado y las API estándar del DOM.

### Inicia el sitio

Hay una tarea `npm run dev` que inicia el único servidor de desarrollo de Astro. Para ejecutarlo en tu GitHub Codespace:

1. Vuelve al Codespace. Después del ejercicio anterior deberías estar en `main`.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando se inicie el servidor de desarrollo de Astro, aparecerá un mensaje con la URL, parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo que te pide abrir una ventana del navegador para `http://localhost:4321`, ciérralo seleccionando **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En un Codespace, al seleccionar el enlace a la URL local en el terminal, se redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado al Codespace, que ahora aloja el servidor web.

### Explora el sitio

Las principales características del sitio son:

- **Página de inicio**: muestra juegos de mesa con títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explora las tareas pendientes con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando corresponda.

La primera versión del sitio funciona, pero queremos ampliarla. Empecemos por revisar la lista de tareas pendientes. Al crear el proyecto a partir de la plantilla, se generó una lista de incidencias automáticamente. Pide a GitHub Copilot que te muestre esos elementos.

1. En el menú desplegable de agentes de la vista Chat, selecciona **Agent**. El agente puede planificar e implementar cambios en varios archivos de forma autónoma, ejecutar comandos de terminal y usar herramientas.

   ![Captura de pantalla del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Las personas autoras del taller no recomiendan un modelo concreto. Para crear este taller se usó Claude Sonnet 4.5 y se incluye en las instrucciones para que las sugerencias de código sean relativamente coherentes y la experiencia sea predecible. Sin embargo, los LLM no son deterministas, por lo que las sugerencias pueden diferir de las descritas. Es normal.

> [!NOTE]
> Debido a la naturaleza probabilística de los LLM, Copilot puede usar un comando MCP distinto y aun así completar la tarea.

3. Envía a Copilot esta solicitud:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para permitir que se ejecute el comando que enumera las incidencias.
5. Revisa la lista generada.

Observa que Copilot también ha priorizado los elementos según cuáles cree que serán más útiles.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto a Copilot. Usarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funcionalidad al sitio.

1. En el Codespace, ve a `.github/instructions/ui.instructions.md`.
2. Fíjate en las pautas generales para añadir funcionalidad. Incluyen:
   - una descripción general de la arquitectura.
   - principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - enlaces a archivos de instrucciones específicos para Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones permiten hacer referencia tanto a otros archivos de instrucciones como a los archivos del proyecto. Las rutas son relativas al archivo de instrucciones, lo que permite reutilizarlas, dividir conjuntos grandes en archivos más manejables y señalar ejemplos y plantillas.

## Implementa el filtrado

Para completar el filtrado, tendrás que actualizar al menos tres partes distintas de la aplicación:

- añadir o ajustar la lógica restante de filtrado en la capa de datos (`src/lib/`).
- añadir o actualizar las pruebas del comportamiento de filtrado.
- actualizar la página de lista de juegos para incluir la interfaz de filtrado.

Además, las pruebas deben ejecutarse y superarse antes de combinar los cambios. El modo Agente de Copilot puede encargarse de estas tareas.

1. Puedes continuar la conversación actual con Copilot o seleccionar **New Chat** para empezar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** para añadir el archivo de instrucciones.
3. Comprueba que **Agent** sigue seleccionado en el menú desplegable de agentes.

   ![Captura de pantalla del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Comprueba que **Claude Sonnet 4.5** sigue seleccionado.
5. Pide a Copilot que implemente la funcionalidad según la incidencia relacionada de la lista:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos pertinentes, incluidos los auxiliares de datos, la interfaz y las pruebas. Después, modifica los archivos y ejecuta las pruebas.

   ![Captura de pantalla de Copilot explorando archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas: explorar el proyecto, modificar archivos y ejecutar pruebas. Puede tardar unos minutos según la complejidad de la tarea y la base de código. Mientras tanto, es posible que aparezcan los botones **Keep** y **Undo** en el editor. Cuando termine, tendrás la opción de conservar o deshacer todos los cambios; no es necesario seleccionarlos mientras Copilot trabaja.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de pantalla de un cuadro de diálogo en Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que haya pausas o que algunas pruebas fallen durante el proceso. No pasa nada: Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y no detectar errores.

   ![Captura de pantalla de una sesión completa con el modo Agente de Copilot](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado para detectar posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador con el sitio en ejecución y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio has aprendido a usar el modo Agente de GitHub Copilot para añadir funcionalidades a Tailspin Toys:

- el modo Agente puede implementar funcionalidades en la interfaz de usuario y la capa de datos.
- Copilot puede explorar el proyecto, identificar archivos y coordinar cambios.
- puedes revisar los cambios y las pruebas generados antes de combinarlos en la base de código.

A continuación, [probarás la funcionalidad con el servidor Playwright MCP][next-lesson] y abrirás una solicitud de cambios.

### Ejercicio de exploración adicional: implementar paginación

A medida que aumenta la lista de juegos, será necesario habilitar la paginación. Usa lo aprendido para pedir a Copilot que implemente la paginación. Ten en cuenta lo siguiente:

- sigue las prácticas recomendadas existentes, incluidos los archivos de instrucciones.
- decide cómo quieres implementar la paginación: por ejemplo, si el usuario podrá elegir el tamaño de página o si será fijo.
- incluye en la solicitud las pautas necesarias para obtener la implementación que necesitas.
- puede que tengas que iterar con Copilot, pedir cambios y proporcionar contexto. Es el flujo habitual de trabajo con Copilot.

## Recursos

- [Modos Ask, Edit y Agent de Copilot: qué hacen y cuándo usarlos][choose-mode]
- [Modo Agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
