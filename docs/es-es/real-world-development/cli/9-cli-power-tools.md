---
title: "Lección 9 - Comandos de barra de GitHub Copilot CLI"
description: "Explora comandos de barra para administrar el contexto, seleccionar modelos, compartir sesiones y delegar tareas opcionalmente en la nube."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como cualquier buena herramienta de CLI, GitHub Copilot CLI incluye muchos comandos de barra. Permiten acceder a funciones avanzadas, información interna y opciones de configuración adicionales. Ya has usado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Veamos algunos otros que pueden ser útiles.

En esta lección:

- usarás `/context` y `/compact` para explorar cómo administra Copilot el contexto de la conversación.
- usarás `/model` para explorar los modelos disponibles.
- aprenderás a exportar o compartir una sesión con `/share`.
- explorarás comandos opcionales para trabajar en paralelo, crear árboles de trabajo y delegar tareas al agente en la nube.

## Escenario

Has completado el flujo de trabajo principal de CLI. Ahora veremos otras funciones: administrar el contexto, cambiar de modelo, compartir sesiones y, opcionalmente, delegar tareas en el [agente en la nube de Copilot][about-cloud-agent].

## Explora el contexto de Copilot CLI

En tareas grandes o complejas puedes llegar al límite de la ventana de contexto del modelo. Copilot CLI compacta automáticamente la conversación cuando hace falta. También puedes inspeccionar o compactar el contexto mediante comandos de barra.

1. Vuelve a tu Codespace e inicia Copilot CLI desde la raíz del repositorio si aún no está abierto.
2. Escribe:

   ```plaintext
   /context
   ```

3. Observa el modelo, el uso actual de tokens y la distribución del contexto entre instrucciones del sistema, herramientas, mensajes y espacio libre.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Vuelve a escribir `/context` y compara los resultados. Si la conversación ya es pequeña, quizá no notes una diferencia importante.

> [!NOTE]
> Copilot CLI compacta el contexto automáticamente a medida que se llena la ventana. Usa `/compact` cuando quieras elegir cuándo hacerlo. Usa `/clear` o `/new` para empezar una conversación distinta con contexto nuevo.

## Elige el modelo

Los modelos tienen distintas fortalezas y cada desarrollador puede tener sus preferencias. Copilot CLI permite mostrar los modelos disponibles y seleccionar uno.

1. Escribe:

   ```plaintext
   /model
   ```

2. Explora los modelos y la información de uso disponible.
3. Mantén el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Comparte una sesión

Compartir el trabajo y los conocimientos ayuda a todo el equipo a mejorar el uso de herramientas de IA. El comando `/share` puede exportar una sesión a un archivo Markdown o HTML, crear un enlace para compartir o publicar un gist de GitHub.

1. Escribe `/help` y revisa las opciones de `/share` en la versión instalada.
2. Si quieres compartir la sesión, elige el destino adecuado. Por ejemplo, `/share file` crea una exportación Markdown local.
3. Revisa el contenido exportado antes de enviarlo o publicarlo. Las exportaciones pueden incluir solicitudes, respuestas y detalles del proyecto.

Publicar un enlace o un gist es opcional. No publiques contenido del repositorio ni de la conversación que el equipo no tenga previsto compartir.

## Opcional: amplía el trabajo o delégalo

El flujo de trabajo principal del taller ha terminado. Copilot CLI también ofrece comandos para tareas mayores:

- `/fleet` puede dividir tareas independientes entre subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un árbol de trabajo Git aislado para otra tarea.
- `/delegate` puede enviar una tarea al agente en la nube de Copilot, que trabaja de forma asíncrona y puede abrir una solicitud de cambios.

Estos comandos son opcionales, ya que pueden crear árboles de trabajo adicionales o trabajo remoto. Empieza con una tarea nueva y bien acotada, y revisa los resultados mediante el flujo de trabajo habitual. Si quieres aprender más sobre el trabajo asíncrono con agentes, continúa con el [taller del agente en la nube][cloud-workshop].

## Resumen y siguientes pasos

Los comandos de barra de Copilot CLI te permiten configurarlo, compartir sesiones y consultar qué ocurre en segundo plano. En esta lección:

- has usado `/context` y `/compact` para explorar cómo administra Copilot el contexto de la conversación.
- has usado `/model` para explorar los modelos disponibles.
- has aprendido cómo exportar o compartir una sesión con `/share`.
- has explorado comandos opcionales para trabajar en paralelo, crear árboles de trabajo y delegar tareas en el agente en la nube.

Copilot CLI incluye muchos más comandos de barra. Terminemos el recorrido [repasando lo aprendido][next-lesson] y los siguientes pasos.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Administración del contexto en Copilot CLI][context-management]
- [Acerca del agente en la nube de Copilot][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
