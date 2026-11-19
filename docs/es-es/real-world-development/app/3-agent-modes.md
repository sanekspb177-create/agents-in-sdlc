---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Explora los modos de agente: utiliza Plan para acordar un enfoque, Autopilot para crear el filtrado a partir de una incidencia e Interactive para revisar y verificar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Empezamos añadiendo una pequeña funcionalidad al proyecto, pero los cambios más amplios requieren un proceso más completo. La aplicación GitHub Copilot está diseñada para adaptarse al flujo de trabajo de una organización y ayudarnos a crear lo adecuado de la manera correcta. En esta y las siguientes lecciones seguirás un proceso habitual de desarrollo con agentes: partirás de una incidencia para crear una funcionalidad, comprobarás que el código es válido y que funciona como se espera y, finalmente, la combinarás correctamente en el proyecto.

> [!NOTE]
> Utilizarás la misma sesión durante todo el flujo de trabajo de la funcionalidad. Normalmente se usarían sesiones o PR distintas para los diferentes tipos de archivo, pero tomaremos un atajo para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una nueva sesión de agente desde una incidencia de GitHub;
- definirás los requisitos en modo **Plan**;
- implementarás la funcionalidad con **Autopilot**;
- revisarás el código;
- comprobarás manualmente la funcionalidad en un lienzo de navegador.

Mientras sigues trabajando en esta funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la habilidad existente `quality-checks`, añadirás validación con MCP, crearás un agente QA y abrirás la PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys está creciendo y sus visitantes necesitan filtrar los juegos por categoría y editor. La incidencia de la lista de tareas pendientes describe la funcionalidad, pero hay que acordar algunos detalles, como la combinación de categorías, antes de programar. Utilizarás el modo Plan para resolver esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

Introducir agentes de programación con IA en el flujo de desarrollo no cambia los fundamentos. De hecho, adquieren aún más importancia. La mayoría de los desarrolladores siguen un flujo similar al siguiente:

1. Abrir una incidencia que detalle lo que debe hacerse.
2. Crear un plan de lo que debe desarrollarse.
3. Desarrollar y revisar el código.
4. Ejecutar las pruebas para validar el código.
5. Comprobar manualmente la nueva funcionalidad.
6. Crear una solicitud de incorporación de cambios (PR).
7. Una vez revisado el código y completado correctamente el proceso de integración continua, combinarlo.

> [!NOTE]
> Los detalles concretos variarán según el equipo y la organización, pero la mayoría de los procesos seguirán alguna variante de este flujo.

Al mantener este enfoque estándar, te aseguras de que el código generado por IA cumpla los requisitos establecidos y pase por el mismo proceso de revisión que el código escrito manualmente.

## Modos de sesión

El **modo de sesión** controla el grado de autonomía del agente. Puedes seleccionarlo en el menú desplegable situado debajo del cuadro de prompt y cambiarlo en cualquier momento:

- **Interactive**: trabajas junto con el agente. El agente propone cambios y espera tu respuesta antes de continuar.
- **Plan**: el agente crea primero un plan. Lo revisas y apruebas antes de que el agente lo ejecute.
- **Autopilot**: el agente trabaja de forma totalmente autónoma: escribe código, ejecuta pruebas e itera sin esperar indicaciones.

Empieza en modo Plan, revisa el plan y, después, utiliza Autopilot para implementarlo.

## Inicia una sesión desde la incidencia

Antes de empezar, confirma que la PR de las valoraciones por estrellas se ha combinado y que la rama local `main` está actualizada.

1. En la barra lateral, selecciona la carpeta del repositorio Tailspin Toys bajo **Sessions** y, después, selecciona **Issues** en la navegación superior del repositorio. Abre **Allow users to filter games by category and publisher**. Es una incidencia existente de la plantilla; no tienes que crear otra.
2. Selecciona **New session** y elige un **new working tree** basado en la rama `main` actualizada.

    ![Vista de una incidencia en la aplicación GitHub Copilot con una flecha que señala el botón New session](../../../_images/app-new-session-from-issue.png)

3. Confirma que la incidencia está adjunta a la sesión y selecciona **Plan** en el selector de modo.

> [!NOTE]
> Si no se encuentra Node.js o npm, pide al agente que ejecute `node --version` y `npm --version`. Si funcionan en el terminal pero no en la sesión, comprueba la ruta `PATH` de la aplicación o la configuración del gestor de versiones y, después, cierra y vuelve a abrir la aplicación por completo. Repite la operación y las comprobaciones que hayan fallado. No reinstales npm solo porque el agente no lo encuentre.

## Planifica la funcionalidad de filtrado

La planificación te permite revisar el enfoque antes de que Copilot escriba código. Como has iniciado la sesión desde la incidencia, Copilot ya tiene la solicitud como contexto. Envía:

```plaintext
Build this feature.
```

Responde a las preguntas de Copilot y compara el plan con los criterios de aceptación de la incidencia. Comprueba que incluye el filtrado por categoría y editor, controles accesibles, cambios de acceso a datos y pruebas. Aclara cualquier comportamiento poco definido, como la combinación de varias categorías o qué ocurre cuando ningún juego coincide.

El plan debe incluir lint, pruebas unitarias, pruebas E2E y comprobación de tipos con las herramientas existentes del proyecto. Céntralo en implementar y probar el filtrado; crearás la PR después de completar el flujo de calidad. Solicita cambios en el plan antes de aprobarlo y conserva la URL de la incidencia y las aclaraciones acordadas para validarlas más adelante.

## Aprueba Autopilot explícitamente

Cuando estés conforme con el plan, selecciona **Approve and implement this plan** o la opción equivalente de tu versión. Comprueba el modo activo en lugar de suponer que la aprobación lo cambia. Confirma que el indicador de modo muestra **Autopilot**.

Copilot comenzará a implementar la funcionalidad. Verás cómo recorre el plan acordado, genera código y ejecuta pruebas.

> [!NOTE]
> Al aprobar el plan, la implementación puede empezar inmediatamente, así que revísalo primero. Si Copilot informa de dependencias ausentes o de un conflicto de puerto, resuelve el problema de configuración antes de dar las comprobaciones por terminadas. Detén solo los servidores que hayas iniciado.

## Revisa y comprueba la implementación

Una vez generado el código, hay que revisarlo antes de combinarlo, igual que cualquier otro código. Revisaremos el código y ejecutaremos el sitio para confirmar que todo funciona correctamente.

1. Abre **Changes** e inspecciona la implementación del filtrado y las pruebas.
2. Compara el resultado con la incidencia y las aclaraciones aprobadas, incluidas las combinaciones de varias categorías y editores. Comprueba que los cambios siguen las instrucciones existentes del repositorio.
3. Revisa los resultados de lint, las pruebas unitarias, las pruebas E2E y la comprobación de tipos. Una comprobación omitida no cuenta como superada.
4. Resuelve los fallos y vuelve a ejecutar las comprobaciones afectadas antes de aceptar la implementación. La configuración E2E de Playwright compila y sirve una vista previa y puede reutilizar un servidor local; asegúrate de que el servidor probado pertenece a este worktree y no a una lección anterior.

## Explora la nueva funcionalidad

El código parece correcto, pero ¿funciona? Inicia la aplicación como antes y abre el sitio en un lienzo de navegador.

1. Utiliza este prompt para pedir a Copilot que inicie la aplicación y abra la página en el lienzo:

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. En unos instantes, la aplicación se iniciará y se abrirá una ventana del navegador dentro de la aplicación Copilot.
3. Selecciona una categoría y confirma que los resultados coinciden. Borra el filtro, selecciona un editor y confirma que los resultados coinciden con él.
4. Combina los filtros de categoría y editor y compara los resultados con la incidencia y el plan aprobado. Si se admiten varias categorías, comprueba el comportamiento acordado.
5. Prueba una combinación sin juegos coincidentes y revisa el estado vacío. Borra o restablece los filtros y confirma que vuelve a aparecer la lista completa.
6. Cuando termines, pide a Copilot que detenga el servidor de desarrollo que ha iniciado para esta sesión:

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## Conserva esta sesión para las lecciones siguientes

Todavía no crees ni combines la PR del filtrado. Conserva esta sesión, el worktree y la rama para las lecciones 4 a 8. En la siguiente lección aplicarás instrucciones personalizadas al código de filtrado; después, añadirás la habilidad y las mejoras de QA antes de crear y combinar la PR conjunta en la Lección 8. Puedes dejar la sesión inactiva después de detener el servidor y volver a ella en la siguiente lección.

## Resumen y pasos siguientes

Has utilizado distintos modos de agente para desarrollar y revisar una funcionalidad. En esta lección:

- has iniciado una sesión de agente desde una incidencia de GitHub;
- has definido los requisitos en modo **Plan**;
- has implementado la funcionalidad con **Autopilot**;
- has revisado el código;
- has comprobado manualmente la funcionalidad en un lienzo de navegador.

A continuación, profundizarás en cómo se genera el código y te asegurarás de que siga las prácticas documentadas mediante el [uso de instrucciones personalizadas][next-lesson].

## Recursos

- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]

[next-lesson]: ../4-custom-instructions/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
