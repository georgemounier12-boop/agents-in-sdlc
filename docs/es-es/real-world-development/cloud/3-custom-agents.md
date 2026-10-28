---
title: "Ejercicio 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados para tareas o áreas concretas del flujo de desarrollo. Al definir agentes mediante archivos Markdown en la carpeta `.github/agents` del repositorio, puedes proporcionarles instrucciones específicas, prácticas recomendadas, patrones de programación y conocimientos del dominio para que realicen determinados tipos de trabajo con mayor eficacia. Los equipos pueden plasmar su experiencia en agentes reutilizables: uno de accesibilidad que aplique el cumplimiento de [WCAG][wcag], uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen mediante archivos Markdown en la carpeta `.github/agents` del proyecto o, de forma global, en `~/.copilot/agents`. Cada archivo tiene un frontmatter YAML que contiene, como mínimo, `name` y `description`, seguido de una instrucción Markdown que define el comportamiento, la experiencia y las pautas del agente.

### Agentes personalizados y habilidades de agente

Existe cierto solapamiento entre los agentes personalizados y las [habilidades de agente][agent-skills-concept]. Ambos se suelen definir con archivos Markdown e indican a una IA cómo realizar operaciones. La forma más sencilla de distinguirlos es esta: un **agente personalizado** es el trabajador y las **habilidades** son las herramientas.

Los agentes personalizados tienen su propia ventana de contexto y están diseñados para coordinar habilidades e incluso otros agentes como parte de su trabajo. En este laboratorio, el agente personalizado de accesibilidad revisará y actualizará el sitio conforme a las pautas de accesibilidad; para hacerlo, podría recurrir a habilidades como un flujo de trabajo de solicitudes de cambios o una que ejecute y administre pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Como ocurre con cualquier herramienta de IA, prueba distintas opciones e itera para encontrar las que se adapten a tu entorno y tus casos de uso.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Con los agentes personalizados:

- aprenderás cómo se definen.
- asignarás una tarea a un agente personalizado.

## Escenario

Tailspin Toys se compromete a que todas las personas puedan usar su plataforma de financiación colectiva, independientemente de sus capacidades o preferencias visuales. Los comentarios recientes indican que algunas personas tienen dificultades para leer el tema oscuro actual debido al contraste insuficiente entre el texto y el fondo. Para abordar este problema de accesibilidad, el equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

Como la accesibilidad es fundamental, quieres implementar esta mejora lo antes posible. Para generar la funcionalidad, usarás un agente personalizado.

## Revisa el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado para accesibilidad. Revisemos su contenido para entender cómo orientará a Copilot.

Vuelve a tu Codespace y revisa el archivo del agente personalizado de accesibilidad:

1. Abre `.github/agents/accessibility.md`.
2. Observa el frontmatter YAML con los campos `name` y `description`.

> [!CAUTION]
> El frontmatter con `name` y `description` es obligatorio para los agentes personalizados.

3. Lee las secciones siguientes, que describen:
   - las responsabilidades principales al generar código para un sitio accesible.
   - las prácticas recomendadas de accesibilidad.
   - ejemplos de código HTML, CSS y JavaScript.
   - errores y problemas habituales.

## Crea y asigna una incidencia

El panel de agentes es el centro de control para trabajar con todos los agentes de tu entorno. Desde allí puedes asignar tareas al agente en la nube de Copilot, supervisarlas y redirigirlas o darles indicaciones adicionales. Empecemos por asignar a Copilot la tarea de crear el modo de alto contraste.

1. Ve a tu repositorio.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de una incidencia nueva.
4. Selecciona **Blank issue** para crearla.
5. Escribe `Add high contrast mode to website` en **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.
9. Selecciona **Accessibility agent** en la lista de agentes personalizados.

    ![Captura de pantalla de la asignación del agente en la nube, con los campos de agente personalizado y accesibilidad resaltados](../../../_images/ex5-select-custom-agent.png)

10. Selecciona **Assign**.
11. Copilot empezará a trabajar en la tarea en segundo plano.

## Resumen y siguientes pasos

En esta lección has explorado los [agentes personalizados][custom-agents] de GitHub Copilot, asistentes especializados en tareas y áreas concretas. Con ellos puedes convertir la experiencia y las normas de tu equipo en agentes reutilizables que orienten a Copilot.

Has aprendido:

- cómo se definen los agentes personalizados.
- cómo se asigna una tarea a un agente personalizado.

Mientras Copilot trabaja en el modo de alto contraste, puedes [supervisar y dirigir la sesión del agente][next-lesson] desde el panel de control.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Prepararse para usar agentes personalizados en tu organización][org-custom-agents]
- [Prepararse para usar agentes personalizados en tu empresa][enterprise-custom-agents]

---

| [← Lección anterior: Agente en la nube de GitHub Copilot][previous-lesson] | [Siguiente lección: Administración de agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
