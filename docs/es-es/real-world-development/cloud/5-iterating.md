---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este laboratorio has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le has pedido que documente el código, cree una funcionalidad de juegos relacionados para que el equipo de diseño pueda iterar y añada mejoras de accesibilidad, incluidos los modos de alto contraste y claro. Revisemos los cambios de código y, si hace falta, demos nuestra opinión a Copilot para mejorar su trabajo.

### Escenario

Como ya hemos señalado, los fundamentos del diseño de software y DevOps no cambian con la IA generativa. Siempre debemos revisar el código generado y seguir nuestros procesos habituales de DevOps. Antes de habilitar las revisiones para el resto del equipo, revisaremos las sugerencias de GitHub Copilot para la documentación, la funcionalidad de juegos relacionados y las mejoras de accesibilidad.

## Seguridad y agente en la nube de GitHub Copilot

Como el agente en la nube de Copilot realiza las tareas de forma asíncrona y sin supervisión, se han establecido restricciones de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utilizará para el código.
- El agente en la nube se ejecuta en GitHub Actions, dentro de un entorno independiente y efímero.
- Una persona debe aprobar todos los flujos de trabajo de GitHub Actions antes de que se ejecuten.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empecemos por la primera solicitud de incorporación de cambios (PR) que generó el agente en la nube de GitHub Copilot: la que añade documentación al código. La revisarás desde la interfaz estándar de solicitudes de incorporación de cambios de GitHub.com.

> [!NOTE]
> Al explorar la solicitud, puede que veas una advertencia que indica que el cortafuegos está bloqueando a GitHub Copilot. Es **normal**: Copilot tiene un acceso limitado a recursos externos, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el cortafuegos del agente en la nube de Copilot][agent-firewall].

1. Abre el repositorio en una pestaña nueva del navegador y ve a github.com.
2. Selecciona **Pull Requests** para abrir la lista de solicitudes.
3. Abre la solicitud cuyo título sea similar a **Add missing documentation** u otro título descriptivo.

> [!NOTE]
> Si Copilot sigue trabajando, la solicitud tendrá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar varios minutos; puedes hacer una pausa o repasar lo que has aprendido.

4. Cuando la solicitud esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluye los nuevos comentarios TSDoc y otros elementos de documentación. Los cambios exactos pueden variar.

    Al revisar los cambios, busca comentarios TSDoc, convenciones de TypeScript y encabezados de comentarios. Estas mejoras siguen los archivos de instrucciones personalizadas que revisaste al principio del Ejercicio 2.

6. Cuando hayas revisado los cambios y estés conforme, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que avisa de que algunos flujos de trabajo están pendientes de aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto para la única aplicación Astro se superarán. El proceso puede tardar varios minutos.

## Pide cambios a GitHub Copilot

Trabajar con Copilot en una solicitud no es un proceso unidireccional. Puedes mencionarlo en comentarios de la solicitud, como harías con cualquier otro miembro del equipo, o añadir comentarios en línea al código. Copilot verá esos comentarios e iniciará otra sesión para resolverlos. Como los resultados no son deterministas, no podemos sugerir un texto exacto. Por ejemplo, puedes pedirle que:

- añada al principio de cada archivo de código encabezados de comentarios con una breve descripción de su propósito;
- añada comentarios TSDoc a archivos TypeScript y Astro;
- cree un archivo README que describa la estructura de la aplicación Astro.

1. Añade un comentario para solicitar un cambio en la documentación generada y menciona a **@copilot**, como harías con cualquier usuario. Puedes utilizar una de las ideas anteriores o pedir otra mejora de documentación para el código.
2. Selecciona **View Session** para observar cómo trabaja Copilot. Fíjate en que inicia una sesión nueva para realizar los cambios.
3. Puedes seleccionar **Back to pull request** para volver a la solicitud.

    ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine los cambios, aparecerá un commit nuevo en la solicitud.
5. Selecciona la pestaña **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Después, puedes marcar la PR en borrador como lista para revisión y combinarla en la rama principal.

![Marcar la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la funcionalidad de juegos relacionados

Vuelve a la PR que Copilot generó para resolver la incidencia que pedía mostrar juegos relacionados en la página de detalles.

1. Abre el repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea similar a **Show related games on the game details page** u otro título descriptivo.
4. Selecciona **Files changed** para revisar el código generado.
5. Cuando hayas revisado los cambios y estés conforme, vuelve a **Conversation** y desplázate hacia abajo.
6. Deberías ver un indicador que avisa de que algunos flujos de trabajo están pendientes de aprobación.
7. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto para la única aplicación Astro se superarán. El proceso puede tardar varios minutos.
9. **Opcional:** puedes cambiar a esta rama en tu codespace para probar manualmente la funcionalidad de juegos relacionados. Abre el terminal y ejecuta estos comandos. Sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la funcionalidad de juegos relacionados. Como antes, puedes trabajar de forma iterativa con el agente en la nube para solicitar cambios. Por ejemplo, podrías pedirle que ajuste la cantidad de juegos relacionados o que añada encabezados de comentarios y comentarios TSDoc (recuerda que asignaste esta tarea **antes** de actualizar las instrucciones personalizadas). Para ello, añade un comentario nuevo en la pestaña **Conversation**. Copilot lo verá e iniciará otra sesión.

## Revisa las mejoras de accesibilidad

Por último, revisa las mejoras de accesibilidad implementadas con el agente personalizado de accesibilidad. Esta PR debería incluir el modo de alto contraste que asignaste en el Ejercicio 3 y el modo claro que solicitaste desde mission control en el Ejercicio 4.

1. Abre el repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea similar a **Add high contrast mode to website** u otro título descriptivo.

> [!NOTE]
> Si Copilot sigue trabajando, la solicitud tendrá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar varios minutos.

4. Selecciona **Files changed** para revisar el código generado.
5. Revisa la implementación y presta atención a:
   - los componentes de interfaz para alternar entre modos;
   - el uso del almacenamiento local para conservar las preferencias;
   - los cambios de CSS o estilo para los modos de alto contraste y claro;
   - los atributos de accesibilidad (etiquetas ARIA, navegación con teclado, etc.);
   - el código JavaScript o TypeScript que gestiona los cambios de modo.

6. Cuando hayas revisado los cambios y estés conforme, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que avisa de que algunos flujos de trabajo están pendientes de aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto para la única aplicación Astro se superarán. El proceso puede tardar varios minutos.
10. **Opcional:** puedes cambiar a esta rama en tu codespace para probar manualmente las mejoras de accesibilidad. Abre el terminal y ejecuta estos comandos. Sustituye `<branch-name>` por el nombre de la rama creada por Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba en el navegador los modos de alto contraste y claro. Comprueba que funcionan y que la preferencia se conserva al volver a cargar la página.

Observa cómo el agente personalizado de accesibilidad ayudó a Copilot a implementar estas mejoras siguiendo prácticas recomendadas de accesibilidad. Si encuentras algún problema o una posible mejora, puedes mencionarlo en un comentario a **@copilot**, como hiciste con las PR anteriores.

## Ejercicio opcional: sigue delegando

El agente en la nube funciona mejor cuando le asignas elementos reales de la lista de tareas y puedes ocuparte de otra cosa mientras trabaja. Para practicarlo, crea algunas incidencias más en el repositorio y asígnaselas a Copilot. Algunas ideas:

- Crear un formulario para que los patrocinadores muestren su interés en la página de detalles del juego.
- Implementar paginación en la página de la lista de juegos.
- Añadir validación de entradas y gestión de errores a las funciones de acceso a datos.

## Resumen

Has completado el itinerario del agente en la nube. En estas lecciones:

- **Has examinado los archivos de instrucciones personalizadas del repositorio** para observar su efecto en el trabajo del agente.
- **Has asignado incidencias al agente en la nube de Copilot** y has visto cómo prepara su entorno, planifica y trabaja de forma asíncrona.
- **Has creado y utilizado un agente personalizado** de accesibilidad para añadir los modos de alto contraste y claro.
- **Has utilizado la página de agentes de Copilot como mission control** para supervisar y orientar la sesión de accesibilidad.
- **Has revisado las solicitudes de incorporación de cambios del agente en la nube y has iterado sobre ellas**, mencionando a `@copilot` para solicitar cambios y aprobando flujos de trabajo.

## Repaso y pasos siguientes

Has completado el itinerario del agente en la nube. Si quieres seguir explorando, los otros itinerarios amplían lo que has practicado:

- 🖥️ **[Itinerario de VS Code](../../vscode/)**: explora el modo agente de Copilot Chat y la integración con MCP directamente desde el IDE.
- 💻 **[Itinerario de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades de agente, un agente personalizado de control de calidad, MCP de Playwright, Agent Merge y un puente opcional `/delegate` al agente en la nube.

En tu propio repositorio, prueba estas ideas:

- Asigna al agente en la nube una incidencia de refactorización o cobertura de pruebas.
- Crea un agente personalizado para otro ámbito.
- Configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube de Copilot][cloud-agent-best-practices]
- [Configurar el cortafuegos del agente en la nube de Copilot][agent-firewall]

---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
