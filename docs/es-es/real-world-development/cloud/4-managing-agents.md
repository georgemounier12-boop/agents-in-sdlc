---
title: "Ejercicio 4 - Supervisión y administración de agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores, pediste al agente en la nube de Copilot que se encargara de tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente en la nube funciona de manera autónoma y asíncrona, es importante poder supervisar estas tareas.

Hay varias herramientas para administrar las tareas asignadas al agente en la nube, incluida la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver todas las tareas de los agentes que tienen solicitudes de cambios abiertas, explorar las operaciones realizadas e incluso dirigir una sesión en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- dirigirás una sesión en curso para solicitar funcionalidad adicional.

## Escenario

Después de asignar al agente la creación de un modo de alto contraste, el equipo se dio cuenta de que también sería buen momento para añadir un modo claro. Como ya se estaba trabajando en actualizar el estilo del sitio y añadir controles para alternar modos, parecía lógico incluir esta funcionalidad. Dirigirás el trabajo del agente para pedirle que añada el modo claro además del alto contraste.

## Revisa las tareas del agente en la nube de Copilot

Veamos el estado de todas las tareas asignadas al agente en la nube de Copilot.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas del panel principal y del panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación de la base de código.
   - generar API para modificar juegos.
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso. Revisa las operaciones realizadas por Copilot, que pueden incluir:
   - obtener el código del repositorio.
   - crear el entorno de trabajo de Copilot.
   - configurar servidores MCP.
   - realizar varios pasos para completar la tarea asignada.

> [!NOTE]
> Los pasos concretos que aparezcan dependerán del estado del trabajo de Copilot y del enfoque que haya seguido.

4. Observa también el panel de solicitudes de cambios que aparece a la derecha. Desde allí puedes ver la solicitud y los archivos modificados para supervisar el trabajo.

## Dirige el agente en la nube

Ahora que has visto las tareas activas, pide a Copilot que añada el control de modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que use Copilot y del estado actual del trabajo.

   ![Sesión de accesibilidad en el panel de control](../../../_images/ex6-accessibility-session.png)

2. Espera unos minutos a que la sesión indique que terminó la configuración y empezó a trabajar. Lo sabrás cuando aparezcan mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe esta solicitud:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Captura de pantalla de la tarea del agente en la nube en la página de agentes, con el cuadro de diálogo para dirigir la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar la solicitud.
5. Observa cómo Copilot la confirma y la incorpora a su flujo de trabajo.

## Deja que Copilot trabaje

Al igual que antes, Copilot empezará a trabajar en la tarea actualizada. Incorporará la nueva solicitud a su flujo de trabajo cuando termine el paso que estaba realizando al recibir el mensaje.

Como antes, tardará varios minutos. Puedes aprovechar el tiempo para reflexionar sobre todo lo que has aprendido y explorado hasta ahora.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro principal para supervisar y dirigir las tareas del agente en la nube de GitHub Copilot. Desde este panel de control puedes seguir las tareas activas y completadas, revisar el trabajo realizado y redirigir tareas en curso para ajustar el alcance o añadir indicaciones.

Has aprendido a:

- explorar el panel de control y la página de agentes para supervisar las tareas del agente en la nube.
- redirigir una sesión en curso para solicitar funcionalidad adicional.

Mientras Copilot termina de trabajar en las mejoras de accesibilidad, puedes [iterar sobre las solicitudes de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iteración sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
