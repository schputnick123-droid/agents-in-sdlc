---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has asignado al agente de Copilot en la nube tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidades. El agente trabaja de forma autónoma y asíncrona, pero sigue siendo importante supervisar las tareas.

Hay varias herramientas para gestionar las tareas asignadas, como la [página de agentes][agents-page] de GitHub.com. Desde este centro puedes ver las tareas de agentes que tienen solicitudes de incorporación de cambios abiertas, consultar las operaciones realizadas y orientar las sesiones que siguen en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- orientarás una sesión activa para solicitar una funcionalidad adicional.

## Escenario

Después de asignar al agente la implementación de un modo de alto contraste, el equipo decide añadir también un modo claro. Como ya se están modificando los estilos del sitio y añadiendo controles de tema, parece lógico incluir esa funcionalidad. Orientarás al agente para que añada el modo claro además del modo de alto contraste.

## Revisa las tareas del agente en la nube

Consulta el estado de todas las tareas asignadas al agente de Copilot en la nube.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa las tareas en el panel principal y en el panel izquierdo. Deberías encontrar las que has asignado, entre ellas:
   - actualizar la documentación del código base.
   - generar API para modificar juegos.
   - añadir un modo de alto contraste al sitio.
3. Selecciona una tarea en curso y revisa las operaciones realizadas por Copilot, que pueden incluir:
   - obtener el código del repositorio.
   - preparar el entorno de trabajo.
   - configurar servidores MCP.
   - realizar distintas tareas para completar el trabajo.

> [!NOTE]
> Los pasos concretos dependerán del estado del trabajo de Copilot y del enfoque que haya utilizado.

4. Observa el panel de solicitudes de incorporación de cambios (PR), que aparece a la derecha y permite consultar la PR y los archivos modificados.

## Orienta al agente en la nube

Una vez que hayas visto las tareas activas, pide a Copilot que añada el modo claro mientras implementa el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre elegido por Copilot y del estado del trabajo.

   ![Sesión de accesibilidad en mission control](../../../_images/ex6-accessibility-session.png)

2. Espera unos minutos hasta que termine la configuración y empiece el trabajo. Lo sabrás cuando aparezcan mensajes parecidos a los siguientes.
3. En el cuadro **Steer active session while Copilot is working**, escribe este prompt:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Tarea del agente en la nube y cuadro para orientar la sesión activa](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot confirma la petición y la incorpora al flujo de trabajo.

## Deja que Copilot trabaje

Copilot incorporará la nueva solicitud al flujo cuando termine el paso que estaba realizando al recibir el mensaje.

El trabajo tardará varios minutos. Puedes aprovechar el tiempo para repasar lo aprendido hasta ahora.

## Resumen y pasos siguientes

Has explorado la página de agentes de Copilot, el centro para supervisar y orientar las tareas del agente en la nube. Permite seguir las tareas activas y completadas, revisar el trabajo y modificar las instrucciones de una sesión en curso.

En esta lección:

- utilizaste mission control y la página de agentes para supervisar las tareas.
- orientaste una sesión activa para solicitar una funcionalidad adicional.

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
