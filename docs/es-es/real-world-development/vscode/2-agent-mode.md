---
title: "Ejercicio 2 - Añadir funcionalidad con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso los cambios más sencillos de una aplicación suelen afectar a varios archivos y requieren operaciones como ejecutar pruebas. El flujo de trabajo habitual de un desarrollador consiste en localizar los archivos, hacer los cambios, ejecutar pruebas, depurar, averiguar qué archivo falta y volver a modificarlo. La lista continúa.

El modo agente de Copilot puede ayudar con ese trabajo.

El modo agente está diseñado para trabajar con más autonomía en el IDE. Se comporta de forma parecida a un desarrollador: primero explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. En este ejercicio descubrirás cómo añadir funcionalidades al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en el IDE y permite iterar rápidamente e interactuar con él. El agente en la nube trabaja de forma asíncrona como otro miembro del equipo, e interactúa contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás:

- cómo el modo agente de Copilot explora el proyecto, identifica archivos y coordina cambios.
- cómo implementar funcionalidades en la interfaz y la capa de datos.
- cómo revisar los cambios y las pruebas antes de combinar el código.

## Escenario

A medida que crece la lista de juegos, quieres que se puedan filtrar por categoría y editorial. En el ejercicio anterior añadiste una función auxiliar para obtener editoriales. Ahora completarás el trabajo pendiente de la capa de datos, la interfaz y las pruebas con el modo agente.

## Ejecuta el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio para entender su funcionamiento actual.

Tailspin Toys es una plataforma de financiación colectiva de juegos de mesa con temática para desarrolladores. Permite consultar juegos y ver sus detalles. El sitio es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM en su frontmatter; no hay una API de backend independiente ni un framework de interfaz del lado del cliente. Las funciones auxiliares reutilizables de acceso a datos se encuentran en `src/lib/`, y la interactividad se implementa con pequeños elementos `<script>` de Astro que usan las API DOM estándar.

### Inicia el sitio

El comando `npm run dev` inicia el único servidor de desarrollo de Astro.

1. Vuelve a tu codespace. Tras el ejercicio anterior, deberías estar en `main`.
2. Abre un terminal en el codespace con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea y cambia a una rama nueva para este trabajo:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, aparecerá un mensaje con la URL, parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir `http://localhost:4321` en un navegador, ciérralo seleccionando **x**.

5. Mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona `http://localhost:4321` en el terminal para abrir el sitio.

> [!NOTE]
> En un codespace, al seleccionar el enlace localhost en el terminal se abrirá `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado al servidor web que se ejecuta en el codespace.

### Explora el sitio web

Las principales funciones del sitio son:

- **Página de inicio**: muestra juegos de mesa, sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como su título, descripción, editorial y categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando sea necesario.

La versión inicial del sitio funciona, pero queremos añadir capacidades. Empieza por revisar el backlog. Al crear el proyecto a partir de la plantilla se generó un conjunto de incidencias; pide a Copilot que lo muestre.

1. En el menú desplegable de agentes de Chat, selecciona **Agent**. El modo agente planifica e implementa cambios en varios archivos, ejecuta comandos de terminal y usa herramientas.

   ![Captura del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores no recomiendan un modelo por encima de otro. Se utilizó Claude Sonnet 4.5 al crear el taller, por lo que se incluye en estas instrucciones para intentar ofrecer una experiencia coherente. Sin embargo, las sugerencias pueden variar porque los modelos de lenguaje no son deterministas. Es normal.

> [!NOTE]
> Debido al carácter probabilístico de los modelos, Copilot podría utilizar un comando MCP diferente, pero debería poder completar la tarea.

3. Envía este prompt para consultar el backlog:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para permitir que se enumeren las incidencias.
5. Revisa la lista generada.

Copilot también habrá priorizado los elementos según cuáles considera más útiles para los usuarios.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto. Usarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funciones al sitio.

1. En el codespace, abre `.github/instructions/ui.instructions.md`.
2. Observa las pautas generales para añadir funciones, que incluyen:
   - una descripción general de la arquitectura.
   - principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - enlaces a instrucciones específicas para distintos tipos de archivo, como Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Así se pueden reutilizar pautas, dividir instrucciones complejas en archivos más pequeños y ofrecer ejemplos y plantillas.

## Implementa el filtrado

Para completar el filtrado hacen falta, como mínimo, tres cambios:

- añadir o ajustar la lógica restante de filtrado en la capa de datos (`src/lib/`).
- añadir o actualizar las pruebas del filtrado.
- actualizar la página de listado de juegos para incluir la interfaz de filtrado.

Las pruebas deben ejecutarse y superarse antes de combinar los cambios. El modo agente puede realizar estas tareas.

1. Continúa en la conversación actual o selecciona **New Chat** para empezar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** para añadir el archivo de instrucciones.

   ![Captura que muestra cómo seleccionar el archivo de instrucciones de UI](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado.
4. Confirma que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Envía este prompt para implementar la funcionalidad relacionada con la incidencia del backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos pertinentes en la capa de datos, la interfaz y las pruebas. Después modifica los archivos y ejecuta las pruebas.

   ![Captura que muestra a Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot explorará el proyecto, modificará archivos y ejecutará pruebas. El proceso puede tardar varios minutos. Mientras trabaja, quizá aparezcan los botones **Keep** y **Undo** en el editor. No es necesario seleccionarlos durante el trabajo; cuando Copilot termine, podrás aceptar o deshacer todos los cambios propuestos.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Cuadro de diálogo de Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que se produzcan pausas o que fallen algunas pruebas. Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea sin detectar errores.
9. Inspecciona el código generado para detectar posibles problemas.

> [!CAUTION]
> Revisa siempre el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador con el sitio y explora la nueva funcionalidad.
11. Cuando hayas confirmado que funciona y revisado el código, selecciona **Keep** en Copilot Chat.

## Resumen y pasos siguientes

Has utilizado el modo agente de GitHub Copilot para añadir capacidades al sitio de Tailspin Toys. Has aprendido:

- cómo implementar funciones en la interfaz y la capa de datos.
- cómo explorar el proyecto, identificar archivos y coordinar cambios.
- cómo revisar los cambios y las pruebas antes de combinar el código.

A continuación, [probarás la funcionalidad con el servidor Playwright MCP][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio adicional: implementar paginación

A medida que crezca la lista de juegos, será necesario paginarla. Utiliza lo aprendido para pedir a Copilot que implemente la paginación. Sigue las prácticas recomendadas y los archivos de instrucciones existentes, decide si quieres que el usuario pueda elegir el tamaño de página y proporciona a Copilot indicaciones suficientes. Es posible que debas iterar y aportar más contexto; así se trabaja normalmente con Copilot.

## Recursos

- [Modos Ask, Edit y Agent de Copilot: qué hacen y cuándo utilizarlos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
