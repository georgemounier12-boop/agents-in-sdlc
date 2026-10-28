---
title: "Lección 8 - Crea y combina la solicitud de cambios"
description: "Revisa en conjunto el filtrado, las instrucciones, la habilidad actualizada, el perfil de QA y las pruebas; después crea una solicitud de cambios y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de instrucciones, la habilidad actualizada, el perfil de garantía de calidad (QA) y las pruebas están en una sola rama. Es el momento de revisarlas en conjunto y abrir una solicitud de cambios. Combinaste manualmente la solicitud de valoración con estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las actualizaciones de instrucciones, la habilidad y el agente de QA en varias solicitudes de cambios. Para simplificar el taller, has mantenido todo el trabajo de filtrado y calidad en una conversación y una rama.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una solicitud de cambios.
- revisarás el cambio completo de funcionalidad y las pruebas de verificación.
- crearás la solicitud de cambios del filtrado.
- activarás Agent Merge solo después de revisar los cambios y confirmarás que se han combinado.

## Escenario

Durante el flujo de trabajo de filtrado, has usado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere ahora automatizar el trabajo restante de la solicitud, pero mantener bajo el control del desarrollador la autorización para combinarla.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de cambios. Al habilitarlo, Copilot resuelve los bloqueos: corrige las comprobaciones de integración continua (CI) que fallan, responde a comentarios de revisión y vuelve a aplicar los cambios sobre la rama base cuando es necesario. Después habilita la combinación automática de GitHub si el repositorio lo permite.

Hasta ahora seleccionabas **Merge pull request**. Agent Merge puede encargarse de esta parte, pero revisa primero el trabajo y asegúrate de que está listo antes de activarlo.

## Usa Agent Merge para administrar la solicitud

Ya has creado todo el código. Revisémoslo, creemos la solicitud y dejemos que Agent Merge se ocupe del resto.

1. Vuelve a tu Codespace.
2. Escribe `/agent` para abrir el cuadro de agentes.
3. Selecciona **Default** en la lista y pulsa <kbd>Enter</kbd>.
4. Crea una solicitud con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará la integración continua de la solicitud. Cuando se superen todas las comprobaciones, la combinará.
7. Confirma que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos que falten. Si se bloquea, revisa el motivo indicado y completa manualmente la combinación revisada cuando el repositorio lo permita.

## Resumen y siguientes pasos

Has automatizado varios aspectos del proceso de desarrollo: generar código, probarlo y validarlo, y ahora administrar la solicitud de cambios. En esta lección:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una solicitud.
- has revisado el cambio completo y las pruebas de verificación.
- has creado la solicitud de cambios del filtrado.
- has activado Agent Merge después de la revisión y confirmado que la solicitud se ha combinado.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para administrar el contexto y los modelos, compartir sesiones y delegar tareas opcionalmente en la nube.

## Recursos

- [Administrar solicitudes de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
