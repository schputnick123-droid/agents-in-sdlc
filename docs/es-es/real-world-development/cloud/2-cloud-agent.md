---
title: "Ejercicio 2 - Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Muchas organizaciones tienen problemas con la deuda técnica: incidencias de seguridad pendientes, código heredado que necesita actualizarse o solicitudes de funcionalidad que llevan tiempo en el backlog. El agente de GitHub Copilot en la nube puede actualizar código y añadir funcionalidades de forma autónoma. Al terminar, crea una solicitud de incorporación de cambios (PR) en borrador para que la revise un desarrollador. Así, puede encargarse de tareas tediosas y ayudar a acelerar el desarrollo mientras el equipo se centra en el trabajo de mayor alcance.

Explorarás cómo usar el agente en la nube para:

- personalizar el entorno de generación de código.
- asegurar que las operaciones se realicen de forma segura.
- comprender la importancia de delimitar claramente las incidencias.
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica pendiente. El equipo que desarrolló la primera versión del sitio dejó la documentación en mal estado: prácticamente no existe. Como primer paso, el equipo quiere añadir comentarios TSDoc a las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar el descubrimiento de juegos. Quiere mostrar juegos relacionados —otros títulos de la misma categoría— en la página de detalles de cada juego, para que quienes visitan el sitio puedan seguir explorando. No necesita una implementación refinada todavía; busca una versión que permita realizar pruebas de aceptación de la experiencia. Este trabajo bloquea al equipo, pero ahora hay incidencias de mayor prioridad.

Son tareas que pueden quedar relegadas y que se pueden asignar al agente de Copilot en la nube. El agente trabaja de forma asíncrona; el desarrollador puede atender otras tareas y, después, revisar el trabajo para confirmar que cumple las expectativas.

## Presentación del agente de GitHub Copilot en la nube

El [agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede trabajar en segundo plano de forma parecida a un desarrollador. Por ejemplo, puedes [asignarle una incidencia de GitHub](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Una vez asignada, Copilot crea una PR en borrador para registrar el progreso, configura un entorno y empieza a trabajar. Puedes consultar la sesión mientras sigue activa o después de que termine. Cuando esté lista la solución propuesta, Copilot te mencionará en la PR.

## La importancia de delimitar bien las instrucciones

GitHub Copilot no hace magia: no elimina la necesidad de pensar. Incluso las tareas aparentemente sencillas tienen complejidades, así que [delimita con cuidado el trabajo que asignas al agente de Copilot en la nube][cloud-agent-best-practices]. Trátalo como a un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. La IA generativa no cambia los fundamentos del desarrollo de software.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores exploraste las instrucciones personalizadas y cómo orientan a Copilot. Si ya completaste el ejercicio de instrucciones, viste `.github/copilot-instructions.md` en acción; si no, puedes revisarlo ahora. Antes de asignar trabajo al agente, examina en modo de solo lectura los archivos de instrucciones que incluye el repositorio para identificar su efecto más adelante.

- `.github/copilot-instructions.md`: revisa **Code standards**, especialmente las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en el frontmatter `applyTo`, que aplica las instrucciones a archivos `**/*.test.ts`.

Cuando asignes al agente la incidencia `Code lacks documentation`, observa si la PR incluye comentarios TSDoc, convenciones de TypeScript y encabezados de archivo. Estas pautas proceden de los archivos de instrucciones y se volverán a mencionar al revisar las PR en un ejercicio posterior.

## Configura el entorno de desarrollo del agente en la nube

El desarrollo de código suele requerir un entorno específico y scripts de configuración. Lo mismo ocurre cuando se asignan tareas a Copilot, que trabaja de forma parecida a un ingeniero de software.

El agente utiliza [GitHub Actions][github-actions] como entorno. Puedes personalizarlo con un [flujo de trabajo de configuración][setup-workflow] en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que empiece el trabajo y permite instalar las herramientas y dependencias necesarias. El flujo ya está configurado en la plantilla del taller para proporcionar Node.js, dependencias del proyecto, binarios del navegador para las pruebas de extremo a extremo y la base de datos SQLite migrada y precargada de la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña Actions del repositorio.
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Configura solo los permisos mínimos necesarios para estos pasos. Copilot utilizará su propio token.
    permissions:
      # Para clonar el repositorio durante la configuración, por ejemplo, para instalar dependencias, hace falta el permiso contents: read. Si no se clona, Copilot lo hará automáticamente al terminar estos pasos.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de la aplicación: Node.js (la aplicación ahora utiliza Astro y Drizzle/libSQL).
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

      # Migra y precarga la base de datos SQLite local para que la compilación y las pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero tiene dos puntos importantes:

- Contiene un único trabajo llamado `copilot-setup-steps`, que se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la PR.
- El activador `workflow_dispatch` permite ejecutar manualmente el flujo de trabajo desde la pestaña **Actions**. Así puedes probar que funciona sin esperar a que Copilot lo ejecute.

## Añade documentación

La documentación suele estar desactualizada o faltar por completo, lo que dificulta el mantenimiento y la incorporación de nuevos desarrolladores. Copilot puede ayudar a crearla, por lo que esta es una buena tarea para asignarle en segundo plano. Más adelante revisarás el resultado.

1. Abre el repositorio en github.com en una pestaña nueva del navegador.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue**.
5. Escribe `Code lacks documentation` en el campo **Title**.
6. En **Description**, introduce:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. Selecciona **Create**.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

   ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Abre la pestaña **Pull Requests**.
11. Abre la PR recién generada, cuyo título será parecido a `[WIP]: Code lacks documentation`. Si todavía no aparece, espera un momento y vuelve a cargar la página.
12. Al cabo de unos minutos, Copilot debería crear una lista de tareas.

> [!NOTE]
> La lista puede tardar varios minutos en aparecer en la PR. Copilot está preparando el entorno mediante el flujo de trabajo de configuración, analizando el proyecto y determinando cómo resolver la tarea.

13. Revisa las tareas que va a completar.
14. Desplázate por la cronología de la PR para ver cuándo Copilot empieza a trabajar en la incidencia.
15. Selecciona **View session**.

   ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que volver a cargar la página para ver el indicador actualizado.

16. Observa la sesión en directo y cómo Copilot explora el código, analiza su estado, decide un plan y crea código.

El proceso puede durar varios minutos. Una ventaja importante del agente en la nube es que trabaja de forma asíncrona, de modo que puedes ocuparte de otras tareas. Aprovecha esa posibilidad y asígnale otra incidencia mientras trabajas en la siguiente actividad del sitio.

## Añade una sección de juegos relacionados

Una ventaja del agente de Copilot en la nube es que permite distribuir el trabajo: puedes ocuparte de una tarea mientras él trabaja en otra. Asigna al agente la sección de juegos relacionados que solicita el equipo de diseño.

1. Vuelve al repositorio en github.com.
2. Selecciona **Issues**.
3. Selecciona **New issue** y, después, **Blank issue**.
4. Escribe `Show related games on the game details page` en **Title**.
5. En **Description**, introduce:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

6. Selecciona **Create** para registrar la incidencia.
7. A la derecha, selecciona **Assign to Copilot**.

   ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

8. Selecciona **Assign**.

Poco después aparecerá un emoji 👀 en el primer comentario de la incidencia para indicar que Copilot está trabajando.

![Copilot utiliza el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot está trabajando en la segunda solicitud. Como trabaja de forma parecida a un ingeniero de software, no es necesario supervisarlo continuamente; revisa el resultado cuando termine. Mientras tanto, explora los agentes personalizados.

## Resumen y pasos siguientes

En esta lección has explorado el [agente de GitHub Copilot en la nube][copilot-agents]. Puedes asignarle incidencias para que las resuelva de forma asíncrona. Puede ayudar con deuda técnica, nuevas funcionalidades o migraciones de código entre frameworks.

Has explorado estos conceptos:

- personalizar el entorno de generación de código.
- asegurar que las operaciones se realicen de forma segura.
- delimitar claramente las incidencias.
- asignar incidencias a Copilot.

Mientras el agente trabaja, puedes crear y utilizar agentes personalizados. El [agente de Copilot en la nube también puede utilizar servidores MCP][cloud-agent-mcp] e instrucciones personalizadas, que ya exploraste en ejercicios anteriores.

## Recursos

- [Acerca del agente de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas para configurar el flujo de trabajo del agente de Copilot en la nube][cloud-agent-best-practices]

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
