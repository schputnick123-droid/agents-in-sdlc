---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando se utiliza el modo agente de GitHub Copilot, este trabaja con autonomía: explora el código, propone cambios, edita archivos y ejecuta comandos. Como trabaja en tiempo real en el equipo, Copilot Chat en VS Code muestra las llamadas a herramientas y las ediciones de archivos. Puedes revisar cada diferencia en línea, aceptar o rechazar cambios concretos y orientar la conversación con prompts adicionales sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás los cambios propuestos.
- pedirás al agente que añada un control para el modo claro junto al de alto contraste.

## Escenario

El agente personalizado de accesibilidad del [Ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño solicita añadir también un modo claro para quienes consideran demasiado oscuro el tema predeterminado. En vez de iniciar una conversación nueva, ampliarás la sesión existente.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada modificación aparece en el chat con diferencias y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del Ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver cada llamada a herramienta, lectura y edición de archivos, así como los comandos de terminal que solicita ejecutar.
3. Cuando proponga una edición, aparecerá con sus diferencias:
   - selecciona **Keep** para aceptar el cambio.
   - selecciona **Undo** para deshacerlo y permitir que el agente lo intente de nuevo.
4. También puedes abrir el indicador **Files changed** en la parte superior del chat para ver todos los archivos modificados durante la sesión.

> [!NOTE]
> Si el agente pide permiso para una llamada a herramienta, como la ejecución de un comando de terminal, aparecerá una solicitud **Allow**. Aprueba solo los comandos que quieras permitir.

5. Espera a que termine el modo de alto contraste. El agente mostrará un mensaje final que resume los cambios.

## Pide al agente que añada el modo claro

Una vez implementado el modo de alto contraste, ampliarás la misma conversación para añadir un control del modo claro. Como continuarás en la misma sesión, el agente conserva el contexto de su trabajo: los componentes modificados, el enfoque de estilo y el uso de almacenamiento local.

1. Envía este prompt adicional en Copilot Chat:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. El agente revisará los cambios anteriores y propondrá los cambios necesarios para el modo claro.
3. Revisa cada modificación nueva y selecciona **Keep** o **Undo**.
4. Si el agente propone algo que no quieres —por ejemplo, cambiar la interfaz de los controles de forma no deseada—, detenlo con el botón **Stop** junto al campo de entrada y envía una aclaración.

> [!TIP]
> Es más rápido orientar una sesión activa que empezar de nuevo. Usa mensajes adicionales para pedir ajustes («simplifica el CSS y usa variables en vez de duplicar selectores»), corregir el rumbo («integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también atajos de teclado para ambos controles»).

## Revisa el conjunto de cambios

Antes de confirmar el trabajo, revisa todo lo que ha modificado el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa todos los archivos modificados. Deberías ver componentes Astro, estilos y las pruebas relacionadas.
3. Abre algunos archivos y revisa las diferencias. Confirma que se aplican las pautas de accesibilidad del agente personalizado: atributos ARIA, navegación por teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios en el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y pasos siguientes

Has supervisado y orientado una sesión activa con Copilot Chat: revisaste cada modificación, aceptaste o rechazaste cambios y enviaste prompts adicionales para ampliar el trabajo sin perder el contexto.

En este ejercicio:

- supervisaste el trabajo del agente en tiempo real.
- aceptaste y rechazaste modificaciones de archivos.
- orientaste la sesión con prompts adicionales.

En la [siguiente lección][next-lesson] publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios generados por el agente personalizado local durante el taller.

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
