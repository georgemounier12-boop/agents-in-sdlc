---
title: "Lección 8 - Crea y combina la solicitud de cambios de la funcionalidad"
description: "Revisa conjuntamente el filtrado, las instrucciones, la actualización de la habilidad, el perfil de QA y las pruebas; después, crea una solicitud de cambios y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La implementación del filtrado, las actualizaciones de instrucciones y de la habilidad, el perfil de garantía de calidad (QA) y las pruebas están en una sola rama. Es el momento de revisarlas en conjunto y abrir una solicitud de cambios. Combinaste manualmente la solicitud de valoraciones con estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las actualizaciones de instrucciones y de la habilidad y el agente de QA en varias solicitudes de cambios. Para simplificar el taller, has mantenido todo el flujo de filtrado y calidad en una sesión y una rama.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación.
- revisarás la solicitud completa de la funcionalidad y las pruebas de verificación.
- autorizarás Agent Merge únicamente después de revisar los cambios y confirmarás que la solicitud se ha combinado.

## Escenario

Durante el flujo de trabajo de filtrado, has usado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere ahora automatizar el trabajo restante de la solicitud, pero mantener bajo el control del desarrollador la autorización para combinarla.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de cambios en la aplicación GitHub Copilot. Al habilitarlo, la sesión de la aplicación lee la solicitud y resuelve los bloqueos: corrige las comprobaciones de integración continua (CI) que fallan, responde a comentarios de revisión y vuelve a aplicar los cambios sobre la rama base cuando es necesario. La combina en cuanto GitHub lo permite. El proceso se ejecuta en segundo plano, continúa aunque reinicies la aplicación y se desactiva cuando se combina la solicitud.

Hasta ahora seleccionabas **Merge pull request** personalmente. Agent Merge puede encargarse de esa tarea, pero su capacidad para editar código y combinar cambios sigue requiriendo tu autorización explícita. Revisa las acciones que puede realizar y el trabajo antes de concederle permiso para combinar.

## Usa Agent Merge para administrar la solicitud

Ya has creado y revisado todo el código. Ahora puedes dejar que Agent Merge administre el proceso de la solicitud.

1. Usa el selector de agentes para seleccionar **Default agent**.
2. Selecciona el menú desplegable junto a **Create PR**.
3. Selecciona **Agent Merge**. El botón cambiará a **Agent Merge**.
4. Selecciona el botón **Agent Merge** para iniciar el proceso. Elegir la opción del menú desplegable no lo inicia.

Se iniciará el proceso de Agent Merge. El agente:

- creará la solicitud de cambios con un título y una descripción.
- incluirá en el cuerpo de la descripción una referencia a la incidencia relacionada si iniciaste la sesión desde una incidencia.
- volverá a aplicar los cambios sobre la rama de destino o resolverá posibles conflictos de combinación.
- supervisará CI para asegurarse de que se superen todas las comprobaciones.
- supervisará la solicitud para detectar comentarios de otros desarrolladores o de la revisión de código de Copilot, y hará cambios para resolverlos.
- podrá combinar automáticamente la solicitud cuando todo haya terminado correctamente.

Dejemos que Agent Merge combine también la solicitud cuando se hayan superado todas las comprobaciones.

5. Revisa las diferencias de la solicitud y las pruebas de verificación antes de conceder permiso para combinar.
6. Selecciona el menú desplegable junto a **Agent Merge**.
7. Comprueba que **Merge pull request** está marcado.
8. Pide explícitamente al agente que combine la solicitud:

   ```plaintext
   Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
   ```

9. Supervisa el proceso y resuelve cualquier bloqueo que se informe. Confirma que el estado de la solicitud es **Merged** antes de continuar; habilitar el permiso de combinación o tener un agente trabajando no confirma que la solicitud se haya combinado.

> [!IMPORTANT]
> Agent Merge no elude las protecciones del repositorio ni los permisos ausentes. Resuelve esos bloqueos antes de continuar.

## Resumen y siguientes pasos

Has automatizado varios aspectos del proceso de desarrollo: generar código, probarlo y validarlo, y ahora administrar la solicitud de cambios. En esta lección:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación.
- has revisado la solicitud completa de la funcionalidad y las pruebas de verificación.
- has autorizado Agent Merge solo después de la revisión y confirmado que la solicitud se ha combinado.

A continuación, [usarás un lienzo existente y crearás uno para clasificar tareas][next-lesson] y explorarás una forma más completa de examinar, planificar y visualizar el trabajo con el agente.

## Recursos

- [Administrar incidencias y solicitudes de cambios con la aplicación GitHub Copilot][managing-issues-prs]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
