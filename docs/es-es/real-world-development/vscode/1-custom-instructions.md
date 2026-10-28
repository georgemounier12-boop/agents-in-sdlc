---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo Agente →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una manera determinada, o Copilot necesita conocer información de contexto, debes asegurarte de que pueda acceder a ella. Hay varias formas de proporcionar contexto específico a Copilot. Una de las principales son los [archivos de instrucciones][instruction-files], que permiten orientar la generación de código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto específico del proyecto, las pautas de programación y los estándares de documentación mediante instrucciones personalizadas del repositorio y archivos de instrucciones aplicables a rutas concretas.
- generarás la primera parte de los datos para el filtrado (una función auxiliar para editoriales) con las instrucciones actuales.
- añadirás una nueva norma para todo el repositorio a `.github/copilot-instructions.md`.
- volverás a ejecutar la misma solicitud y observarás cómo el código generado sigue la nueva norma.
- confirmarás el cambio de instrucciones y la parte de filtrado en `main` para que Copilot use las instrucciones actualizadas en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no seguir algunas de las normas que establezcas. Copilot no es determinista. El objetivo del ejercicio es observar la *tendencia* de los cambios de comportamiento después de actualizar las instrucciones, no obtener siempre un resultado idéntico, carácter por carácter.

## Archivos de instrucciones

### Escenario

Como cualquier empresa de desarrollo competente, Tailspin Toys tiene pautas y requisitos para sus prácticas de desarrollo:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz de usuario debe usar un tema oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe haber un bloque de comentarios que describa su contenido.

Mediante archivos de instrucciones, te asegurarás de que Copilot tenga la información necesaria para realizar las tareas conforme a estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas permiten proporcionar a Copilot contexto y preferencias para que entienda mejor tu estilo y tus requisitos de programación. Te ayudan a obtener sugerencias y fragmentos de código más pertinentes. Puedes especificar las convenciones de programación, las bibliotecas y los tipos de comentarios que prefieres. Puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`: un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información pertinente para la mayoría de las interacciones con Copilot, como la pila tecnológica, una descripción general del proyecto, prácticas recomendadas y otras pautas globales.
- `.github/instructions/*.instructions.md`: archivos que puedes crear para tareas o tipos de archivos concretos. Permiten definir pautas para lenguajes como TypeScript o Astro, o para tareas como crear un componente de interfaz de usuario o un conjunto de pruebas unitarias.

> [!NOTE]
> En un IDE, los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para las sugerencias de código ni las sugerencias de edición siguiente.
>
> Copilot Chat, Copilot CLI y el agente en la nube de Copilot usan tanto las instrucciones del repositorio como los archivos `*.instructions.md` (con metadatos `applyTo` en el frontmatter) al generar código.
>
> Copilot también [admite otros formatos de archivos de instrucciones][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para administrar archivos de instrucciones

Una conversación completa sobre cómo crear archivos de instrucciones queda fuera del alcance del taller. Sin embargo, los ejemplos del proyecto muestran un enfoque representativo. A grandes rasgos:

- Limita las instrucciones de `copilot-instructions.md` a pautas del proyecto, como su descripción, estructura y estándares globales de programación.
- Usa `*.instructions.md` para pautas de tipos de archivos (pruebas unitarias, componentes Astro y la capa de datos) o tareas concretas.
- Usa lenguaje natural y mantén las pautas claras. Añade ejemplos de cómo debería ser el código y de cómo no debería serlo.

No hay una única manera correcta de crear archivos de instrucciones, igual que tampoco hay una sola forma correcta de usar la IA. La experimentación te ayudará a encontrar la que mejor se adapte al proyecto.

> [!TIP]
> Todos los proyectos que usan GitHub Copilot deberían tener un conjunto sólido de archivos de instrucciones. Al explorar los archivos de este proyecto, puede que veas instrucciones para distintos tipos de tareas, como [actualizar la interfaz de usuario][ui-instructions] y trabajar con [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta opción de forma distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. La lección correspondiente al entorno que estés usando lo indicará cuando sea pertinente.
>
> ¿Buscas plantillas o un punto de partida? Explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizados del proyecto

Dedica un momento a leer los archivos de instrucciones incluidos en este repositorio: hay un `copilot-instructions.md` principal y varios archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la breve descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. Dentro de **Code standards**, observa las pautas anidadas para los flujos de trabajo de GitHub Actions. Se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y explora su contenido. Verás instrucciones para archivos Astro, la capa de datos Drizzle, pruebas y otros elementos.
4. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo` del principio, que define un patrón glob (relativo a la raíz del repositorio) para determinar a qué archivos se aplican las instrucciones. En este caso, coincide con cualquier archivo de prueba TypeScript, por ejemplo, uno que cumpla `**/*.test.ts`.
5. Fíjate en las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y señalar a Copilot ejemplos que puede seguir. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero aún no exige documentación dentro del código. En los siguientes pasos añadirás reglas para los comentarios TSDoc y los comentarios de cabecera de los archivos.

## Crea una rama para los cambios

Sigamos algunas prácticas recomendadas y creemos una rama para los cambios.

1. Vuelve al Codespace del ejercicio anterior.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usa Copilot Chat *antes* de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando corresponda.

Para observar el efecto de las instrucciones personalizadas, empieza enviando una solicitud con el archivo de instrucciones actual. Más adelante lo actualizarás y volverás a enviar la misma solicitud para comparar los resultados.

1. Cierra las pestañas abiertas del editor para que Copilot use solo el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para indicar a Copilot dónde debe ubicarse la función auxiliar.
3. En el menú desplegable de agentes de la vista Chat, selecciona **Agent** para que Copilot pueda aplicar cambios a los archivos.

   ![Captura de pantalla del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envía esta solicitud:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explorará el proyecto y aplicará cambios de código, normalmente en el archivo auxiliar y sus pruebas.
6. Observa que la función auxiliar propuesta tiene tipos, acepta un cliente `db` como primer argumento y devuelve una matriz de editoriales con tipos. Esto se debe a las convenciones de la capa de datos definidas en `.github/instructions/drizzle.instructions.md`, aplicable a `src/lib/*.ts`.
7. Fíjate en que al código propuesto **le faltan** comentarios TSDoc y un comentario de cabecera del archivo.

> [!CAUTION]
> Como Copilot no es determinista, puede que añada comentarios de documentación aunque no se le indique. No pasa nada; el objetivo es observar la mejora de coherencia después de actualizar las instrucciones.

## Añade una nueva norma para el repositorio

Como se indicó antes, `.github/copilot-instructions.md` proporciona información del proyecto a Copilot. Documentemos los estándares de programación del repositorio para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Describe los estándares de programación, pero todavía no incluye reglas para la documentación en el código; por eso a la función auxiliar generada le faltaban comentarios.
3. Añade las siguientes líneas de Markdown debajo de los estándares existentes para indicar que Copilot debe añadir comentarios de cabecera y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, los archivos de instrucciones se pueden crear en el nivel del repositorio (`.github/copilot-instructions.md`) para definir pautas globales, o como archivos `*.instructions.md` para lenguajes, tipos de archivos o tareas concretos. El archivo del repositorio es el lugar adecuado para estándares aplicables a todo el proyecto, como la regla sobre los comentarios de documentación que acabas de añadir.

## Vuelve a enviar la solicitud y observa los cambios

1. Vuelve a Copilot Chat y selecciona **New Chat** para borrar el contenido de la conversación anterior.
2. Selecciona `src/lib/publishers.ts` para que Copilot se centre en el archivo adecuado.
3. Envía la **misma solicitud** que antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que el archivo propuesto ahora empieza con un bloque de comentarios parecido al siguiente:

   ```typescript
   /**
    * Funciones auxiliares de acceso a datos de editoriales para la plataforma de financiación colectiva Tailspin Toys.
    * Proporciona funciones para recuperar información de editoriales de la base de datos.
    */
   ```

5. Observa que la función propuesta incluye ahora un comentario TSDoc parecido al siguiente:

   ```typescript
   /**
    * Devuelve una lista de todas las editoriales con su identificador y nombre.
    *
    * @param db - Cliente de base de datos de Drizzle.
    * @returns Una promesa que se resuelve en una matriz de objetos de editorial.
    */
   ```

Has orientado a Copilot para que siga un estándar nuevo y lo aplique al código real que utilizarás en el siguiente ejercicio.

## Confirma las instrucciones y publica la rama

Los archivos de instrucciones son recursos del repositorio y se administran con el mismo sistema de control de versiones que los demás archivos. Confirmemos el cambio y publiquemos la rama.

1. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Ejecuta este comando para comprobar que están los cambios de instrucciones y de la función auxiliar:

   ```bash
   git status
   ```

3. Prepara y confirma el cambio de instrucciones y todos los archivos modificados por Copilot para crear la función auxiliar:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra otros cambios relacionados (por ejemplo, pruebas), añade también esos archivos.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Publica la rama en el repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y combina una solicitud de cambios

Ya has publicado la rama. Ahora crea una solicitud de cambios vinculada a la incidencia sobre las instrucciones de la lista de tareas pendientes. Podrías hacerlo manualmente, pero Copilot puede buscar la incidencia, crear la solicitud y combinarla.

> [!NOTE]
> En este ejercicio Copilot interactúa por primera vez con GitHub. La plantilla del proyecto ya incluye las herramientas de GitHub, así que no tienes que configurar nada. La primera vez que Copilot use una de ellas, VS Code puede pedirte que inicies sesión en GitHub. Sigue las indicaciones para autorizarlo. En un ejercicio posterior aprenderás cómo funciona esta conexión (Model Context Protocol).

1. Abre Copilot Chat en el Codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y comprueba que **Agent** está seleccionado.
3. Pide a Copilot que encuentre la incidencia sobre la actualización de los archivos de instrucciones y cree una solicitud de cambios desde la rama actual que describa tanto las instrucciones como la base de la función auxiliar de editoriales:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot buscará la incidencia y creará la solicitud de cambios.
5. Cuando se te pida **Allow** para que Copilot realice acciones de GitHub, revisa el comando y selecciona **Allow** si corresponde.
6. Cuando se cree la solicitud, pide a Copilot que la combine y vuelva a cambiar la rama local a `main`:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Cuando se te pida **Allow** para que Copilot realice acciones de GitHub y ejecute comandos de shell, revisa el comando y selecciona **Allow** si corresponde.

¡Has creado y combinado una solicitud de cambios con ayuda de GitHub Copilot!

## Resumen y siguientes pasos

Has explorado cómo usa Copilot los archivos de instrucciones del proyecto y has usado Copilot Chat en VS Code para:

- enviar una solicitud de generación de código y observar el resultado con las instrucciones *existentes*.
- añadir una nueva norma para todo el repositorio en `.github/copilot-instructions.md`.
- volver a enviar la misma solicitud y observar cómo el código propuesto sigue la nueva norma.
- confirmar el cambio de instrucciones y la base de la función auxiliar en `main` para que el siguiente ejercicio pueda aprovecharlos.

A continuación, pondrás esas instrucciones en práctica en el [modo Agente][next-lesson], cuando Copilot añada una nueva funcionalidad en varias partes de la base de código.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personalizadas personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
