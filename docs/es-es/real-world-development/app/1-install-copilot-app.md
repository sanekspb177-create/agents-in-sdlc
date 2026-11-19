---
title: "Lección 1 - Instalar la aplicación GitHub Copilot"
description: "Instala la aplicación GitHub Copilot, conecta el repositorio que has creado a partir de la plantilla, familiarízate con el espacio de trabajo y prueba un chat rápido."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La **[aplicación GitHub Copilot][about-copilot-app]** es una aplicación de escritorio para el desarrollo dirigido por agentes. Se basa en GitHub Copilot CLI y se integra de forma nativa con GitHub, por lo que los repositorios, las ramas y las canalizaciones de CI funcionan sin configuración adicional. Está diseñada para flujos de trabajo en los que diriges varios agentes en paralelo, cada uno en su propio espacio de trabajo aislado, en lugar de realizar todo el trabajo y automatizar las tareas repetitivas por tu cuenta. Con Node.js instalado y tu copia del proyecto preparada, el siguiente paso es instalar la aplicación y conectar ese repositorio.

En esta lección:

- instalarás la aplicación GitHub Copilot e iniciarás sesión;
- añadirás el proyecto a la aplicación desde su repositorio de GitHub;
- conocerás el espacio de trabajo, incluida la lista de tareas pendientes que creó la plantilla;
- probarás un chat rápido para obtener información sobre la propia aplicación.

## Escenario

Tu equipo está adoptando agentes de IA para abordar una lista creciente de tareas pendientes. La aplicación Copilot ofrece un único lugar desde el que dirigir ese trabajo: seleccionar incidencias, ejecutar agentes, revisar cambios y combinar solicitudes de incorporación de cambios. En esta lección instalarás y conectarás la aplicación, y aprenderás a iniciar una conversación sobre el proyecto.

> [!NOTE]
> Se necesita un plan de Copilot válido: Copilot Free, Copilot Student o cualquier plan de pago (Pro, Pro+, Business o Enterprise). Si utilizas Copilot Business o Copilot Enterprise, el administrador debe mantener habilitada la directiva **GitHub Copilot app**.

## Instala y configura la aplicación GitHub Copilot

Como cabe esperar, el primer paso para utilizar la aplicación GitHub Copilot es instalarla. Hay versiones disponibles para Windows, macOS y Linux. Vamos a instalar la aplicación, autenticarnos y añadir el repositorio Tailspin Toys.

1. En un navegador, abre la [página de inicio de la aplicación GitHub Copilot][download-app].
2. Descarga la aplicación para tu plataforma e instálala siguiendo las instrucciones de la página.
3. Abre la aplicación después de instalarla.
4. Selecciona **Sign in to GitHub** y sigue las indicaciones para iniciar sesión con una cuenta de GitHub.com o GHE.com. Si utilizas GitHub Enterprise Cloud con residencia de datos, elige **GitHub Enterprise Cloud** e introduce el nombre de host `*.ghe.com` cuando se te solicite.

   Si utilizas GitHub Enterprise Server, completa primero la incorporación con tu cuenta en la nube. Después, abre **Settings**, selecciona **Accounts**, selecciona **Add account** y elige **GitHub Enterprise Server**. Sigue las instrucciones para autorizar la aplicación en el navegador, como se describe en la [guía de inicio de sesión en Enterprise Server][ghes-sign-in].

5. Si durante la incorporación se te pide que selecciones un repositorio, selecciona el repositorio Tailspin Toys que has creado a partir de la plantilla. También puedes omitir este paso y añadir el proyecto después.
6. Cuando se te solicite, elige un tema y termina el proceso de incorporación.
7. Busca el repositorio en **Sessions**, en la barra lateral. Si ya aparece en la lista, no hace falta volver a añadirlo.
8. Si todavía no está conectado, selecciona **+** junto a **Sessions** para crear una sesión y, después, selecciona **Add GitHub repository**. Escribe el propietario y el nombre del repositorio que anotaste en la lección de requisitos previos.
9. Confirma que el repositorio aparece en la barra lateral. En esta vista, las carpetas de repositorio, como `tailspin-toys`, aparecen agrupadas en **Sessions**, con sus sesiones debajo de cada repositorio.

## Familiarízate con el espacio de trabajo

Con el proyecto conectado, dedica un momento a conocer el espacio de trabajo. La aplicación organiza todo en varias áreas de la barra lateral:

- **New**: aquí puedes iniciar una nueva sesión de chat con Copilot.
- **Pull requests**: tus solicitudes de incorporación de cambios, disponibles mediante la integración nativa con GitHub de la aplicación. Puedes examinarlas y filtrarlas, comprobar el estado de CI y revisarlas sin salir de la aplicación.
- **Issues**: tus incidencias, disponibles del mismo modo. Puedes examinarlas y filtrarlas e iniciar una sesión a partir de una incidencia.
- **Automations**: tareas de agente guardadas que se ejecutan según una programación o bajo demanda. Son útiles para gestionar listas de tareas, realizar el mantenimiento periódico del proyecto o delegar otras tareas tediosas. En el resumen se enlazan como un paso siguiente, no como otro ejercicio del taller.
- **Customize**: añade funciones a la aplicación Copilot mediante servidores MCP, plugins, habilidades y otros componentes. Lo utilizarás para configurar MCP de Playwright.
- **Chats**: conversaciones ligeras para preguntas e ideas que no necesitan una rama ni un espacio de trabajo propios. Esta sección aparece después de iniciar el primer chat. Si no está visible, selecciona **+** junto a **Sessions** para iniciar un chat.
- **Sessions**: agrupa las carpetas de repositorios y las sesiones de agente. Selecciona un repositorio, como `tailspin-toys`, para abrir su vista, o selecciona una sesión debajo para volver a la conversación. Cada sesión se ejecuta en su propio espacio de trabajo aislado, por lo que puedes ejecutar varias a la vez sin que sus cambios entren en conflicto. Iniciarás tu primera sesión cuando añadas las valoraciones por estrellas.

Durante el taller explorarás el espacio de trabajo.

> [!TIP]
> Si tienes dudas, pregunta a Copilot. Si no sabes cómo hacer algo o si es posible, puede orientarte.

### Localiza la lista de tareas inicial

Prácticamente todos los proyectos tienen tareas pendientes, y Tailspin Toys no es una excepción. Exploremos la lista de incidencias que se creó al generar el proyecto a partir de la plantilla.

1. En la barra lateral, selecciona la carpeta del repositorio Tailspin Toys bajo **Sessions**, no una sesión individual. Después, selecciona **Issues** en la navegación superior del repositorio.
2. Busca estas incidencias por su título en lugar de dar por hecho su número:

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Selecciona una incidencia para leer los detalles. Cada incidencia también sirve para iniciar una sesión de agente. Partirás de la incidencia de filtrado después de realizar un primer cambio rápido.

> [!NOTE]
> Las listas de **Issues** y **Pull requests** se filtran automáticamente para mostrar solo elementos de los repositorios que has añadido a la aplicación Copilot. Para ver elementos de otros repositorios, añádelos a la aplicación.

## Prueba un chat rápido

Una buena manera de familiarizarse con la aplicación es utilizarla para conocer la *propia aplicación*. Para ello, un **chat rápido** es justo lo que necesitas. Permite hacer una pregunta o plantear ideas sin crear una rama ni un worktree, por lo que resulta ideal para una consulta breve que no requiere una sesión.

1. En la barra lateral, selecciona **+** junto a **Chats** para abrir un chat. Si **Chats** aún no está visible, selecciona **+** junto a **Sessions** para iniciar el primer chat; después aparecerá **Chats**.
2. Pregunta cómo funcionan las sesiones de la aplicación:

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. Lee la respuesta en la vista de conversación. Verás que cada sesión se ejecuta en su propio worktree de Git aislado, lo que permite ejecutar varios agentes en paralelo sin que sus cambios entren en conflicto. Puedes continuar la conversación o iniciar otra cuando quieras.

## Resumen y pasos siguientes

Has instalado la aplicación GitHub Copilot, conectado el proyecto y explorado el espacio de trabajo. Has aprendido a:

- instalar la aplicación e iniciar sesión en GitHub;
- añadir un proyecto desde su repositorio de GitHub;
- familiarizarte con el espacio de trabajo y localizar la lista de tareas inicial en **Issues**;
- utilizar un chat rápido para formular una pregunta breve y puntual.

A continuación, [iniciarás tu primera sesión de agente][next-lesson] y mostrarás una valoración por estrellas en las tarjetas de juegos.

## Recursos

- [Acerca de la aplicación GitHub Copilot][about-copilot-app]
- [Introducción a la aplicación GitHub Copilot][getting-started]
- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
[ghes-sign-in]: https://docs.github.com/enterprise-server@3.22/copilot/copilot-on-ghes/sign-in-on-copilot-app
