---
title: "Ejercicio 2 - Añadir funcionalidades con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y tareas como ejecutar pruebas. Normalmente, el trabajo implica localizar todos los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo falta y volver a modificar el código. La lista continúa.

El modo agente de Copilot te ayuda con este proceso.

El modo agente de Copilot está diseñado para trabajar de forma más autónoma en el IDE. Se comporta de manera parecida a un desarrollador: primero explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Veamos cómo utilizarlo para añadir una funcionalidad al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en tu IDE y permite ciclos de respuesta rápidos e interacción directa. El agente en la nube trabaja de forma asíncrona, como un compañero del equipo, y se comunica contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás cómo:

- el modo agente de Copilot explora el proyecto, identifica los archivos relevantes y coordina cambios.
- el modo agente de GitHub Copilot implementa funcionalidades en la interfaz y la capa de datos.
- revisar los cambios y las pruebas generados por Copilot antes de combinar el código en el repositorio.

## Escenario

A medida que crece la lista de juegos, quieres permitir que los usuarios filtren por categoría y editor. En el ejercicio anterior creaste un asistente para editores. Ahora completarás con el modo agente el resto del trabajo de datos, interfaz y pruebas.

## Ejecuta el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio web para conocer sus funciones actuales.

Tailspin Toys es una plataforma de financiación colectiva para juegos de mesa con temática de desarrollo. Permite mostrar juegos y sus detalles. El sitio es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan una base de datos SQLite local mediante Drizzle ORM en su frontmatter; no hay una API de backend independiente ni un framework de interfaz en el cliente. Los asistentes reutilizables para acceder a los datos están en `src/lib/`, y la interactividad se implementa con pequeños elementos Astro `<script>` que utilizan las API estándar del DOM.

### Inicia el sitio

El comando `npm run dev` inicia el único servidor de desarrollo de Astro. Ejecútalo en el Codespace:

1. Vuelve a tu Codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Actualiza `main` y crea una rama para el filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor de Astro esté listo, aparecerá una dirección parecida a esta:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir `http://localhost:4321` en un navegador, ciérralo seleccionando **x**.

5. Para abrir el sitio, mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona `http://localhost:4321` en el terminal.

> [!NOTE]
> En Codespaces, al seleccionar el enlace localhost en el terminal, se te redirigirá automáticamente a una URL como `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado al servidor web que se ejecuta en tu Codespace.

### Explora el sitio

Las funciones principales son:

- **Home Page**: muestra juegos de mesa con sus títulos, imágenes y descripciones.
- **Game Details Page**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, el editor y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve al Codespace, abre Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso indicará las opciones necesarias.

La primera versión del sitio funciona, pero queremos añadir capacidades. Empieza por revisar el backlog: al crear el proyecto a partir de la plantilla se generaron incidencias automáticamente. Pídele a GitHub Copilot que te las muestre:

1. Selecciona **Agent** en el selector de agentes de Chat. El agente planifica e implementa cambios de forma autónoma en varios archivos, ejecuta comandos del terminal y utiliza herramientas.

   ![Captura del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no expresan preferencia por un modelo concreto. Durante su elaboración se utilizó Claude Sonnet 4.5 para que las sugerencias fueran relativamente coherentes y la experiencia resultara predecible. Sin embargo, los modelos de lenguaje son probabilísticos y es normal que las sugerencias difieran de los ejemplos.

> [!NOTE]
> Por su naturaleza probabilística, Copilot podría utilizar un comando MCP diferente, pero debería poder completar la tarea.

3. Envía este mensaje a Copilot:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para permitir que enumere las incidencias.
5. Revisa la lista generada.

Observa cómo Copilot también ha priorizado las incidencias según la utilidad que podrían tener para los usuarios.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto a Copilot. Utilizarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funciones al sitio.

1. En tu Codespace, abre `.github/instructions/ui.instructions.md`.
2. Revisa sus directrices para añadir funcionalidades, que incluyen:
   - una descripción de la arquitectura.
   - principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - enlaces a archivos específicos para Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden incluir referencias a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Esto facilita la reutilización, la división de instrucciones complejas en partes manejables y la referencia a ejemplos y plantillas.

## Implementa el filtrado

Para completar el filtrado hay que hacer al menos tres cambios en la aplicación:

- añadir o ajustar la lógica restante de filtrado en la capa de datos (`src/lib/`).
- añadir o actualizar las pruebas del comportamiento de filtrado.
- actualizar la página de la lista de juegos para incluir la interfaz de filtrado.

Además, hay que ejecutar las pruebas y confirmar que se superan antes de combinar los cambios. El modo agente de Copilot puede realizar estas tareas:

1. Continúa la conversación actual o selecciona **New Chat** para empezar una nueva.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Captura de cómo seleccionar el archivo de instrucciones de interfaz](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado.

   ![Captura del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado.
5. Pide a Copilot que implemente la funcionalidad a partir de la incidencia relacionada del backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los asistentes de la capa de datos, la interfaz y las pruebas. Después, modifica los archivos y ejecuta las pruebas.

   ![Captura de Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. El proceso puede tardar unos minutos según la complejidad de la tarea y del código. Mientras trabaja, quizá veas los botones **Keep** y **Undo** en el editor. Al terminar, podrás aceptar o deshacer todos los cambios; no tienes que seleccionar esos botones mientras el agente trabaja.

7. Cuando Copilot te lo pida, selecciona **Continue** para ejecutar las pruebas.

   ![Captura del diálogo de Copilot Chat que pide permiso para ejecutar las pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que haya pausas o que algunas pruebas fallen durante el proceso. Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y dejar de detectar errores.

   ![Captura de una sesión completada con el modo agente de Copilot](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado y busca posibles problemas.

> [!CAUTION]
> Revisa siempre el código generado por Copilot o por cualquier otra herramienta de IA.

10. Vuelve al navegador con el sitio en ejecución y explora la nueva funcionalidad.
11. Cuando hayas confirmado que funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y siguientes pasos

¡Enhorabuena! Has utilizado el modo agente de GitHub Copilot para añadir funcionalidades al sitio web de Tailspin Toys. Has aprendido cómo:

- el modo agente puede implementar funcionalidades en la interfaz y la capa de datos.
- el modo agente explora el proyecto, identifica los archivos relevantes y coordina cambios.
- revisar los cambios y las pruebas generados por el modo agente antes de combinar el código.

A continuación, [probarás la funcionalidad con el servidor Playwright MCP][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementar paginación

A medida que crece la lista de juegos, será necesario habilitar la paginación. Utiliza lo aprendido para pedir a Copilot que la implemente. Ten en cuenta lo siguiente:

- sigue las buenas prácticas existentes e incluye los archivos de instrucciones correspondientes.
- decide cómo quieres que funcione la paginación y si el usuario podrá elegir el tamaño de página.
- proporciona a Copilot las directrices necesarias en el mensaje.
- itera con Copilot y pídele cambios o añade contexto. Así suele trabajarse con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: diferencias y cuándo utilizarlos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
