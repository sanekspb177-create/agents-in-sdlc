---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando utilizas el modo agente de GitHub Copilot, trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como el trabajo se realiza en tu equipo y en tiempo real, Copilot Chat en VS Code muestra cada llamada a herramientas y cada edición a medida que se producen. Puedes revisar los diffs en línea, aceptar o rechazar cambios y orientar la conversación con prompts de seguimiento para ajustar o ampliar el trabajo sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad;
- revisarás y aceptarás las ediciones propuestas;
- orientarás al agente durante la sesión para añadir un selector de modo claro además del modo de alto contraste.

## Escenario

El agente personalizado de accesibilidad que has utilizado en el [Ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño solicita además un modo claro para quienes encuentran demasiado oscuro el tema predeterminado. En lugar de iniciar una conversación nueva, orientarás la sesión actual para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Las ediciones aparecen en el chat con un diff y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del Ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para observar cada llamada a herramientas. Verás lecturas de archivos, ediciones y los comandos de terminal que solicite ejecutar.
3. Cuando el agente proponga una edición, aparecerá un diff en el chat:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertirlo y pedir al agente que vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** en la parte superior del chat para ver todos los archivos que ha modificado durante la sesión.

> [!NOTE]
> Si una llamada a una herramienta necesita permiso (por ejemplo, para ejecutar un comando de terminal), el agente se detendrá y mostrará una solicitud **Allow**. Aprueba solo los comandos con los que estés conforme.

5. Espera a que el agente termine de implementar el modo de alto contraste. Debería mostrar un mensaje final que resuma los cambios.

## Orienta al agente para añadir el modo claro

El modo de alto contraste ya está implementado. Ahora ampliarás la conversación para añadir un selector de modo claro. Como continuarás en la misma sesión de chat, el agente conserva el contexto del trabajo anterior, incluidos los componentes modificados, el enfoque de estilo y el patrón de almacenamiento local.

1. Envía este prompt de seguimiento desde el cuadro de entrada de Copilot Chat:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente retomará el trabajo, examinará los cambios existentes y propondrá los cambios necesarios para añadir el modo claro.
3. Revisa cada nueva edición a medida que aparezca, como hiciste con el modo de alto contraste. Utiliza **Keep** o **Undo** en cada diff.
4. Si el agente sigue un enfoque que no quieres (por ejemplo, intenta reorganizar la interfaz de los selectores de una manera que no te gusta), detenlo con el botón **Stop** junto al cuadro de entrada y envía un mensaje de seguimiento para aclarar lo que quieres.

> [!TIP]
> Orientar una sesión en curso es más rápido que volver a empezar. Utiliza prompts de seguimiento para solicitar ajustes («simplifica el CSS y utiliza variables CSS en lugar de duplicar selectores»), corregir el rumbo («no añadas un tercer selector; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también métodos abreviados de teclado para los dos selectores»).

## Revisa el conjunto de cambios

Antes de confirmar el trabajo, revisa rápidamente todos los archivos que ha modificado el agente.

1. Abre la vista **Source Control** en VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en los componentes Astro, los estilos y las pruebas relacionadas.
3. Abre varios archivos modificados y examina los diffs. Confirma que aplican las prácticas de accesibilidad del agente personalizado: atributos ARIA, navegación con teclado, HTML semántico y almacenamiento local de las preferencias.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y pasos siguientes

Has utilizado el modo agente de Copilot Chat para supervisar y orientar una sesión en curso: has revisado cada edición, aceptado o rechazado cambios y enviado prompts de seguimiento para ampliar el trabajo sin perder el contexto.

En esta lección:

- has supervisado en tiempo real el trabajo del agente;
- has aceptado y rechazado ediciones de archivos;
- has orientado una sesión en curso con prompts de seguimiento.

En la [siguiente lección][next-lesson] publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás lo que ha generado el agente personalizado local durante el laboratorio.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
