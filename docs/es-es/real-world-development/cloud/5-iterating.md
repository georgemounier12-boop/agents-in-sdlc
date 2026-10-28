---
title: "Ejercicio 5: Iteración sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Administración de agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este laboratorio has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le pediste que añadiera documentación al código, creara una funcionalidad de juegos relacionados para que el equipo de diseño pudiera iterar y añadiera mejoras de accesibilidad, incluidos controles para alternar los modos de alto contraste y claro. Revisemos los cambios de código y, si es necesario, demos comentarios a Copilot para mejorarlos.

### Escenario

Como ya hemos mencionado varias veces, los fundamentos del diseño de software y DevOps no cambian con la IA generativa. Siempre queremos revisar el código generado y seguir nuestro proceso habitual de DevOps. Con esto en mente, revisemos las sugerencias de GitHub Copilot para añadir documentación, la funcionalidad de juegos relacionados y las mejoras de accesibilidad antes de habilitar la revisión para el resto del equipo.

## Seguridad y agente en la nube de GitHub Copilot

Como el agente en la nube de Copilot realiza las tareas de forma asíncrona y sin supervisión, se han establecido ciertas restricciones de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que usa para sus cambios.
- El agente en la nube se ejecuta en GitHub Actions, donde crea un entorno independiente y efímero para trabajar.
- Una persona debe aprobar cualquier flujo de trabajo de GitHub Actions antes de que pueda ejecutarse.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empecemos por la primera solicitud de cambios que generó el agente en la nube de GitHub Copilot: la que añade documentación al código. Revisarás el cambio mediante la interfaz estándar de solicitudes de cambios de GitHub.com.

> [!NOTE]
> Al explorar la solicitud, puede que veas una advertencia que indica que el cortafuegos bloqueó a GitHub Copilot. Es lo esperado, ya que el acceso del agente en la nube a recursos externos, incluidas las llamadas a servidores MCP externos, está limitado de forma predeterminada. Si quieres, puedes [personalizar o deshabilitar el cortafuegos del agente en la nube de Copilot][agent-firewall].

1. Vuelve a tu repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de solicitudes de cambios.
3. Abre la solicitud titulada, por ejemplo, **Add missing documentation** u otro título similar.

> [!NOTE]
> Si Copilot sigue trabajando en la tarea, la solicitud tendrá la marca **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos; puedes descansar o repasar lo que has aprendido.

4. Cuando la solicitud esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

   ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluye los nuevos comentarios TSDoc y otra documentación. Los cambios concretos pueden variar.

   Mientras revisas los cambios, busca comentarios TSDoc, convenciones de TypeScript y comentarios de cabecera. Proceden de los archivos de instrucciones personalizadas que revisaste al principio del ejercicio 2.

6. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que avisa de que algunos flujos de trabajo esperan aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro deberían superarse. Puede tardar unos minutos.

## Pide cambios a GitHub Copilot

Trabajar con Copilot en una solicitud de cambios no es una interacción unidireccional. Puedes mencionarlo en comentarios de la solicitud o en comentarios insertados en el código, igual que harías con otros miembros del equipo. Copilot verá esos comentarios e iniciará otra sesión para resolverlos. Como los resultados no son deterministas, no podemos darte un texto exacto para la solicitud. Algunas ideas:

- Añadir al principio de cada archivo de código un comentario breve que describa su contenido.
- Añadir comentarios TSDoc a archivos TypeScript y Astro.
- Crear un README que describa la estructura de la aplicación Astro.

1. Añade un comentario solicitando un cambio en la documentación generada y menciona a **@copilot**, como harías con cualquier usuario. Puedes usar una de las ideas anteriores u otra sugerencia de documentación.
2. Selecciona **View Session** para ver el trabajo de Copilot. Observa que inicia una sesión nueva para realizar los cambios.
3. Puedes seleccionar **Back to pull request** para volver a la solicitud.

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine los cambios, aparecerá una confirmación nueva en la solicitud.
5. Selecciona la pestaña **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Cuando lo estés, puedes marcar la solicitud como lista para revisión y combinarla en la rama principal.

![Marca la solicitud como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la funcionalidad de juegos relacionados

Volvamos a la solicitud que generó Copilot para resolver la incidencia sobre los juegos relacionados en la página de detalles.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la solicitud titulada, por ejemplo, **Show related games on the game details page** u otro título similar.
4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Deberías ver un indicador que avisa de que algunos flujos de trabajo esperan aprobación.
7. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro deberían superarse. Puede tardar unos minutos.
9. **Opcional:** también puedes cambiar a esta rama en tu Codespace para probar manualmente la funcionalidad de juegos relacionados. Ve al Codespace, abre un terminal y ejecuta estos comandos (sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**):

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot ha creado la funcionalidad de juegos relacionados. Como antes, puedes trabajar de forma iterativa con el agente en la nube para solicitar cambios. Por ejemplo, podrías pedirle que modifique cuántos juegos relacionados se muestran o que añada comentarios de cabecera y comentarios TSDoc. Recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas. Para solicitar cambios, añade otro comentario en la pestaña **Conversation**; Copilot lo verá e iniciará una nueva sesión.

## Revisa las mejoras de accesibilidad

Por último, revisemos las mejoras de accesibilidad implementadas con el agente personalizado de accesibilidad. Esta solicitud debería incluir el modo de alto contraste asignado en el ejercicio 3 y el modo claro solicitado desde el panel de control en el ejercicio 4.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la solicitud titulada, por ejemplo, **Add high contrast mode to website** u otro título similar.

> [!NOTE]
> Si Copilot sigue trabajando en la tarea, la solicitud tendrá la marca **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos.

4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Revisa la implementación y presta especial atención a:
   - los componentes de la interfaz para alternar entre modos.
   - el uso del almacenamiento local para conservar las preferencias.
   - los cambios de CSS o estilo para los modos de alto contraste y claro.
   - los atributos de accesibilidad, como las etiquetas ARIA y la navegación con teclado.
   - el código JavaScript o TypeScript que controla el cambio de modo.

6. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que avisa de que algunos flujos de trabajo esperan aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro deberían superarse. Puede tardar unos minutos.
10. **Opcional:** puedes cambiar a esta rama en tu Codespace para probar manualmente las mejoras de accesibilidad. Ve al Codespace, abre un terminal y ejecuta los siguientes comandos (sustituye `<branch-name>` por el nombre de la rama creada por Copilot):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba los controles de los modos de alto contraste y claro en el navegador. Comprueba que funcionan y que los ajustes se conservan al volver a cargar las páginas.

Observa cómo el agente personalizado de accesibilidad ayudó a orientar a Copilot para que implementara las funcionalidades conforme a las prácticas recomendadas de accesibilidad. Si detectas problemas o mejoras posibles, puedes mencionar a **@copilot** en un comentario y solicitar cambios, como hiciste con las solicitudes anteriores.

## Ejercicio opcional: sigue delegando

El agente en la nube funciona mejor cuando puedes asignarle tareas reales de la lista de pendientes y dedicarte a otras cosas. Para adquirir esa costumbre, crea algunas incidencias más en el repositorio y asígnaselas a Copilot. Algunas ideas:

- Crear un formulario de interés para patrocinadores en la página de detalles del juego.
- Implementar la paginación en la página de la lista de juegos.
- Añadir validación de entradas y gestión de errores a las funciones de acceso a datos.

## Resumen

Has completado el recorrido del agente en la nube. En estas lecciones:

- **Has inspeccionado los archivos de instrucciones personalizados del repositorio** para reconocer su efecto en el trabajo del agente en la nube.
- **Has asignado incidencias al agente en la nube de Copilot** y has visto cómo prepara el entorno, planifica y trabaja de forma asíncrona.
- **Has creado y usado un agente personalizado** para accesibilidad y has añadido controles para los modos de alto contraste y claro.
- **Has usado la página de agentes de Copilot como centro de control** para supervisar y dirigir la sesión de accesibilidad.
- **Has revisado las solicitudes de cambios del agente en la nube e iterado sobre ellas**, mencionando a `@copilot` para solicitar cambios y aprobando flujos de trabajo.

## Revisión y siguientes pasos

Has completado el recorrido del agente en la nube. Si quieres seguir aprendiendo, los otros recorridos complementan lo que has practicado:

- 🖥️ **[Recorrido de VS Code](../../vscode/)**: explora el modo Agente de Copilot Chat y la integración con MCP directamente desde el IDE.
- 💻 **[Recorrido de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades de agente, un agente de QA personalizado, Playwright MCP, Agent Merge y un puente opcional `/delegate` hacia el agente en la nube.

En tu propio repositorio, prueba estas ideas:

- Asigna al agente en la nube una incidencia de refactorización o cobertura de pruebas.
- Crea un agente personalizado para otro ámbito.
- Configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el cortafuegos del agente en la nube de Copilot][agent-firewall]

---

| [← Lección anterior: Administración de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
