---
title: "Lección 10 - Resumen y siguientes pasos"
description: "Recapitula el flujo de trabajo de Copilot CLI, las dos solicitudes de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has usado GitHub Copilot CLI en un flujo de trabajo completo para Tailspin Toys. Has:

- preparado un Codespace, instalado Copilot CLI, explorado el proyecto y encontrado la incidencia de filtrado.
- añadido valoraciones con estrellas, revisado el resultado en el navegador que abre el puerto reenviado y combinado manualmente tu primera solicitud de cambios (PR).
- partido de la incidencia de filtrado, definido el enfoque en modo Plan, implementado la funcionalidad en modo Autopilot y revisado el resultado en modo Interactive.
- orientado al agente con instrucciones personalizadas, personalizado la habilidad `quality-checks` y usado para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- añadido el servidor Model Context Protocol (MCP) de Playwright y usado el navegador para explorar el filtrado.
- creado y seleccionado un agente personalizado de garantía de calidad (QA) para revisar los requisitos, la cobertura, los resultados de la habilidad y las evidencias obtenidas en el navegador.
- revisado el cambio completo del filtrado y autorizado Agent Merge para combinar la solicitud.
- explorado comandos de barra para consultar el contexto y los modelos, compartir sesiones y delegar tareas opcionalmente en la nube.

## Lo que has entregado

El taller incluye dos hitos de solicitudes de cambios, cada uno en su propia rama basada en `main` actualizada:

1. **Valoraciones con estrellas**: muestra en las tarjetas de juegos la valoración existente en `starRating` o un estado explícito para los juegos sin valorar.
2. **Filtrado y flujo de calidad**: implementa el filtrado, actualiza las instrucciones y aplícalas a la funcionalidad, personaliza el informe de `quality-checks`, crea un perfil de QA e incluye las pruebas correspondientes.

Desde la planificación del filtrado hasta la apertura de su solicitud de cambios, trabajaste en la misma conversación y rama. Para simplificar el taller, incluimos todo ese trabajo en una solicitud.

## Distintos tipos de verificación

Comprobaste el código de varias maneras: con pruebas automatizadas, inspeccionando el sitio en el navegador y pidiendo a Copilot que lo explorara con MCP. La habilidad `quality-checks` ejecutó las pruebas unitarias, lint y comprobaciones de tipos, y presentó los resultados con el nuevo formato. El agente de QA reunió estos resultados con una revisión de los requisitos y la cobertura antes de abrir la solicitud.

Las pruebas nuevas deben cubrir carencias reales; si el agente de QA determina que no hace falta añadir pruebas, también puede ser correcto. Revisa el código y las pruebas antes de autorizar la combinación, y actualiza las evidencias de verificación afectadas después de realizar cambios.

## Prácticas recomendadas

El contexto y las herramientas que proporcionas influyen en el trabajo de Copilot. En este taller has actualizado instrucciones, personalizado una habilidad, creado un perfil de QA y configurado un servidor MCP. Reutiliza estas personalizaciones en otras conversaciones y ajústalas a las necesidades del equipo. Las instrucciones definen estándares, las habilidades describen tareas repetibles, los agentes personalizados definen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados de las herramientas, no solo el resumen del agente.

Elige el **modo y el modelo** adecuados para la tarea. Usa **Plan** para pensar en un enfoque antes de implementarlo, **Interactive** para seguir de cerca los cambios específicos y **Autopilot** para tareas bien acotadas. Elige un modelo más rápido para ediciones rutinarias y uno más capaz para tareas complejas.

El contexto sigue siendo tan importante como la infraestructura. Describir claramente *qué* quieres crear, *por qué* y *cómo* influye mucho en el resultado.

## Más cosas que explorar

Ya has practicado el flujo de trabajo principal. Algunas otras funciones de CLI que puedes explorar:

- `/review` para pedir al agente de revisión de código que analice los cambios.
- `/rubber-duck` para explicar un problema y obtener otra perspectiva.
- `/fleet` para coordinar tareas independientes en paralelo.
- `/worktree` para aislar otra tarea.
- `/delegate` para enviar una tarea al agente en la nube de Copilot.

## Siguientes pasos

La mejor forma de mejorar con cualquier herramienta es seguir usándola: para código de producción, proyectos personales o esa pequeña aplicación que tienes en mente desde hace años. Comparte lo aprendido con el equipo y aprende de sus experiencias. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller del agente en la nube de Copilot][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Administrar solicitudes de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
