---
title: "Ejercicio 2 - Agente en la nube de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que muy pocas organizaciones —o ninguna— estén libres de deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funcionalidades que llevan tiempo en la lista de tareas pendientes porque nunca hubo tiempo para implementarlas. El agente en la nube de GitHub Copilot está diseñado para tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, genera una solicitud de incorporación de cambios (PR) en borrador para que la revise una persona. Así puedes delegar tareas tediosas, acelerar el desarrollo y centrarte en cuestiones de mayor alcance.

Con el agente en la nube de Copilot explorarás:

- cómo personalizar el entorno para generar código;
- cómo garantizar que las operaciones se realicen de forma segura;
- por qué las incidencias deben tener un alcance claro;
- cómo asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica que quiere resolver. Los contratistas que crearon la primera versión del sitio dejaron la documentación en un estado poco satisfactorio: comprobarás que, en realidad, no hay documentación. El primer paso es añadir comentarios TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere que sea más fácil descubrir juegos. En la página de detalles de cada juego quiere mostrar otros juegos relacionados, es decir, títulos de la misma categoría, para que las personas puedan seguir explorando. Por ahora no hace falta una implementación pulida: solo necesitan algo que permita hacer pruebas de aceptación de la experiencia de usuario. Aunque esto bloquea su trabajo, hay otras incidencias con mayor prioridad.

Ambas son tareas que suelen quedar relegadas y que se pueden asignar al agente en la nube de Copilot. El agente puede trabajar en segundo plano mientras el desarrollador se ocupa de otras tareas y, después, revisar su trabajo y comprobar que el resultado es correcto.

## Introducción al agente en la nube de GitHub Copilot

El [agente en la nube de GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma parecida a un desarrollador. Al igual que con una persona, puedes asignarle trabajo de distintas maneras, por ejemplo, [asignándole una incidencia de GitHub](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Cuando se le asigna una tarea, Copilot crea una solicitud de incorporación de cambios en borrador para seguir el progreso, configura un entorno y empieza a trabajar. Puedes consultar la sesión mientras sigue activa o después de que termine. Cuando la solución propuesta esté lista para revisarse, Copilot te mencionará en la solicitud.

## La importancia de definir bien las instrucciones

GitHub Copilot no hace magia: no puedes saltarte el trabajo de análisis. Incluso las operaciones que parecen sencillas pueden resultar complejas al examinarlas con detalle, así que [define cuidadosamente el alcance de las tareas para el agente en la nube de Copilot][cloud-agent-best-practices]. Trátalo como a un asistente de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. La IA generativa no cambia los fundamentos del desarrollo de software.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has visto cómo las instrucciones personalizadas orientan a Copilot. Si ya has completado un ejercicio práctico sobre instrucciones personalizadas, habrás visto `.github/copilot-instructions.md` en acción; si no, este es un buen momento para echarle un vistazo. Antes de asignar trabajo al agente en la nube de Copilot, examina los archivos de instrucciones que ya incluye este repositorio. Así podrás observar su efecto más adelante.

Abre los siguientes archivos en la interfaz web de GitHub del repositorio o en un codespace que ya tengas en ejecución:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, en especial las expectativas sobre las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en el frontmatter `applyTo`, que aplica estas instrucciones a los archivos `**/*.test.ts`.

Cuando en la siguiente sección asignes al agente en la nube la incidencia `Code lacks documentation`, fíjate en si la solicitud de incorporación de cambios resultante incluye comentarios TSDoc, sigue las convenciones de TypeScript y añade encabezados de comentarios. Estas directrices proceden de los archivos de instrucciones que acabas de revisar. Volveremos a señalarlo al revisar las solicitudes en un ejercicio posterior.

## Configura el entorno de desarrollo del agente en la nube de Copilot

Para desarrollar código, tanto si lo haces tú como si delegas el trabajo, suele ser necesario configurar el entorno y ejecutar scripts de preparación. Lo mismo ocurre cuando asignas tareas a Copilot, que trabaja de forma similar a un ingeniero de software.

El agente en la nube usa [GitHub Actions][github-actions] como entorno de trabajo. Puedes personalizarlo mediante un [flujo de trabajo de configuración especial][setup-workflow] en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que empiece a trabajar. De este modo, puede acceder a las herramientas y dependencias de desarrollo necesarias. Este flujo ya está configurado para facilitar el taller y permitirte explorar esta opción. Proporciona Node.js, las dependencias del proyecto, los binarios del navegador para las pruebas de extremo a extremo y la base de datos SQLite migrada y precargada de la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio.
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Define los permisos mínimos necesarios para *tus pasos*. Copilot recibirá su propio token para las operaciones.
    permissions:
      # Si quieres clonar el repositorio como parte de los pasos de configuración (por ejemplo, para instalar dependencias), necesitarás el permiso `contents: read`. Si no lo clonas en estos pasos, Copilot lo hará automáticamente cuando terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración del frontend y la aplicación: Node.js (toda la aplicación utiliza ahora Astro + Drizzle/libSQL).
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y precarga la base de datos SQLite local para que las compilaciones y las pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

Este archivo se parece a cualquier otro flujo de trabajo de GitHub, pero tiene algunos aspectos importantes:

- Contiene un único trabajo llamado `copilot-setup-steps`. Se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la solicitud de incorporación de cambios.
- Observa el desencadenador `workflow_dispatch`, que permite ejecutar el flujo de trabajo manualmente desde la pestaña **Actions** del repositorio. Resulta útil para comprobar que funciona correctamente sin tener que esperar a que Copilot lo ejecute.

## Añade documentación

Aunque todo el mundo reconoce la importancia de la documentación, muchos proyectos tienen información obsoleta o carecen de ella. Esa es una deuda técnica que suele quedar sin resolver, reduce la productividad y dificulta el mantenimiento del código o la incorporación de nuevos miembros al equipo. Copilot destaca en la creación de documentación, así que esta es una incidencia ideal para asignarle al agente en la nube. Trabajará en segundo plano para generar la documentación necesaria. En un ejercicio posterior volverás a revisar su trabajo.

1. En una nueva pestaña del navegador, abre el repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue** para crear una incidencia en blanco.
5. Escribe `Code lacks documentation` en **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

  ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

  ![Detalles de la asignación de Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecciona la pestaña **Pull Requests**.
11. Abre la solicitud de incorporación de cambios recién generada, cuyo título será similar a `[WIP]: Code lacks documentation`. Si no aparece, espera un momento y actualiza la ventana del navegador.
12. Al cabo de unos minutos, Copilot debería haber creado una lista de tareas.

> [!NOTE]
> La lista de tareas puede tardar varios minutos en aparecer en la solicitud. Copilot está preparando su entorno (ejecuta el flujo de trabajo que se ha explicado antes), analizando el proyecto y determinando cómo abordar la tarea.

13. Revisa la lista y las tareas que va a completar.
14. Desplázate por la cronología de la solicitud de incorporación de cambios. Deberías ver una actualización que indica que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

  ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la ventana para ver el indicador actualizado.

16. Observa que puedes recorrer la sesión en directo y ver cómo Copilot resuelve el problema: explora el código y comprende su estado, se detiene a analizar y decidir el plan adecuado y crea código.

El proceso puede tardar varios minutos. Uno de los objetivos principales del agente en la nube de Copilot es que trabaje de forma asíncrona para que puedas ocuparte de otras tareas. Aprovecharemos esa característica: asignaremos otra tarea al agente en la nube y, mientras trabaja, nos centraremos en crear funcionalidades para la aplicación.

## Añade una sección de juegos relacionados a la página de detalles

Una de las ventajas del agente en la nube de GitHub Copilot es que permite repartir el trabajo. Tú puedes ocuparte de unas tareas mientras el agente trabaja en otras. Puede que añadir una sección de juegos relacionados no lleve mucho tiempo al equipo de diseño, pero ese tiempo se puede dedicar a otras tareas. Asignemos el trabajo al agente en la nube de Copilot.

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue** para usar la plantilla en blanco.
5. Escribe `Show related games on the game details page` en **Title**.
6. Escribe lo siguiente en **Description**:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

  ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Al poco tiempo, verás un 👀 en el primer comentario de la incidencia. Indica que Copilot está trabajando en la tarea.

![Copilot utiliza el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en la segunda solicitud. El agente en la nube de Copilot funciona de forma parecida a un ingeniero de software: no hace falta supervisarlo continuamente, puedes revisar el trabajo cuando termine. Mientras tanto, vamos a crear y utilizar agentes personalizados.

## Resumen y pasos siguientes

En esta lección has explorado el [agente en la nube de GitHub Copilot][copilot-agents]. Puedes asignarle incidencias a Copilot para que las resuelva de forma asíncrona. Puede ayudar a abordar deuda técnica, crear funcionalidades o migrar código de un framework a otro.

Has explorado estos conceptos:

- personalizar el entorno para generar código;
- garantizar que las operaciones se realicen de forma segura;
- definir con claridad el alcance de las incidencias;
- asignar incidencias a Copilot.

Mientras el agente en la nube trabaja en segundo plano, puedes empezar a crear y utilizar agentes personalizados. [El agente en la nube de Copilot también admite servidores MCP][cloud-agent-mcp] y utiliza las instrucciones personalizadas que has explorado en módulos anteriores.

## Recursos

- [Acerca del agente en la nube de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube de Copilot][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
