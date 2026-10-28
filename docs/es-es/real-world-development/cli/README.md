---
slug: es-es/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Crea, verifica y entrega dos cambios para Tailspin Toys mientras aprendes los modos de Copilot CLI, las personalizaciones, las herramientas MCP y la automatización de solicitudes de cambios."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** incorpora GitHub Copilot a tu terminal como asistente de programación con agentes. Explora bases de código, genera código, ejecuta comandos y se conecta a herramientas externas, todo desde la línea de comandos, para que puedas mantener el flujo sin cambiar a un editor gráfico.

El taller sigue un flujo de trabajo continuo de Tailspin Toys:

1. Prepara el proyecto en GitHub Codespaces, instala Copilot CLI y familiarízate con la herramienta.
2. Realiza un cambio específico para mostrar valoraciones con estrellas, revísalo en el navegador y combina manualmente tu primera solicitud de cambios (PR).
3. Parte de la incidencia de filtrado, define el enfoque en modo Plan, impleméntalo en modo Autopilot y revísalo en modo Interactive.
4. Actualiza las instrucciones del repositorio y aplícalas al trabajo de filtrado.
5. Personaliza la habilidad `quality-checks` existente y úsala para ejecutar las comprobaciones del proyecto.
6. Añade el servidor Model Context Protocol (MCP) de Playwright y úsalo para explorar el filtrado en un navegador.
7. Crea un agente personalizado de garantía de calidad (QA) y úsalo para revisar los requisitos, la cobertura y las pruebas de verificación.
8. Revisa el cambio completo de filtrado y usa Agent Merge para combinar su solicitud de cambios.
9. Explora comandos de barra útiles para administrar el contexto y los modelos, compartir sesiones y delegar tareas opcionalmente en la nube.

Para mantener el taller centrado, crearás dos solicitudes de cambios: una para las valoraciones con estrellas y otra para el filtrado con las actualizaciones de instrucciones y de la habilidad, el perfil de QA y las pruebas. El trabajo de filtrado y calidad comparte una conversación y una rama para que puedas aprovechar lo realizado mientras exploras cada herramienta.

## Lecciones

| Lección | Tema | Descripción |
| ------ | ----- | ----------- |
| [0. Requisitos previos][ex0] | Configuración | Crea el repositorio y un Codespace |
| [1. Instala Copilot CLI][ex1] | Instalación | Instala y autentica Copilot CLI y familiarízate con la herramienta |
| [2. Añade valoraciones con estrellas: una mejora rápida][ex2] | Primer cambio | Muestra las valoraciones existentes y el valor alternativo cuando es `null`; después combina tu primera solicitud de cambios |
| [3. Modos de agente: Plan y Autopilot][ex3] | Modos de agente | Planifica la funcionalidad desde su incidencia, impleméntala con Autopilot y revísala en modo Interactive |
| [4. Orienta Copilot con instrucciones personalizadas][ex4] | Contexto | Explora y actualiza las instrucciones y aplícalas al filtrado |
| [5. Personaliza y usa la habilidad quality-checks][ex5] | Comprobaciones repetibles | Explora la habilidad existente, cambia el formato del informe y ejecútala |
| [6. Valida la funcionalidad con Playwright MCP][ex6] | Observación en el navegador | Configura MCP en CLI y examina el comportamiento del filtrado |
| [7. Crea y usa un agente de QA][ex7] | Requisitos y cobertura | Crea y selecciona un perfil especializado y reúne las pruebas de verificación finales |
| [8. Crea y combina la solicitud de la funcionalidad][ex8] | Revisión y combinación | Revisa el cambio completo, crea la solicitud y usa Agent Merge |
| [9. Comandos de barra de GitHub Copilot CLI][ex9] | Funciones de CLI | Explora el contexto, los modelos, el uso compartido y la delegación opcional al agente en la nube |
| [10. Resumen y siguientes pasos][ex10] | Resumen | Repasa el flujo de trabajo, las personalizaciones reutilizables y otros recursos |
| [Opcional: Incorpora Foundry][foundry] | Agentes alojados | Prepara un modelo, implementa un agente basado en el catálogo y conéctalo al sitio web |

## Requisitos previos

Antes de asistir a este taller, asegúrate de tener:

- [ ] Una cuenta de GitHub con un plan activo **Copilot Student, Pro, Pro+, Business o Enterprise**
- [ ] Permiso para crear un repositorio y un Codespace
- [ ] Conocimientos básicos de las operaciones del terminal o la línea de comandos

> [!TIP]
> ¿No tienes un plan de pago? El alumnado verificado puede obtener GitHub Copilot gratis a través de [GitHub Education][student-plan]. El plan **Copilot Student** incluye el agente, MCP, la revisión de código y Copilot CLI que se usan en este taller.

> [!NOTE]
> Si usas Copilot Business o Copilot Enterprise, comprueba que el administrador haya habilitado Copilot CLI.

## Empezar

**[Empieza con los requisitos previos →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
