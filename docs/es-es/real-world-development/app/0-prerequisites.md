---
title: "Lección 0 - Requisitos previos"
description: "Prepara el entorno para las lecciones de la aplicación GitHub Copilot: instala Node.js para Tailspin Toys y crea tu propia copia del repositorio a partir de la plantilla."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La aplicación GitHub Copilot es una aplicación de escritorio que funciona como centro de operaciones para Copilot y GitHub. Permite acceder rápidamente a incidencias y solicitudes de incorporación de cambios y desarrollar con GitHub Copilot. Durante este taller trabajarás en local con la aplicación Tailspin Toys, creada con Astro, utilizando la aplicación GitHub Copilot. Antes de empezar, comprueba que Node.js esté instalado en el equipo y, después, instala la aplicación Copilot.

En esta lección:

- instalarás Node.js para poder ejecutar las pruebas del proyecto en tu equipo;
- crearás tu propia copia del proyecto Tailspin Toys a partir de la plantilla.

## Instala Node.js

En varias lecciones pedirás a un agente que implemente funcionalidades y ejecute en local el conjunto de pruebas de Tailspin Toys. Para ello se necesita **[Node.js][nodejs]**, el único entorno de ejecución que requiere el proyecto. Instala la versión **LTS** actual.

La opción más sencilla en cualquier plataforma es el instalador oficial:

1. Abre una ventana de terminal en el sistema operativo con Windows Terminal, Terminal de macOS o la aplicación que utilices habitualmente.
2. Ejecuta estos comandos para comprobar las versiones instaladas de Node.js y npm:

    ```shell
    node --version
    npm --version
    ```

3. Si ambos comandos terminan correctamente y Node.js cumple los requisitos del README y de `package.json` del proyecto, puedes pasar a la sección siguiente.

> [!TIP]
> Solo tienes que completar estos pasos si no tienes Node instalado o si necesitas actualizarlo.

4. Abre la [página de descarga de Node.js][node-download].
5. Descarga la versión **LTS** para tu sistema operativo.
6. Ejecuta el instalador y acepta las opciones predeterminadas. En Windows, mantén seleccionada la opción **Add to PATH**.
7. Cuando termine la instalación, abre una nueva ventana de terminal.
8. En la nueva ventana, ejecuta estos comandos para confirmar la instalación:

    ```bash
    node --version
    npm --version
    ```

9. Deberían aparecer las versiones instaladas de Node.js y npm.

> [!IMPORTANT]
> El agente de la aplicación Copilot también debe poder ejecutar `node --version` y `npm --version`. Que funcionen en tu terminal habitual no garantiza que estén disponibles en la aplicación. Cierra la aplicación por completo y vuelve a abrirla después de instalar Node.js o cambiar `PATH`. Si alguno de los comandos sigue fallando en una sesión, sigue los [pasos para solucionar problemas del entorno de la sesión][session-environment] antes de continuar.

> [!IMPORTANT]
> Cada worktree también necesita las dependencias del proyecto y Chromium de Playwright para las comprobaciones E2E. Sigue el README del repositorio de Tailspin Toys al preparar un worktree y revisa cualquier solicitud de instalación antes de aprobarla.

## Configura el repositorio del laboratorio

Trabajarás con tu propia copia del proyecto Tailspin Toys. Créala ahora a partir del [repositorio de plantilla][template-repository]. El nuevo repositorio contiene todos los archivos necesarios para el laboratorio y lo conectarás cuando instales la aplicación.

1. En una nueva ventana del navegador, ve al repositorio de GitHub de este laboratorio: `https://github.com/github-samples/tailspin-toys`.
2. Crea tu propia copia seleccionando el botón **Use this template** en la página del repositorio del laboratorio. Después, selecciona **Create a new repository**.

    ![Botón Use this template con Create a new repository seleccionado en el menú desplegable](../../../_images/app-0-use-template.png)

3. Si participas en un taller organizado por GitHub o Microsoft, sigue las instrucciones de los mentores. En caso contrario, puedes crear el nuevo repositorio en una organización donde tengas acceso a GitHub Copilot.

    ![Formulario Create a new repository con github-samples/tailspin-toys como plantilla y el nombre del repositorio completado](../../../_images/app-0-create-repository.png)

4. Anota la ruta del repositorio que has creado (**organization-or-user-name/repository-name**), ya que la necesitarás más adelante en el laboratorio.

> [!NOTE]
> Al crear el repositorio a partir de la plantilla, se generará automáticamente una lista de tareas pendientes con incidencias de GitHub. Trabajarás con ellas durante todo el taller; no tienes que crear ninguna.

Utiliza una copia nueva de la plantilla del taller. Incluye las instrucciones del repositorio, el código de la aplicación, las pruebas, una habilidad `quality-checks` y una extensión de lienzo existente. Durante el taller personalizarás la habilidad y crearás un agente QA. Si tienes una copia anterior, confirma con la persona que imparte el taller que incluye los archivos necesarios.

## Resumen y pasos siguientes

Ya tienes todo preparado. En esta lección:

- has instalado Node.js para compilar y probar el proyecto en tu equipo;
- has creado tu propia copia del repositorio Tailspin Toys a partir de la plantilla.

A continuación, [instalarás la aplicación GitHub Copilot][next-lesson], conectarás el repositorio que acabas de crear y te familiarizarás con el espacio de trabajo.

## Recursos

- [Descargar Node.js][node-download]
- [Crear un repositorio a partir de una plantilla][template-repository]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../1-install-copilot-app/
[nodejs]: https://nodejs.org/
[node-download]: https://nodejs.org/en/download
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[session-environment]: ../3-agent-modes/#inicia-una-sesión-desde-la-incidencia
