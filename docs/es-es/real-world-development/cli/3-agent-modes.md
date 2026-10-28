---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para implementar el filtrado desde una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Hemos empezado con una funcionalidad pequeña. Los cambios mayores requieren un proceso más sólido. GitHub Copilot CLI está diseñado para adaptarse al flujo de trabajo existente de una organización y ayudarnos a crear lo adecuado de la manera correcta. En esta y las siguientes lecciones seguirás un proceso habitual de desarrollo con agentes: partirás de una incidencia para crear una funcionalidad, comprobarás que el código es válido y que la funcionalidad se comporta como esperas, y finalmente combinarás los cambios en el proyecto.

> [!NOTE]
> Seguirás el flujo de trabajo de la funcionalidad en la misma conversación y rama. Normalmente se usarían ramas o solicitudes de cambios distintas para diferentes tipos de archivos, pero simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una conversación de Copilot a partir de una incidencia de GitHub.
- definirás los requisitos en el modo Plan.
- implementarás la funcionalidad con el modo Autopilot.
- revisarás el código.
- validarás la funcionalidad manualmente en el navegador que accede al puerto reenviado.

Mientras desarrollas esta funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la habilidad `quality-checks` existente, añadirás validación con MCP, crearás un agente de QA y abrirás la solicitud de cambios.

## Escenario

El catálogo de Tailspin Toys crece y las personas necesitan filtrar los juegos por categoría y editorial. La incidencia pendiente describe la funcionalidad, pero antes de programar hay que acordar algunos detalles, como la combinación de categorías. Usarás el modo Plan para resolver esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

La introducción de agentes de programación con IA no cambia los fundamentos del desarrollo; si acaso, los hace aún más importantes. El flujo de trabajo suele ser parecido a este:

1. Empezar con una incidencia que describa el trabajo necesario.
2. Crear un plan de implementación.
3. Desarrollar y revisar el código.
4. Ejecutar las pruebas para validar el código.
5. Validar manualmente la funcionalidad nueva.
6. Crear una solicitud de cambios (PR).
7. Combinar el código después de que se revise y de que se superen las comprobaciones de integración continua.

> [!NOTE]
> Los detalles exactos dependen del equipo y la organización, pero la mayoría de los procesos son variaciones de este flujo.

Al seguir este enfoque habitual, te aseguras de que el código generado por IA cumpla los requisitos y pase por el mismo proceso de validación que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** controla la autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre los modos:

- **Interactive**: tú y el agente trabajáis juntos. El agente propone cambios y espera tus indicaciones antes de continuar.
- **Plan**: el agente primero crea un plan y no puede editar los archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas e itera hasta completar la tarea.

Empieza en Plan, revisa el plan y, después, usa Autopilot para implementarlo.

## Empieza por la incidencia

Antes de empezar a trabajar en el filtrado, vuelve al Codespace y comprueba que el repositorio y el terminal estén listos.

1. Vuelve al Codespace. Si está detenido, reinícialo.
2. Confirma que se ha combinado la solicitud de cambios de valoración con estrellas.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las teclas de dirección para seleccionar la incidencia **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia a la solicitud y volver a la pestaña **Session**.

Observa que la solicitud empieza ahora con `#7` (o un número parecido). El carácter `#` permite incluir en el contexto una incidencia o solicitud de cambios de GitHub.

## Planifica la funcionalidad de filtrado

La planificación te permite acordar un enfoque antes de pedir a Copilot que implemente una funcionalidad o realice una tarea. Es recomendable dedicar tiempo a planificar las tareas complejas. Cambia al modo Plan y pide a Copilot que elabore el plan.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Confirma que el indicador debajo de la solicitud muestra **Plan**.
2. Después de la referencia a la incidencia que añadiste en el paso anterior, escribe:

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot explorará el proyecto y determinará el mejor enfoque.

3. Puede que Copilot te pregunte cómo debería funcionar el filtrado. Responde según tus preferencias; no hay respuestas incorrectas.
4. Cuando termine el plan, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandirlo.
5. Desplázate por el plan y revísalo.
6. Pide a Copilot que revise cualquier parte que no coincida con tus decisiones.

## Aprueba Autopilot

Ya has revisado el plan; ahora puedes implementarlo con Autopilot. Este modo permite a Copilot iterar en la tarea hasta que considere que está completa.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción equivalente de la versión instalada.
2. Confirma que el indicador debajo de la solicitud muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan, genera código y ejecuta pruebas.

> [!NOTE]
> Al aprobar el plan, la implementación puede empezar inmediatamente; revísalo antes. Si Copilot informa de dependencias ausentes o conflictos de puertos, resuelve el problema de configuración antes de dar por completas las comprobaciones.

## Revisa y comprueba la implementación

Como cualquier otro código, el código generado debe revisarse antes de combinarlo. Revisemos los cambios y ejecutemos el sitio para comprobar que todo funciona.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para pasar al modo Interactive. Confirma que el indicador ya no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` e inspecciona la implementación del filtrado y las pruebas.
3. Compara el resultado con la incidencia y las decisiones tomadas durante la planificación.
4. Cuando hayas revisado el código, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que resuelva cualquier error.

## Explora la nueva funcionalidad

El código parece correcto, pero ¿se ejecuta? Iniciemos la aplicación como antes y abramos el sitio en el navegador mediante el puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba los filtros por categoría y editorial, y las combinaciones acordadas en el plan.
4. Confirma que el reinicio de filtros y el comportamiento sin resultados coinciden con la incidencia y tus decisiones.
5. Vuelve al Codespace y pide a Copilot que detenga el servidor de desarrollo.

## Resumen y siguientes pasos

Has usado distintos modos de conversación para crear y revisar una funcionalidad. En esta lección:

- has iniciado una conversación de Copilot a partir de una incidencia de GitHub.
- has definido los requisitos en el modo Plan.
- has implementado la funcionalidad con Autopilot.
- has revisado el código.
- has validado la funcionalidad manualmente en el navegador que accede al puerto reenviado.

A continuación, veremos cómo se genera el código y nos aseguraremos de que siga las prácticas documentadas mediante [instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
