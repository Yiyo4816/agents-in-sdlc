---
title: "Ejercicio 2 - Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que casi todas las organizaciones tengan deuda técnica: incidencias de seguridad pendientes, código antiguo que se debe actualizar o funcionalidades que llevan tiempo en el backlog porque no ha habido tiempo para implementarlas. El agente de GitHub Copilot en la nube puede actualizar código y añadir funcionalidades de forma autónoma. Al terminar, abre un borrador de solicitud de incorporación de cambios (PR) para que un desarrollador lo revise. Así puedes delegar tareas tediosas, acelerar el desarrollo y dedicarte a trabajos más amplios.

En este ejercicio explorarás cómo:

- personalizar el entorno de generación de código.
- asegurarte de que las operaciones se realizan de forma segura.
- definir incidencias con un alcance claro.
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica pendiente. Los contratistas que crearon la primera versión del sitio dejaron la documentación en mal estado: en realidad, no hay documentación. El primer paso consiste en añadir comentarios TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar el descubrimiento de juegos. Quiere mostrar juegos relacionados —otros títulos de la misma categoría— en la página de detalles de cada juego para poder probar la experiencia de usuario. No necesita una implementación refinada todavía; solo algo con lo que realizar pruebas de aceptación. Aunque esta tarea está bloqueada, hay incidencias de mayor prioridad.

Estos son ejemplos de tareas que pueden quedar relegadas y que conviene asignar al agente de Copilot en la nube. El agente trabaja en ellas de forma asíncrona mientras tú te ocupas de otras tareas. Después puedes volver para revisar su trabajo y confirmar que cumple los requisitos.

## Presentación del agente de GitHub Copilot en la nube

El [agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma parecida a un desarrollador. Puedes, por ejemplo, [asignarle una incidencia de GitHub](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Al recibirla, Copilot crea una PR en borrador para registrar el progreso, prepara un entorno y empieza a trabajar. Puedes consultar la sesión mientras está en curso o una vez completada. Cuando la solución esté lista para revisar, Copilot te mencionará en la PR.

## La importancia de acotar bien las instrucciones

GitHub Copilot no hace magia: no puedes saltarte el análisis. Incluso las tareas que parecen sencillas pueden ser complejas al examinarlas en detalle. Por eso, [define con cuidado el alcance de las tareas para el agente en la nube][cloud-agent-best-practices]. Trátalo como a un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. Incorporar IA generativa no cambia los fundamentos del desarrollo de software.

## Instrucciones personalizadas del repositorio

En ejercicios anteriores has visto cómo las instrucciones personalizadas orientan a Copilot. Si ya completaste un ejercicio práctico sobre instrucciones personalizadas, habrás visto cómo se utiliza `.github/copilot-instructions.md`; si no, aprovecha para explorarlo. Antes de asignar trabajo al agente, revisa los archivos de instrucciones del repositorio para observar su efecto más adelante.

Abre estos archivos en la interfaz web de GitHub o en un Codespace:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, especialmente las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en el frontmatter `applyTo`, que aplica estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes al agente la incidencia `Code lacks documentation`, observa en la PR los comentarios TSDoc, las convenciones de TypeScript y los encabezados de comentarios. Estas directrices proceden de los archivos de instrucciones y volverán a aparecer al revisar las PR en un ejercicio posterior.

## Configura el entorno de desarrollo del agente en la nube

Para crear código suele ser necesario preparar un entorno y ejecutar scripts de configuración. Lo mismo ocurre al asignar tareas a Copilot, que trabaja de forma parecida a un ingeniero de software.

El agente utiliza [GitHub Actions][github-actions] como entorno de trabajo. Puedes personalizarlo con un [flujo de configuración especial][setup-workflow], definido en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que el agente empiece. Así puedes instalar las herramientas y dependencias necesarias. El repositorio ya incluye una configuración para este taller: instala Node.js y las dependencias del proyecto, los navegadores para las pruebas de extremo a extremo y prepara la base de datos SQLite migrada y con datos para la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio.
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Concede solo los permisos mínimos necesarios. Copilot utilizará un token propio para sus operaciones.
    permissions:
      # Para clonar el repositorio en estos pasos, por ejemplo, para instalar dependencias, necesitas el permiso `contents: read`. Si no lo clonas aquí, Copilot lo hará automáticamente cuando terminen los pasos.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de la aplicación y el frontend: Node.js (la aplicación es Astro con Drizzle/libSQL).
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

      # Migra y carga la base de datos SQLite local para que las compilaciones y pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier flujo de trabajo de GitHub, pero tiene algunos aspectos importantes:

- Contiene un único trabajo llamado `copilot-setup-steps`. GitHub Actions lo ejecuta antes de que Copilot empiece a trabajar en la PR.
- El activador `workflow_dispatch` permite ejecutar manualmente el flujo desde la pestaña **Actions**. Así puedes comprobar que funciona sin esperar a que Copilot lo ejecute.

## Añade documentación

Aunque todo el mundo reconoce la importancia de la documentación, muchos proyectos tienen información obsoleta o carecen de ella. Esa deuda técnica reduce la productividad y dificulta mantener el código o incorporar a nuevos desarrolladores. Copilot puede ayudar a crear documentación; asígnale esta incidencia y volverás más adelante para revisar el resultado.

1. En una nueva pestaña del navegador, ve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el diálogo de incidencias.
4. Selecciona **Blank issue**.
5. Escribe `Code lacks documentation` en **Title**.
6. Escribe lo siguiente en **Description**:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el diálogo de asignación.

   ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
   
   ![Detalles de la asignación de Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecciona la pestaña **Pull Requests**.
11. Abre la nueva PR, cuyo título será parecido a `[WIP]: Code lacks documentation`. Si todavía no aparece, espera un momento y actualiza la página.
12. Al cabo de unos minutos, Copilot debería crear una lista de tareas.

> [!NOTE]
> La lista puede tardar varios minutos en aparecer. Copilot está preparando su entorno —ejecuta el flujo de trabajo descrito anteriormente—, analizando el proyecto y determinando cómo resolver la tarea.

13. Revisa la lista y las tareas previstas.
14. Desplázate por la cronología de la PR para ver cuándo Copilot empieza a trabajar en la incidencia.
15. Selecciona **View session**.

   ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la página para ver el indicador actualizado.

16. Observa la sesión en directo: Copilot explora el código y comprende el estado del proyecto, se detiene a pensar y decidir el plan y, después, crea código.

El proceso tardará varios minutos. Una de las ventajas principales del agente en la nube es que trabaja de forma asíncrona y te permite ocuparte de otras tareas. Aprovecha esa función asignándole otra tarea mientras trabajas en la aplicación.

## Añade una sección de juegos relacionados

Una ventaja del agente de Copilot en la nube es que puede trabajar en paralelo mientras tú te ocupas de otras tareas. Aunque añadir una sección de juegos relacionados quizá no lleve mucho tiempo, puedes aprovechar ese tiempo para avanzar en otro trabajo. Asigna esta tarea al agente:

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** y, después, **Blank issue**.
4. Escribe `Show related games on the game details page` en **Title**.
5. Escribe lo siguiente en **Description**:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

6. Selecciona **Create** para crear la incidencia.
7. En el lado derecho, selecciona **Assign to Copilot** y, después, **Assign**.

Poco después verás el emoji 👀 en el primer comentario de la incidencia: indica que Copilot ya está trabajando en ella.

![Copilot utiliza el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en tu segunda solicitud. Como trabaja de manera parecida a un ingeniero de software, no es necesario supervisarlo continuamente; revisa el resultado cuando haya terminado. Mientras tanto, puedes crear y utilizar agentes personalizados.

## Resumen y siguientes pasos

En esta lección has explorado el [agente de GitHub Copilot en la nube][copilot-agents]. Puedes asignarle incidencias para que trabaje en ellas de forma asíncrona y ayude a resolver deuda técnica, crear funcionalidades o migrar código entre frameworks.

Has explorado estos conceptos:

- personalizar el entorno de generación de código.
- asegurarte de que las operaciones se realizan de forma segura.
- definir incidencias con un alcance claro.
- asignar incidencias a Copilot.

Mientras el agente trabaja en segundo plano, puedes crear y utilizar agentes personalizados. El [agente de Copilot en la nube también puede utilizar servidores MCP][cloud-agent-mcp] y acceder a las instrucciones personalizadas que has explorado en módulos anteriores.

## Recursos

- [Acerca del agente de Copilot en la nube][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

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
