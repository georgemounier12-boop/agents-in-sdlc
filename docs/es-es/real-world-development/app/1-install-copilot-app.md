---
title: "Lección 1 - Instala la aplicación GitHub Copilot"
description: "Instala la aplicación GitHub Copilot, conecta el repositorio que has creado a partir de la plantilla, familiarízate con el espacio de trabajo y prueba un chat rápido."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La **[aplicación GitHub Copilot][about-copilot-app]** es una aplicación de escritorio para el desarrollo dirigido por agentes. Se basa en GitHub Copilot CLI y se integra de forma nativa con GitHub, por lo que los repositorios, las ramas y las canalizaciones de CI funcionan sin configuración adicional. Está diseñada para flujos de trabajo en los que diriges varios agentes en paralelo, cada uno en su propio espacio de trabajo aislado, en vez de hacer todo el trabajo por tu cuenta; también permite automatizar tareas repetitivas. Ya tienes Node.js instalado y una copia del proyecto preparada. Ahora instalarás la aplicación y conectarás el repositorio.

En esta lección:

- instalarás la aplicación GitHub Copilot e iniciarás sesión.
- añadirás el proyecto a la aplicación desde su repositorio de GitHub.
- conocerás el espacio de trabajo, incluida la lista de tareas pendientes creada por la plantilla.
- probarás un chat rápido para conocer la propia aplicación.

## Escenario

Tu equipo está adoptando agentes de IA para abordar una lista de tareas pendientes cada vez mayor. La aplicación Copilot ofrece un único lugar desde el que dirigir ese trabajo: seleccionar incidencias, ejecutar agentes, revisar cambios y combinar solicitudes de cambios. En esta lección instalarás y conectarás la aplicación y aprenderás a iniciar una conversación sobre el proyecto.

> [!NOTE]
> Se necesita un plan de Copilot válido: Copilot Free, Copilot Student o cualquier plan de pago (Pro, Pro+, Business o Enterprise). Si usas Copilot Business o Copilot Enterprise, el administrador debe mantener habilitada la directiva **GitHub Copilot app**.

## Instala y configura la aplicación GitHub Copilot

Para usar la aplicación GitHub Copilot, primero tienes que instalarla. Hay versiones para Windows, macOS y Linux. Instalemos la aplicación, autentiquémonos y añadamos el repositorio de Tailspin Toys.

1. En un navegador, abre la [página de la aplicación GitHub Copilot][download-app].
2. Descarga la aplicación para tu plataforma e instálala siguiendo las instrucciones de la página.
3. Abre la aplicación después de instalarla.
4. Selecciona **Sign in to GitHub** y sigue las indicaciones para autenticarte. Si usas GitHub Enterprise Cloud con residencia de datos, selecciona **Use GitHub Enterprise** e introduce el nombre de host `*.ghe.com` cuando se te solicite. GitHub Enterprise Server no es compatible.
5. Si el proceso de incorporación te ofrece seleccionar repositorios, selecciona el repositorio de Tailspin Toys que creaste a partir de la plantilla. También puedes omitir este paso y añadir el proyecto después.
6. Cuando se te solicite, elige un tema y termina el proceso de incorporación.
7. Busca el repositorio en **Sessions**, en la barra lateral. Si ya aparece en la lista, no hace falta volver a añadirlo.
8. Si el repositorio aún no está conectado, selecciona **+** junto a **Sessions** para crear una sesión y, después, selecciona **Add GitHub repository**. Escribe el propietario y el nombre del repositorio que anotaste en la lección de requisitos previos.
9. Comprueba que el repositorio aparece en la barra lateral. En esta disposición, las carpetas de repositorio, como `tailspin-toys`, se agrupan en **Sessions**, y las sesiones individuales aparecen debajo de cada repositorio.

## Familiarízate con el espacio de trabajo

Con el proyecto conectado, dedica un momento a conocer el espacio de trabajo. La aplicación organiza el contenido en varias áreas de la barra lateral:

- **New**: inicia una sesión de chat con Copilot.
- **Pull requests**: muestra las solicitudes de cambios mediante la integración nativa con GitHub. Desde aquí puedes explorarlas y filtrarlas, comprobar el estado de CI y revisarlas sin salir de la aplicación.
- **Issues**: muestra las incidencias del mismo modo. Desde aquí puedes explorarlas y filtrarlas, e iniciar una sesión a partir de una incidencia.
- **Automations**: contiene tareas de agente guardadas que se ejecutan según una programación o bajo demanda. Son útiles para gestionar listas de tareas, realizar el mantenimiento periódico del proyecto o delegar otras tareas tediosas. En el resumen se mencionan como un siguiente paso, no como otro ejercicio del taller.
- **Customize**: añade funcionalidades a la aplicación Copilot mediante servidores MCP, plugins, habilidades y otros componentes. Lo utilizarás para configurar Playwright MCP.
- **Chats**: contiene conversaciones ligeras para preguntas y lluvias de ideas que no necesitan una rama ni un espacio de trabajo propios. Esta sección aparece después de iniciar el primer chat. Si aún no aparece, selecciona **+** junto a **Sessions** para iniciar un chat.
- **Sessions**: agrupa las carpetas de repositorios y sus sesiones de agente. Selecciona el nombre de un repositorio, como `tailspin-toys`, para abrir su vista, o selecciona una sesión debajo para volver a esa conversación. Cada sesión se ejecuta en su propio espacio de trabajo aislado, por lo que puedes ejecutar varias a la vez sin que sus cambios entren en conflicto. Iniciarás tu primera sesión cuando añadas las valoraciones por estrellas.

Durante el taller explorarás el espacio de trabajo.

> [!TIP]
> Si tienes dudas, pregunta a Copilot. Si no sabes cómo hacer algo o si es posible, puedes preguntarle y te ayudará a orientarte.

### Busca la lista de tareas pendientes creada por la plantilla

Probablemente todos los proyectos tienen tareas pendientes, y Tailspin Toys no es una excepción. Exploremos las incidencias creadas al generar el repositorio a partir de la plantilla.

1. En **Sessions**, en la barra lateral, selecciona la carpeta del repositorio Tailspin Toys, no una sesión individual. Después, selecciona **Issues** en la navegación superior del repositorio.
2. Busca estas incidencias por el título, sin dar por sentado cuál es su número:

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Selecciona una incidencia y lee sus detalles. Cada incidencia también permite iniciar una sesión de agente. Partirás de la incidencia de filtrado después de completar un primer cambio rápido.

> [!NOTE]
> Las listas de **Issues** y **Pull requests** se filtran automáticamente para mostrar solo los elementos de los repositorios que has añadido a la aplicación Copilot. Para ver elementos de otros repositorios, añádelos a la aplicación.

## Prueba un chat rápido

Una buena forma de familiarizarse con la aplicación es utilizarla para conocer la *propia aplicación*. Un **chat rápido** es justo la herramienta adecuada: permite formular una pregunta o plantear ideas sin crear una rama ni un árbol de trabajo, por lo que sirve para una consulta rápida y desechable que no requiere una sesión.

1. En la barra lateral, selecciona **+** junto a **Chats** para abrir un chat nuevo. Si **Chats** aún no está visible, selecciona **+** junto a **Sessions** para iniciar el primer chat; **Chats** aparecerá después.
2. Pregunta a la aplicación cómo funcionan sus sesiones:

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. Lee la respuesta en la vista de conversación. Verás que cada sesión se ejecuta en su propio árbol de trabajo Git aislado, lo que permite ejecutar varios agentes en paralelo sin que sus cambios entren en conflicto. Puedes continuar la conversación o iniciar un chat nuevo en cualquier momento.

## Resumen y siguientes pasos

¡Enhorabuena! Has instalado la aplicación GitHub Copilot, conectado el proyecto y explorado el espacio de trabajo. Has aprendido a:

- instalar la aplicación e iniciar sesión en GitHub.
- añadir un proyecto desde su repositorio de GitHub.
- familiarizarte con el espacio de trabajo y localizar la lista de tareas pendientes inicial en **Issues**.
- usar un chat rápido para formular una pregunta breve y desechable.

A continuación, [iniciarás tu primera sesión de agente][next-lesson] y mostrarás una valoración con estrellas en las tarjetas de juegos.

## Recursos

- [Acerca de la aplicación GitHub Copilot][about-copilot-app]
- [Introducción a la aplicación GitHub Copilot][getting-started]
- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
