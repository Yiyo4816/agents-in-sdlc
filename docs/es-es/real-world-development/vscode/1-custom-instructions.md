---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo agente →][next-lesson]

El contexto es clave cuando trabajas con IA generativa. Si una tarea debe realizarse de una forma concreta —o Copilot debe conocer información de fondo—, asegúrate de proporcionarle ese contexto. Los [archivos de instrucciones][instruction-files] son una de las principales formas de ofrecer directrices para la generación de código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto del proyecto, las directrices de programación y los estándares de documentación mediante instrucciones personalizadas del repositorio y archivos específicos de rutas.
- generarás la primera parte de la capa de datos para el filtrado (un asistente para editores) con las instrucciones actuales.
- añadirás un estándar global al archivo `.github/copilot-instructions.md`.
- repetirás el mismo mensaje y observarás cómo el código generado sigue el nuevo estándar.
- confirmarás los cambios de instrucciones y de filtrado en `main` para que Copilot utilice las directrices actualizadas en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no seguir todos los estándares que definas. Copilot es probabilístico. El objetivo del ejercicio es observar la tendencia tras actualizar las instrucciones, no obtener siempre un resultado idéntico.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene directrices y requisitos para las prácticas de desarrollo:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz debe utilizar el modo oscuro y tener un aspecto moderno.
- Se debe documentar el código mediante comentarios TSDoc.
- Cada archivo debe empezar con un bloque de comentarios que describa su función.

Mediante los archivos de instrucciones, proporcionarás a Copilot el contexto necesario para que realice las tareas conforme a estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan a Copilot contexto y preferencias para que entienda mejor tu estilo y requisitos. Puedes especificar convenciones de programación, bibliotecas y los tipos de comentarios que quieres incluir. Puedes crear instrucciones para todo el repositorio o para ciertos tipos de archivos y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`: un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información global, como la pila tecnológica, una descripción del proyecto, buenas prácticas y otra información relevante para la mayoría de las solicitudes de chat o CLI.
- `.github/instructions/*.instructions.md`: archivos que proporcionan directrices para determinados lenguajes o tipos de archivo, o para tareas concretas.

> [!NOTE]
> En un IDE, los archivos de instrucciones solo se utilizan para generar código con Copilot Chat, no para completar código ni para sugerir las siguientes ediciones.
>
> Copilot Chat, Copilot CLI y el agente de Copilot en la nube utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` que tienen frontmatter `applyTo`.
>
> Copilot también admite archivos de instrucciones con otros formatos, como AGENTS.md y CLAUDE.md, tal como se explica en la [referencia de compatibilidad con instrucciones personalizadas][custom-instructions-support].

### Buenas prácticas para gestionar archivos de instrucciones

Los ejemplos del proyecto muestran un enfoque representativo. A grandes rasgos:

- Mantén `copilot-instructions.md` centrado en las directrices globales: qué se está desarrollando, cómo se estructura el proyecto y cuáles son los estándares de programación.
- Utiliza `*.instructions.md` para tipos de archivo (pruebas unitarias, componentes Astro, capa de datos) o tareas concretas.
- Escribe en lenguaje natural y claro. Incluye ejemplos de cómo debería ser el código y de cómo no debería ser.

No hay una única forma correcta de crear archivos de instrucciones ni de utilizar la IA. La experimentación te ayudará a encontrar el enfoque adecuado para tu proyecto.

> [!TIP]
> Todos los proyectos que utilizan GitHub Copilot deberían tener un conjunto sólido de archivos de instrucciones. En este proyecto encontrarás directrices para tareas como [cambios de interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta opción de forma distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. El taller del entorno que estés usando lo explicará cuando corresponda.
>
> Para encontrar plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], que reúne archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas del proyecto

Lee los archivos de instrucciones incluidos en el repositorio: hay un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la descripción del proyecto y las secciones **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. Dentro de **Code standards**, observa las directrices anidadas para **GitHub Actions Workflows**.
3. Explora la carpeta `.github/instructions`. Contiene directrices para archivos Astro, la capa de datos Drizzle, pruebas y otros ámbitos.
4. Abre `.github/instructions/unit-tests.instructions.md` y fíjate en el campo `applyTo` de la parte superior: define un patrón relativo a la raíz del repositorio que determina a qué archivos se aplican las instrucciones. Por ejemplo, coincide con los archivos de prueba TypeScript `**/*.test.ts`.
5. Observa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos extensos de instrucciones en archivos pequeños y reutilizables, y señalar a Copilot ejemplos que puede seguir. Las rutas son relativas al archivo de instrucciones.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero aún no exige documentación dentro del código. En los pasos siguientes añadirás directrices para los comentarios TSDoc y los encabezados de los archivos.

## Crea una rama para los cambios

Prepara una rama para trabajar:

1. Vuelve al Codespace del ejercicio anterior.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Utiliza Copilot Chat *antes* de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve al Codespace, abre Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso indicará las opciones necesarias.

Para observar el efecto de las instrucciones personalizadas, empieza enviando un mensaje con el archivo de instrucciones actual. Más adelante, lo actualizarás y enviarás de nuevo el mismo mensaje.

1. Cierra las pestañas abiertas de ejercicios anteriores para que Copilot solo tenga el contexto necesario.
2. Abre `src/lib/publishers.ts` para indicar a Copilot dónde debe ubicarse el asistente.
3. Selecciona **Agent** en el selector de agentes de Chat para que Copilot pueda editar archivos.

   ![Captura del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envía este mensaje:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explorará el proyecto y aplicará los cambios, normalmente en el archivo del asistente y sus pruebas.
6. Observa que el asistente generado es una función tipada que recibe un cliente `db` como primer argumento y devuelve una matriz tipada de editores. Esto sigue las convenciones de la capa de datos de `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observa que el código propuesto **no incluye** comentarios TSDoc ni un encabezado de comentario a nivel de archivo.

> [!CAUTION]
> Como Copilot es probabilístico, puede que añada comentarios de documentación aunque no se lo hayas pedido. No pasa nada: lo importante es observar si aumenta la coherencia al actualizar las instrucciones.

## Añade un estándar para todo el repositorio

El archivo `.github/copilot-instructions.md` proporciona información global a Copilot. Añade a los estándares de programación directrices para los comentarios de documentación:

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Localiza la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Observa que todavía no exige documentación dentro del código, por eso el asistente generado no tenía comentarios.
3. Añade estas líneas Markdown debajo de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Las instrucciones globales del repositorio son el lugar adecuado para incluir estándares que se aplican a todo el proyecto, como la directriz sobre comentarios de documentación. Los archivos `*.instructions.md` sirven para ámbitos más específicos.

## Repite el mensaje y observa el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para borrar el contexto anterior.
2. Selecciona de nuevo `src/lib/publishers.ts` para centrar a Copilot en el archivo adecuado.
3. Envía **el mismo mensaje**:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que el archivo propuesto ahora empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observa que la función propuesta ahora incluye un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Has guiado a Copilot para que siga un estándar nuevo y lo aplique a código real que utilizarás en el ejercicio siguiente.

## Confirma las instrucciones y publica la rama

Los archivos de instrucciones se gestionan mediante el control de código fuente, como cualquier otro archivo del repositorio. Confirma los cambios y publícalos:

1. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Confirma que están presentes las actualizaciones de instrucciones y del asistente:

   ```bash
   git status
   ```

3. Prepara y confirma las instrucciones y todos los archivos que Copilot modificó para crear el asistente. Si `git status` muestra cambios adicionales, como pruebas, inclúyelos también:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra otros cambios relacionados, añádelos también.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Publica la rama:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y combina una solicitud de incorporación de cambios

Con la rama publicada, crea una PR vinculada a la incidencia del backlog sobre la actualización de las instrucciones. Copilot puede localizar la incidencia, crear la PR y combinarla.

> [!NOTE]
> Esta es la primera vez que Copilot actúa en GitHub en este ejercicio. La plantilla del proyecto ya configura las herramientas de GitHub; la primera vez que Copilot las utiliza, VS Code podría pedirte que inicies sesión. Sigue las instrucciones para permitir el acceso. En una lección posterior aprenderás cómo funciona esta conexión (Model Context Protocol).

1. Abre Copilot Chat en tu Codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y confirma que **Agent** está seleccionado.
3. Pide a Copilot que busque la incidencia relacionada con los archivos de instrucciones y cree una PR desde la rama actual. Indícale que describa tanto las instrucciones actualizadas como el asistente de editores:

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot buscará la incidencia y creará la PR.
5. Cuando Copilot te pida permiso para realizar acciones en GitHub, revisa el comando y selecciona **Allow** si estás de acuerdo.
6. Cuando se cree la PR, pídele que la combine y vuelva a `main`:

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Cuando Copilot solicite permiso para realizar acciones de GitHub y ejecutar comandos de shell, revisa el comando y selecciona **Allow** si estás de acuerdo.

¡Has creado y combinado una solicitud de incorporación de cambios con ayuda de GitHub Copilot!

## Resumen y siguientes pasos

Has explorado cómo Copilot utiliza los archivos de instrucciones del proyecto y, después, has utilizado Copilot Chat en VS Code para:

- enviar un mensaje de generación de código y observar el resultado con las instrucciones existentes.
- añadir un estándar global a `.github/copilot-instructions.md`.
- repetir el mismo mensaje y observar cómo el código propuesto sigue el estándar nuevo.
- confirmar las instrucciones y el asistente en `main` para que el siguiente ejercicio pueda utilizarlo.

A continuación, aplicarás esas instrucciones en [modo agente][next-lesson] mientras Copilot añade una funcionalidad al proyecto.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Buenas prácticas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para redactar mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personalizadas personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
