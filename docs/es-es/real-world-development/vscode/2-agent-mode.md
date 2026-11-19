---
title: "Ejercicio 2 - Añadir funcionalidades con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y tareas como ejecutar pruebas. El flujo de trabajo de un desarrollador suele incluir localizar todos los archivos necesarios, aplicar los cambios, ejecutar las pruebas, depurar, averiguar qué archivo falta, volver a cambiarlo y repetir el proceso.

Para eso sirve el modo agente de Copilot.

El modo agente de Copilot está diseñado para trabajar con más autonomía en el IDE. Se comporta de forma parecida a un desarrollador: primero explora la estructura del proyecto, después realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Veamos cómo utilizarlo para añadir nuevas funcionalidades al sitio.

> [!NOTE]
> Aunque sus nombres se parecen, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en el IDE, con ciclos rápidos de comentarios e interacción. El agente en la nube está diseñado para trabajar de forma asíncrona como un miembro del equipo y comunicarse contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás a:

- utilizar el modo agente de Copilot para explorar el proyecto, identificar archivos pertinentes y coordinar cambios;
- implementar funcionalidades en la interfaz y la capa de datos con el modo agente de GitHub Copilot;
- revisar los cambios y las pruebas generados por el modo agente de Copilot antes de incorporarlos al código.

## Escenario

A medida que crece la lista de juegos, quieres permitir que los usuarios filtren por categoría y editor. En el ejercicio anterior ya has añadido una función auxiliar para obtener editores. Ahora completarás los cambios restantes en la capa de datos, la interfaz y las pruebas con el modo agente de Copilot.

## Ejecuta el sitio de Tailspin Toys

Antes de hacer cambios, exploremos el sitio de Tailspin Toys para conocer sus funciones actuales.

El sitio es una plataforma de financiación colectiva de juegos de mesa con temática de desarrollo. Permite mostrar listas de juegos y sus detalles. Es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM desde su frontmatter; no hay una API de backend independiente ni un framework de interfaz de cliente. Las funciones reutilizables de acceso a datos se encuentran en `src/lib/`, y la interactividad se implementa con scripts Astro pequeños y específicos que utilizan las API estándar del DOM.

### Inicia el sitio

Para facilitar la ejecución, se ha incluido la tarea `npm run dev`, que inicia el único servidor de desarrollo de Astro. Puedes ejecutarla en tu GitHub Codespace:

1. Vuelve al codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal en el Codespace con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Actualiza `main` y crea una rama para el filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio con este comando:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, aparecerá un mensaje con la URL, similar a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir un navegador en `http://localhost:4321`, ciérralo seleccionando **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En un codespace, al seleccionar el enlace localhost en el terminal de Codespaces se te redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado al codespace, donde ahora se ejecuta el servidor web.

### Explora el sitio

Con el sitio en ejecución, puedes explorar sus funciones principales:

- **Página de inicio**: muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, el editor y la categoría.

## Explora la lista de tareas pendientes con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando corresponda.

El sitio funciona, pero queremos añadir nuevas capacidades. Primero, revisemos la lista de tareas. Al crear el proyecto a partir de la plantilla se generaron automáticamente varias incidencias. Pide a GitHub Copilot que las muestre.

1. Selecciona **Agent** en el selector de agentes de la vista Chat. El agente puede planificar e implementar cambios en varios archivos, ejecutar comandos de terminal e invocar herramientas.

   ![Captura de pantalla del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no recomiendan un modelo concreto. Durante su elaboración se utilizó Claude Sonnet 4.5, por lo que se incluye en estas instrucciones para intentar ofrecer resultados relativamente coherentes y una buena experiencia. Como los LLM son probabilísticos, es normal que recibas sugerencias distintas a las que se muestran en el taller.

> [!NOTE]
> Debido a la naturaleza probabilística de los LLM, Copilot puede utilizar un comando MCP distinto, pero debería poder completar la tarea.

3. Pide a Copilot que muestre la lista de tareas pendientes con este prompt:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera todas las incidencias.
5. Revisa la lista generada.

Observa que Copilot también ha priorizado los elementos según cuáles podrían resultar más útiles para el usuario.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto para su trabajo. Utilizarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funcionalidades al sitio.

1. En el codespace, abre `.github/instructions/ui.instructions.md`.
2. Observa las directrices generales para añadir funcionalidades, entre ellas:
   - una descripción general de la arquitectura;
   - principios de diseño de componentes, facilidad para probarlos y accesibilidad;
   - enlaces a archivos de instrucciones específicos para Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden incluir referencias a otros archivos de instrucciones y a los archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Así se pueden reutilizar instrucciones, dividir conjuntos complejos en partes más manejables e incluir ejemplos y plantillas.

## Implementa el filtrado

Para completar el filtrado hay que hacer al menos tres cambios independientes en la aplicación:

- añadir o ajustar la lógica restante de filtrado en la capa de datos (`src/lib/`);
- añadir o actualizar las pruebas del comportamiento del filtrado;
- actualizar la página de lista de juegos para incluir la interfaz de filtrado.

Además, hay que ejecutar las pruebas y conseguir que terminen correctamente antes de incorporar los cambios. El modo agente de Copilot puede realizar estas tareas.

1. Continúa la conversación actual con Copilot o selecciona **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Ejemplo de selección del archivo de instrucciones de interfaz de usuario](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado en el selector de agentes de la vista Chat.

   ![Captura de pantalla del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad de acuerdo con la incidencia correspondiente:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos de la funcionalidad. Debería encontrar tanto las funciones auxiliares de la capa de datos como la interfaz y las pruebas. Después, empezará a modificar archivos y a ejecutar pruebas.

   ![Captura de pantalla de Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. Según la complejidad de la tarea y del código, puede tardar varios minutos. Mientras trabaja, puede que aparezcan los botones **Keep** y **Undo** en el editor. Al terminar, habrá un botón **Keep** o **Undo** para cada cambio; no hace falta que los selecciones mientras el agente todavía está trabajando.

7. Cuando Copilot te lo pida, selecciona **Continue** para ejecutar las pruebas.

   ![Cuadro de diálogo de Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que haya pausas o que fallen algunas pruebas durante el proceso. No pasa nada: Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y no detectar errores.

   ![Sesión completa de Copilot Chat en modo agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado y busca posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador con el sitio en ejecución y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y pasos siguientes

En este ejercicio has aprendido a utilizar el modo agente de GitHub Copilot para añadir funcionalidades al sitio de Tailspin Toys. Has visto cómo:

- el modo agente de GitHub Copilot puede implementar nuevas funcionalidades en la interfaz y la capa de datos;
- puede explorar el proyecto, identificar archivos pertinentes y coordinar cambios;
- conviene revisar los cambios y las pruebas generados antes de incorporarlos al código.

A continuación, [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio adicional: implementar paginación

A medida que crezca la lista de juegos, será necesario añadir paginación. Utiliza lo que has aprendido para pedir a Copilot que implemente la paginación. Al preparar la solicitud:

- sigue las prácticas recomendadas, incluidos los archivos de instrucciones existentes;
- decide cómo debería funcionar la paginación, por ejemplo, si el usuario podrá elegir el tamaño de página o si será fijo;
- proporciona a Copilot las instrucciones necesarias para obtener el resultado que buscas;
- itera con GitHub Copilot, solicitando cambios y proporcionando contexto cuando sea necesario. Es lo habitual al trabajar con Copilot.

## Recursos

- [Modos Ask, Edit y Agent de Copilot: qué hacen y cuándo utilizarlos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
