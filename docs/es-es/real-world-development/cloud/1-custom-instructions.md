---
title: "Ejercicio 1 - Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Agente en la nube de Copilot →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una manera determinada, o Copilot necesita conocer información de contexto, debes asegurarte de que pueda acceder a ella. Los [archivos de instrucciones][instruction-files] proporcionan esa orientación para que Copilot entienda no solo *qué* quieres que haga, sino también *cómo* quieres que lo haga.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto específico del proyecto, las pautas de programación y los estándares de documentación mediante instrucciones personalizadas del repositorio y archivos de instrucciones aplicables a rutas concretas.
- añadirás una nueva norma para todo el repositorio en `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los recorridos de VS Code y CLI, aquí no compararás una solicitud anterior y otra posterior. El agente en la nube de Copilot trabaja de forma asíncrona con incidencias de GitHub, por lo que es más difícil mostrar el efecto en tiempo real. Verás la influencia del archivo de instrucciones más adelante, al revisar las solicitudes de cambios que produzca el agente en la nube.

## Archivos de instrucciones

### Escenario

Como cualquier empresa de desarrollo competente, Tailspin Toys tiene pautas y requisitos para sus prácticas de desarrollo. Entre ellos:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz de usuario debe usar un tema oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe haber un bloque de comentarios que describa su contenido.

Mediante archivos de instrucciones, te asegurarás de que Copilot tenga la información necesaria para realizar las tareas conforme a estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas permiten proporcionar a Copilot contexto y preferencias para que entienda mejor tu estilo y tus requisitos de programación. Te ayudan a obtener sugerencias y fragmentos de código más pertinentes. Puedes especificar las convenciones de programación, las bibliotecas y los tipos de comentarios que prefieres. Puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`: un único archivo de instrucciones que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información pertinente para la mayoría de las solicitudes, como la pila tecnológica, una descripción general del proyecto, las prácticas recomendadas y otras pautas globales.
- `.github/instructions/*.instructions.md`: archivos que se pueden crear para tareas o tipos de archivos concretos. Permiten definir pautas para lenguajes como TypeScript o Astro, o para tareas como crear un componente de interfaz de usuario o un conjunto de pruebas unitarias.

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

## Añade una nueva norma para el repositorio

Este es el único cambio que harás en este ejercicio: añadir una regla para todo el proyecto que indique que la documentación debe incluirse en el código mediante comentarios TSDoc y un comentario de cabecera en cada archivo. El agente en la nube aplicará esta regla al trabajar en las incidencias que le asignes más adelante.

Antes de editar, crea una rama. El agente en la nube leerá las instrucciones de la rama a la que se dirija la incidencia, pero así también podrás combinar los cambios de forma ordenada:

1. En el terminal del Codespace, crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como se indicó antes, `.github/copilot-instructions.md` proporciona información del proyecto a Copilot. Documentemos los estándares de programación del repositorio para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Describe los estándares de programación del proyecto, pero todavía no incluye reglas para la documentación en el código; por eso el asistente generado no tenía comentarios de documentación.
3. Añade las siguientes líneas de Markdown justo después de los estándares existentes para indicar que Copilot debe añadir comentarios de cabecera y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, los archivos de instrucciones se pueden crear en el nivel del repositorio (`.github/copilot-instructions.md`) para definir pautas globales, o como archivos `*.instructions.md` para lenguajes, tipos de archivos o tareas concretos. El archivo del repositorio es el lugar adecuado para estándares aplicables a todo el proyecto, como la regla sobre los comentarios de documentación que acabas de añadir.

## Confirma, publica y combina el cambio de instrucciones

El agente en la nube lee los archivos de instrucciones de la rama a la que se dirige la incidencia. Cuando le asignes una incidencia en el siguiente ejercicio, Copilot creará una rama desde `main`; por tanto, los cambios de instrucciones deben llegar a `main` para que el agente los aplique.

1. Prepara y confirma el cambio:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Publica la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de cambios de `custom-instructions-cloud` a `main` y combínala. Así, el agente en la nube leerá estas instrucciones cuando trabaje en las incidencias que le asignes en el siguiente ejercicio.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante este taller, puedes omitir la rama y confirmar el cambio directamente en `main`. El paso de la rama permite practicar un flujo de trabajo habitual en un proyecto real.

## Resumen y siguientes pasos

Has explorado cómo utiliza Copilot el contexto de los archivos de instrucciones y has añadido una nueva norma para todo el repositorio en `.github/copilot-instructions.md`. Verás cómo se aplica en las solicitudes de cambios que genere el agente en la nube durante el resto de este recorrido.

A continuación, [asignarás tu primera incidencia al agente en la nube de Copilot][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
