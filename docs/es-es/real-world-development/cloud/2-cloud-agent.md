---
title: "Ejercicio 2 - Agente en la nube de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que muy pocas organizaciones, si es que hay alguna, no tengan problemas con la deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funcionalidad que llevan mucho tiempo pendientes por falta de recursos. El agente en la nube de GitHub Copilot puede actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, genera un borrador de solicitud de cambios listo para que lo revise una persona. Así puedes delegar tareas tediosas, acelerar el desarrollo y dedicarte a cuestiones de mayor alcance.

Con el agente en la nube de Copilot explorarás:

- cómo personalizar el entorno de generación de código.
- cómo asegurarte de que las operaciones se realicen de forma segura.
- por qué es importante definir incidencias con un alcance claro.
- cómo asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica que quiere resolver. Los contratistas que crearon la primera versión del sitio dejaron la documentación en mal estado: comprobarás que falta por completo. Como primer paso, el equipo quiere añadir comentarios TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar la búsqueda de juegos. Quiere que la página de detalles de cada juego muestre otros títulos de la misma categoría, para que las personas puedan seguir explorando. Por ahora no necesita una implementación refinada; basta con una versión que permita probar y aceptar la experiencia de usuario. Aunque esta tarea bloquea el trabajo, hay otras incidencias que tienen más prioridad en este momento.

Son ejemplos de tareas que pueden quedar relegadas y que conviene asignar al agente en la nube de Copilot. El agente puede trabajar en ellas de forma asíncrona mientras tú te ocupas de otras tareas y, después, puedes revisar su trabajo y confirmar que cumple tus expectativas.

## Introducción al agente en la nube de GitHub Copilot

[El agente en la nube de GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma parecida a un desarrollador. Puedes trabajar de varias maneras, por ejemplo, [asignándole una incidencia de GitHub](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Cuando se le asigna una incidencia, Copilot crea un borrador de solicitud de cambios para registrar el progreso, configura un entorno y empieza a trabajar. Puedes explorar la sesión mientras sigue activa o después de que termine. Cuando la solución propuesta esté lista para revisión, te mencionará en la solicitud de cambios.

## La importancia de definir bien el alcance

GitHub Copilot no hace magia: no puedes saltarte el trabajo de análisis. Incluso las tareas aparentemente sencillas pueden ser complejas, así que [define cuidadosamente el alcance de las tareas que asignas al agente en la nube de Copilot][cloud-agent-best-practices]. Trátalo como a un compañero de programación basado en IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian al añadir IA generativa.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has conocido las instrucciones personalizadas y cómo orientan a Copilot. Si ya completaste el ejercicio práctico sobre instrucciones personalizadas, habrás visto `.github/copilot-instructions.md` en acción. Si no, este es un buen momento para echarle un vistazo. Antes de asignar trabajo al agente en la nube, lee los archivos de instrucciones que ya incluye el repositorio para reconocer su influencia más adelante.

Abre estos archivos en la interfaz web de GitHub de tu repositorio o en un Codespace si ya tienes uno abierto:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, especialmente las expectativas para las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en el frontmatter `applyTo`, que aplica estas instrucciones a los archivos `**/*.test.ts`.

Cuando en la siguiente sección asignes al agente en la nube la incidencia `Code lacks documentation`, fíjate en los comentarios TSDoc, las convenciones de TypeScript y los comentarios de cabecera de la solicitud resultante. Proceden de estos archivos de instrucciones. Volveremos a mencionarlo al revisar las solicitudes en un ejercicio posterior.

## Configura el entorno de desarrollo del agente en la nube de Copilot

Normalmente, crear código requiere un entorno concreto y ejecutar scripts de configuración para garantizar que todo esté listo. Lo mismo ocurre al asignar tareas a Copilot, que trabaja de forma parecida a un ingeniero de software.

El agente en la nube usa [GitHub Actions][github-actions] como entorno de trabajo. Puedes personalizarlo mediante un [flujo de trabajo de configuración especial][setup-workflow], definido en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que empiece a trabajar. Así, puede acceder a las herramientas y dependencias de desarrollo necesarias. El flujo ya está configurado para este laboratorio. Instala Node.js y las dependencias del proyecto, los navegadores para las pruebas de extremo a extremo y la base de datos SQLite migrada y preparada para la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Establece los permisos mínimos necesarios para *tus pasos*. Copilot tendrá su propio token para sus operaciones.
    permissions:
      # Si quieres clonar el repositorio como parte de los pasos de configuración, por ejemplo, para instalar dependencias, necesitarás el permiso `contents: read`. Si no clonas el repositorio en esos pasos, Copilot lo hará automáticamente cuando terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de frontend y aplicación - Node.js (toda la aplicación usa ahora Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y carga datos iniciales en SQLite local para que las compilaciones y pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero hay un par de puntos importantes:

- Contiene un único trabajo llamado `copilot-setup-steps`. Se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la solicitud de cambios.
- El desencadenador `workflow_dispatch` permite ejecutar manualmente el flujo desde la pestaña **Actions** del repositorio. Esto resulta útil para comprobar que funciona correctamente sin esperar a que Copilot lo ejecute.

## Añade documentación

Aunque todo el mundo reconoce la importancia de la documentación, muchos proyectos tienen información desactualizada o no tienen documentación. Esta deuda técnica suele quedar sin resolver, reduce la productividad y dificulta el mantenimiento de la base de código o la incorporación de nuevos desarrolladores. Copilot destaca en la generación de documentación, así que esta es una buena tarea para asignarle al agente en la nube. Trabajará en segundo plano para crear la documentación necesaria. En un ejercicio posterior volverás para revisar el resultado.

1. En una nueva pestaña del navegador, ve a tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de una incidencia nueva.
4. Selecciona **Blank issue** para crearla.
5. Escribe `Code lacks documentation` en **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

   ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests**.
11. Abre la solicitud de cambios (PR) recién generada, cuyo título será parecido a `[WIP]: Code lacks documentation`. Si aún no aparece en la lista, espera uno o dos minutos y vuelve a cargar la página.
12. Al cabo de unos minutos, Copilot debería crear una lista de tareas.

> [!NOTE]
> Es posible que la lista de tareas de Copilot tarde varios minutos en aparecer en la solicitud. Copilot está preparando su entorno (ejecutando el flujo de trabajo anterior), analizando el proyecto y determinando cómo abordar la tarea.

13. Revisa la lista y las tareas que realizará.
14. Desplázate por la cronología de la solicitud de cambios. Deberías ver una actualización que indica que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona **View session**.

   ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que volver a cargar la página para ver el indicador actualizado.

16. Observa que puedes recorrer la sesión en directo y ver cómo Copilot resuelve la tarea: explora el código y su estado, se detiene a analizar y decidir el enfoque, y crea código.

El proceso puede tardar varios minutos. Una de las principales ventajas del agente en la nube es poder realizar tareas de forma asíncrona para que podamos dedicarnos a otras cosas. Aprovecharemos esa característica asignándole otra tarea y, mientras tanto, nos ocuparemos de escribir código para añadir funcionalidades a la aplicación.

## Añade una sección de juegos relacionados a la página de detalles

Una gran ventaja del agente en la nube de GitHub Copilot es que permite dividir el trabajo: tú puedes centrarte en un conjunto de tareas mientras él se ocupa de otro. Aunque añadir una sección de juegos relacionados para el equipo de diseño no debería llevar demasiado tiempo, podrías aprovechar ese tiempo para otras tareas. Asignemos esta tarea al agente en la nube de Copilot.

1. Vuelve a tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de una incidencia nueva.
4. Selecciona **Blank issue** para usar la plantilla en blanco.
5. Escribe `Show related games on the game details page` en **Title**.
6. Escribe lo siguiente en **Description**:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

   ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Al poco tiempo, deberías ver varios emoji de ojos 👀 en el primer comentario de la incidencia. Indican que Copilot ya está trabajando.

![Copilot usa el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en tu segunda solicitud. El agente en la nube funciona de manera parecida a un ingeniero de software: no necesitas supervisarlo activamente, sino revisar el resultado cuando termine. Ahora puedes centrarte en crear y usar agentes personalizados.

## Resumen y siguientes pasos

En esta lección has explorado el [agente en la nube de GitHub Copilot][copilot-agents]. Puedes asignarle incidencias para que trabaje de forma asíncrona. Úsalo para resolver deuda técnica, crear funcionalidades o ayudar a migrar código de un framework a otro.

Has explorado estos conceptos:

- personalizar el entorno para generar código.
- asegurarte de que las operaciones se realicen de forma segura.
- definir incidencias con un alcance claro.
- asignar incidencias a Copilot.

Mientras el agente en la nube trabaja, puedes centrarte en crear y usar agentes personalizados. [El agente en la nube de Copilot también admite servidores MCP][cloud-agent-mcp] e instrucciones personalizadas, que ya exploramos en módulos anteriores.

## Recursos

- [Acerca del agente en la nube de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
