---
title: "Lección 5 - Personaliza y usa la habilidad quality-checks"
description: "Explora la habilidad quality-checks existente, personaliza el formato de su informe y úsala para verificar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Programar no consiste únicamente en escribir código. Ya hemos comprobado manualmente que el código funciona y hemos usado archivos de instrucciones para asegurarnos de que sigue nuestros estándares. Pero ¿qué ocurre con las pruebas, el lint y el resto de la integración continua (CI)?

Las **habilidades de agente** son adecuadas para estas tareas. Ayudan a Copilot a entender cómo realizar correctamente operaciones como estas.

En esta lección:

- explorarás la habilidad `quality-checks` existente.
- personalizarás el formato de los resultados.
- volverás a cargar la habilidad y la ejecutarás.

## Escenario

Tailspin Toys usa la habilidad `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que los resultados sean más fáciles de entender.

## Instrucciones, scripts y recursos

Las habilidades de agente agrupan instrucciones reutilizables para tareas, scripts ejecutables y recursos de apoyo que un agente carga cuando los necesita. En esencia, son una carpeta con el nombre de la habilidad y un archivo Markdown llamado `SKILL.md`. El archivo contiene un frontmatter con un nombre y una descripción, una explicación de su función y pautas sobre cuándo debe usarse. También puede contener subcarpetas con scripts y otros recursos.

> [!NOTE]
> Las carpetas y los archivos adicionales son opcionales. La habilidad `quality-checks` de Tailspin Toys solo contiene `SKILL.md`, ya que usa los comandos existentes del proyecto.

Las habilidades pueden estar en `.github/skills` para convertirse en recursos del repositorio que el equipo puede compartir y reutilizar, o en la carpeta de habilidades del usuario, `~/.copilot/skills`.

## Explora la habilidad

Exploremos la habilidad que creó el equipo de Tailspin Toys para ejecutar pruebas unitarias, lint y comprobaciones de tipos: `quality-checks`.

1. Vuelve a tu Codespace. En el editor de Codespaces, abre `.github/skills/quality-checks/SKILL.md`.
2. Lee el `name` y la `description` del principio. La descripción ayuda a Copilot a decidir cuándo usar la habilidad.
3. Lee las instrucciones y observa cómo guían a Copilot durante las pruebas y el lint.
4. Fíjate en que la habilidad aún no incluye una sección **Results output formatting** para dar formato a los resultados.

## Ejecuta la habilidad antes de cambiarla

Puedes ejecutar habilidades directamente con Copilot CLI o pedirlo en lenguaje natural. Pide a Copilot que ejecute las tres comprobaciones de la habilidad.

1. Vuelve a la conversación de filtrado en modo Interactive.
2. Envía esta solicitud:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Lee el informe final.

## Personaliza el informe

Queremos un informe mejor que indique qué se ejecutó, si se superó y qué resultados proporcionaron las herramientas. Actualicemos la habilidad para obtenerlo.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade esta sección al final del archivo:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. El archivo se guarda automáticamente.

## Ejecuta la habilidad actualizada

Veamos cómo funciona el cambio. Copilot CLI puede volver a cargar las habilidades editadas sin reiniciar la conversación.

1. Escribe:

   ```plaintext
   /skills reload
   ```

2. Envía exactamente la misma solicitud que antes:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Lee el informe final y compáralo con el primero.

## Resumen y siguientes pasos

Has personalizado y usado una habilidad de agente existente. En esta lección:

- has explorado la habilidad `quality-checks` para las pruebas unitarias, el lint y las comprobaciones de tipos.
- has personalizado el formato de los resultados.
- has vuelto a cargar y ejecutar la habilidad.

Este cambio se incluirá en la solicitud de la funcionalidad de filtrado. A continuación, permitirás que Copilot interactúe directamente con el sitio [mediante el servidor Playwright MCP][next-lesson].

## Más ejemplos de habilidades

Estos ejemplos de la comunidad sirven de referencia; no son tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de trabajo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y script de exportación incluido: `drawio`][drawio-example]
- [Pruebas de navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
