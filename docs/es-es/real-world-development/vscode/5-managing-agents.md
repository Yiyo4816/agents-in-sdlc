---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando utilizas el modo agente de GitHub Copilot, trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como trabaja en tu equipo en tiempo real, Copilot Chat en VS Code muestra cada llamada a herramientas y cada edición. Puedes revisar las diferencias directamente en el chat, aceptar o rechazar cambios concretos y orientar la conversación con mensajes posteriores, sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás los cambios propuestos en los archivos.
- orientarás al agente durante la sesión para que añada un control para el modo claro junto al de alto contraste.

## Escenario

El agente personalizado de accesibilidad que utilizaste en el [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño solicita también un modo claro para quienes consideran que el tema predeterminado es demasiado oscuro. En vez de iniciar una conversación nueva, orientarás la sesión actual para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en línea con una diferencia y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver sus llamadas a herramientas, lecturas y ediciones de archivos y comandos del terminal.
3. Cuando proponga una edición, aparecerá una diferencia en línea:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para deshacerlo y permitir que el agente vuelva a intentarlo.
4. Abre el indicador **Files changed** en la parte superior del chat para ver todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a una herramienta requiere permiso, por ejemplo, para ejecutar un comando del terminal, el agente se detendrá y mostrará **Allow**. Aprueba únicamente los comandos que te parezcan adecuados.

5. Espera a que termine el modo de alto contraste. Copilot mostrará un mensaje final con un resumen de los cambios.

## Orienta al agente para añadir el modo claro

Ahora que ya está implementado el modo de alto contraste, amplía la misma conversación para añadir un control para el modo claro. Como continuarás en la misma sesión, el agente conservará el contexto de los componentes, los estilos y el uso del almacenamiento local.

1. Envía este mensaje de seguimiento en Copilot Chat:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. El agente revisará los cambios anteriores y propondrá las actualizaciones necesarias para el modo claro.
3. Revisa cada cambio nuevo y selecciona **Keep** o **Undo**, como hiciste con el modo de alto contraste.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, intenta reorganizar los controles de una forma que no te convence—, deténlo con el botón **Stop** junto al campo de mensaje y envía una aclaración.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de nuevo. Utiliza mensajes de seguimiento para solicitar ajustes («simplifica el CSS y utiliza variables en vez de duplicar selectores»), corregir el enfoque («no añadas un tercer control; integra el modo claro en el panel de configuración actual») o ampliar el alcance («añade también métodos abreviados de teclado para ambos controles»).

## Revisa los cambios propuestos

Antes de confirmar el trabajo, revisa los archivos modificados:

1. Abre la vista **Source Control** en VS Code.
2. Revisa la lista completa de archivos. Deberías ver cambios en componentes Astro, estilos y pruebas relacionadas.
3. Abre algunos archivos y revisa las diferencias. Confirma que se aplican las prácticas de accesibilidad del agente personalizado: atributos ARIA, navegación con teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y siguientes pasos

Has utilizado el modo agente de Copilot Chat para supervisar y orientar una sesión en curso: has revisado las ediciones, aceptado o rechazado cambios y enviado mensajes de seguimiento para ampliar el trabajo sin perder contexto.

En esta lección:

- has supervisado el trabajo del agente en tiempo real.
- has aceptado y rechazado ediciones de archivos.
- has orientado una sesión en curso con mensajes de seguimiento.

En la [siguiente lección][next-lesson] publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios realizados por el agente personalizado local durante el taller.

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
