---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

## Revisa el trabajo

A lo largo de este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le has pedido que documente el código, cree una sección de juegos relacionados y añada funciones de accesibilidad con modos de alto contraste y claro. Revisemos los cambios y, si hace falta, pidamos a Copilot que los mejore.

### Escenario

Los fundamentos del diseño de software y DevOps no cambian con la IA generativa. Siempre hay que revisar el código generado y seguir el proceso habitual. En este ejercicio revisarás las propuestas de Copilot para documentar el código, mostrar juegos relacionados y añadir funciones de accesibilidad antes de compartirlas con el equipo.

## Seguridad y agente de GitHub Copilot en la nube

Como el agente de Copilot trabaja de forma asíncrona y sin supervisión, se han establecido restricciones para mantener la seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utiliza para el código.
- El agente en la nube se ejecuta en GitHub Actions, dentro de un entorno independiente y efímero.
- Una persona debe aprobar los flujos de trabajo de GitHub Actions antes de que se ejecuten.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empieza por la primera solicitud de incorporación de cambios (PR) que generó el agente de Copilot: la que documenta el código. Utiliza la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Es posible que la PR muestre un aviso de que un firewall ha bloqueado a GitHub Copilot. Es **normal**: por defecto, Copilot tiene acceso limitado a recursos externos, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el firewall del agente en la nube][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de PR.
3. Abre la solicitud cuyo título sea parecido a **Add missing documentation**.

> [!NOTE]
> Si Copilot sigue trabajando, el título de la PR incluirá la etiqueta **[WIP]**. Espera a que termine; puede tardar varios minutos.

4. Cuando la PR esté lista, selecciona **Files changed** y revisa los cambios.

   ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado. Los cambios concretos pueden variar, pero deberían incluir comentarios TSDoc y otra documentación. Busca los comentarios TSDoc, las convenciones de TypeScript y los encabezados de comentarios: proceden de los archivos de instrucciones que revisaste al principio del ejercicio 2.
6. Cuando hayas revisado todo, vuelve a **Conversation** y desplázate hacia abajo.
7. Comprueba si hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Comprueba que se han puesto en cola las comprobaciones de la aplicación Astro. Si todo va bien, se superarán al cabo de unos minutos.

## Pide cambios a GitHub Copilot

La colaboración con Copilot en una PR no tiene por qué ser un proceso unidireccional. Puedes mencionarlo en comentarios de la PR o en comentarios asociados a líneas de código. Copilot verá el comentario e iniciará otra sesión para resolverlo. Como los resultados son probabilísticos, no hay una única petición adecuada. Por ejemplo, puedes pedirle que:

- añada al principio de cada archivo de código un encabezado con una descripción breve.
- añada comentarios TSDoc a los archivos TypeScript y Astro.
- cree un README que describa la estructura de la aplicación Astro.

1. Añade un comentario en la PR y menciona a **@copilot** para pedirle un cambio en la documentación. Puedes utilizar una de las ideas anteriores u otra petición relacionada con la documentación.
2. Selecciona **View Session** para observar cómo trabaja Copilot. Verás que inicia una sesión nueva para realizar los cambios.
3. Selecciona **Back to pull request** para volver a la PR.

   ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, aparecerá un commit nuevo en la PR.
5. Selecciona **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Después, marca la PR como lista para revisión y combínala.

![Marcar la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la funcionalidad de juegos relacionados

Vuelve a la PR que Copilot creó para mostrar juegos relacionados en la página de detalles:

1. Ve al repositorio en GitHub.com y selecciona **Pull Requests**.
2. Abre la PR titulada de forma parecida a **Show related games on the game details page**.
3. Selecciona **Files changed** y revisa el código generado.
4. Cuando termines la revisión, vuelve a **Conversation** y desplázate hacia abajo.
5. Si hay flujos de trabajo pendientes de aprobación, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

6. Comprueba que se han puesto en cola las comprobaciones del proyecto para la aplicación Astro. Puede que tarden varios minutos.
7. **Opcional:** cambia a esta rama en tu Codespace y prueba manualmente la funcionalidad. Sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot ha creado la funcionalidad de juegos relacionados. Puedes seguir iterando mediante nuevos comentarios en la pestaña **Conversation**. Por ejemplo, pide que cambie el número de juegos relacionados o que añada encabezados de comentarios y comentarios TSDoc. Recuerda que esta tarea se asignó antes de actualizar las instrucciones personalizadas.

## Revisa las funciones de accesibilidad

Por último, revisa las funciones implementadas con el agente personalizado de accesibilidad. Esta PR debería incluir el modo de alto contraste que pediste en el ejercicio 3 y el modo claro que solicitaste desde Mission Control en el ejercicio 4.

1. Ve al repositorio en GitHub.com y selecciona **Pull Requests**.
2. Abre la PR titulada de forma parecida a **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot todavía trabaja en la tarea, el título incluirá la etiqueta **[WIP]**. Espera a que termine; puede tardar unos minutos.

3. Selecciona **Files changed** y revisa el código.
4. Presta especial atención a:
   - los controles de la interfaz para cambiar de modo.
   - el uso del almacenamiento local para conservar las preferencias del usuario.
   - los cambios de CSS o de estilos para los modos de alto contraste y claro.
   - los atributos de accesibilidad, como las etiquetas ARIA y la navegación con teclado.
   - el código JavaScript/TypeScript que gestiona el cambio de modo.
5. Cuando termines la revisión, vuelve a **Conversation** y desplázate hacia abajo.
6. Si hay flujos de trabajo pendientes de aprobación, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

7. Confirma que las comprobaciones de la aplicación Astro se han puesto en cola y se completan correctamente.
8. **Opcional:** cambia a la rama de Copilot en tu Codespace y prueba manualmente las funciones de accesibilidad:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   Después, inicia la aplicación y prueba los controles de alto contraste y modo claro en el navegador. Confirma que funcionan y que las preferencias se conservan al volver a cargar la página.

Observa cómo el agente personalizado de accesibilidad ha orientado a Copilot para que aplique buenas prácticas. Si detectas problemas o mejoras posibles, menciona a **@copilot** en un comentario de la PR y solicita los cambios.

## Ejercicio opcional: sigue delegando

El agente en la nube resulta útil para delegar incidencias reales del backlog y ocuparte de otras tareas. Practica creando más incidencias y asignándolas a Copilot. Por ejemplo:

- crear un formulario de interés para patrocinadores en la página de detalles de un juego.
- implementar la paginación en la página de la lista de juegos.
- añadir validación de entradas y gestión de errores a las funciones de acceso a datos.

## Resumen

Has completado el itinerario del agente en la nube. En estas lecciones:

- **Has inspeccionado los archivos de instrucciones del repositorio** para observar su efecto en el trabajo del agente.
- **Has asignado incidencias al agente de Copilot en la nube** y has visto cómo prepara el entorno, planifica y trabaja de forma asíncrona.
- **Has creado y utilizado un agente personalizado** para añadir modos de alto contraste y claro.
- **Has utilizado la página de agentes de Copilot como centro de control** para supervisar y orientar una sesión de accesibilidad.
- **Has revisado las PR del agente e iterado sobre ellas**, mencionando a `@copilot` para solicitar cambios y aprobando los flujos de trabajo.

## Revisión y siguientes pasos

Has completado el itinerario del agente en la nube. Para seguir explorando, los otros itinerarios complementan lo que has practicado:

- 🖥️ **[Itinerario de VS Code](../../vscode/)**: explora el modo agente de Copilot Chat y la integración MCP desde el IDE.
- 💻 **[Itinerario de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con Plan y Autopilot, habilidades de agente, un agente QA, Playwright MCP, Agent Merge y la opción `/delegate` para volver a delegar en el agente de la nube.

También puedes probar estas ideas en tu propio repositorio:

- asignar al agente una incidencia de refactorización o cobertura de pruebas.
- crear un agente personalizado para otro ámbito.
- configurar `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el firewall del agente en la nube][agent-firewall]

---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
