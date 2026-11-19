---
title: "Lección 8 - Crear y combinar la PR de la funcionalidad"
description: "Revisa conjuntamente el filtrado, las instrucciones, la actualización de la habilidad, el perfil QA y las pruebas; después, autoriza Agent Merge y confirma que la PR se ha combinado."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La implementación del filtrado, las actualizaciones de instrucciones y de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Es hora de revisarlos en conjunto y abrir una solicitud de incorporación de cambios. Ya has combinado personalmente la PR de valoraciones por estrellas; esta vez permitirás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las actualizaciones de instrucciones y de la habilidad y el agente QA en varias PR. Para agilizar el taller, has mantenido todo el flujo de filtrado y calidad en una sesión y una rama, y todo ese trabajo se incluirá en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de combinación;
- inspeccionarás todos los cambios de la funcionalidad y las pruebas de verificación;
- crearás la PR del filtrado;
- activarás Agent Merge solo después de revisarla y confirmarás que se ha combinado.

## Escenario

A lo largo del flujo de trabajo de filtrado, has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere ahora automatizar el trabajo restante de la PR y mantener la autorización para combinar bajo el control del desarrollador.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios en la aplicación GitHub Copilot. Al activarlo, la sesión de la aplicación lee la solicitud y resuelve los bloqueos: corrige las comprobaciones de integración continua (CI) que fallan, responde a comentarios de revisión y reorganiza la rama cuando es necesario. Después, la combina en cuanto GitHub lo permite. Se ejecuta en segundo plano, sigue funcionando tras reiniciar la aplicación y se desactiva cuando se combina la solicitud.

Hasta ahora has seleccionado **Merge pull request** manualmente. Agent Merge puede encargarse de esta tarea, pero todavía necesita tu autorización explícita para editar código y combinar. Revisa las acciones permitidas y el trabajo antes de concederle permiso para combinar.

## Utiliza Agent Merge para gestionar la PR

Ya has creado y revisado el código. Ahora dejarás que Agent Merge gestione el proceso de la PR.

1. Utiliza el selector de agentes para seleccionar **Default agent**.
2. Selecciona el menú desplegable junto a **Create PR**.
3. Selecciona **Agent Merge**. El botón cambiará a **Agent Merge**.
4. Selecciona el botón **Agent Merge** para iniciar el proceso. Elegir la opción del menú desplegable no lo inicia por sí solo.

Se inicia el proceso de Agent Merge:

- Crea la solicitud de incorporación de cambios con un título y una descripción.
- Si has iniciado la sesión desde una incidencia, incluye una referencia a ella en el cuerpo de la descripción.
- Reorganiza la rama o gestiona los posibles conflictos con la rama de destino.
- Supervisa CI para confirmar que se superen todas las comprobaciones.
- Supervisa la PR por si otros desarrolladores o la revisión de código de Copilot dejan comentarios, y realiza los cambios necesarios para resolverlos.
- Puede combinar automáticamente la PR cuando todo termine correctamente.

Vamos a permitir que Agent Merge combine también la PR cuando terminen correctamente todas las comprobaciones.

5. Revisa el diff de la PR y las pruebas de verificación antes de conceder permiso para combinar.
6. Selecciona el menú desplegable junto a **Agent Merge**.
7. Confirma que la opción **Merge pull request** está marcada.
8. Pide explícitamente al agente que combine la PR:

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. Supervisa el progreso y resuelve cualquier bloqueo que se indique. Antes de continuar, confirma que el estado de la PR es **Merged**. Que el permiso para combinar esté habilitado o que haya un agente trabajando no confirma que la combinación haya terminado.

> [!IMPORTANT]
> Agent Merge no elude las protecciones del repositorio ni los permisos que falten. Resuelve esos bloqueos antes de continuar.

## Resumen y pasos siguientes

Has automatizado varias partes del proceso de desarrollo: la generación de código, las pruebas y la validación y, ahora, también el proceso de la solicitud de incorporación de cambios. En esta lección:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de combinación;
- has inspeccionado todos los cambios de la funcionalidad y las pruebas de verificación;
- has autorizado Agent Merge solo después de la revisión y confirmado que la PR se ha combinado.

A continuación, [utilizarás un lienzo existente y crearás uno de clasificación][next-lesson] para explorar una forma más completa de examinar, planificar y visualizar el trabajo con el agente.

## Recursos

- [Gestionar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
