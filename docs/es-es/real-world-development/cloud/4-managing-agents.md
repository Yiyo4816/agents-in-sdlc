---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores pediste al agente de GitHub Copilot en la nube que realizara tres tareas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente trabaja de forma autónoma y asíncrona, es importante poder supervisar esas tareas.

Puedes gestionar las tareas asignadas al agente mediante distintas herramientas, como la [página de agentes][agents-page] de GitHub.com. Desde este panel de control puedes ver las tareas de los agentes que tienen solicitudes de incorporación de cambios abiertas, consultar las operaciones realizadas e incluso orientar una sesión en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- orientarás una sesión en curso para solicitar una funcionalidad adicional.

## Escenario

Después de asignar al agente la tarea de crear un modo de alto contraste, el equipo decidió que sería un buen momento para añadir también un modo claro. Como ya se están actualizando los estilos y añadiendo controles de alternancia, es lógico incorporar esta función. Orientarás el trabajo del agente para que añada ambos modos.

## Revisa las tareas del agente en la nube

Consulta el estado de las tareas asignadas al agente de GitHub Copilot en la nube:

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa las tareas en el panel principal y en el panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación del código.
   - generar API para modificar juegos.
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una tarea en curso y revisa las operaciones realizadas por Copilot. Por ejemplo:
   - descargar el código del repositorio.
   - preparar el entorno para Copilot.
   - configurar servidores MCP.
   - realizar distintas tareas para completar el trabajo.

> [!NOTE]
> Los pasos concretos varían según el estado del trabajo de Copilot y el enfoque que haya utilizado.

4. Observa también el panel de solicitudes de incorporación de cambios que aparece a la derecha. Desde allí puedes ver la PR y los archivos modificados.

## Orienta al agente en la nube

Ahora que has visto las tareas activas, pídele a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que haya elegido Copilot y del estado del trabajo.

   ![Sesión de accesibilidad en Mission Control](../../../_images/ex6-accessibility-session.png)

2. Espera unos minutos hasta que la sesión indique que se ha completado la configuración y ha empezado el trabajo.
3. En el diálogo **Steer active session while Copilot is working**, escribe este mensaje:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Captura de la tarea del agente en la nube con el diálogo para orientar la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el mensaje.
5. Observa cómo Copilot confirma la solicitud y la incorpora al flujo de trabajo.

## Deja que Copilot trabaje

Copilot continuará con la tarea actualizada. Incorporará la nueva solicitud al terminar el paso que estaba ejecutando cuando enviaste el mensaje.

El proceso tardará varios minutos. Puedes aprovechar el tiempo para repasar lo que has aprendido y explorado hasta ahora.

## Resumen y siguientes pasos

Has explorado la página de agentes de Copilot, el centro de control para supervisar y orientar las tareas del agente en la nube. Puedes realizar el seguimiento de las tareas activas y completadas, revisar el trabajo y redirigir las sesiones en curso para ajustar su alcance o añadir indicaciones.

En esta lección:

- has explorado Mission Control y la página de agentes para supervisar las tareas del agente en la nube.
- has redirigido una sesión en curso para solicitar una funcionalidad adicional.

Mientras Copilot completa las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
