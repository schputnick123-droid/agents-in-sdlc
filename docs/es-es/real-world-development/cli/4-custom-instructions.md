---
title: "Lección 4 - Guiar a Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta, conviene que Copilot tenga esas indicaciones disponibles. Los [archivos de instrucciones][instruction-files] describen no solo *qué* código se quiere, sino también *cómo* debe estructurarse. Ahora que ya has creado el filtrado, explorarás las instrucciones utilizadas por Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección:

- explorarás cómo llegan al agente las instrucciones del repositorio y las instrucciones aplicables a rutas concretas.
- actualizarás el archivo de instrucciones para que se sigan los estándares de programación.
- observarás el efecto de los archivos de instrucciones en el código.

## Escenario

Tailspin Toys tiene pautas y requisitos para el desarrollo:

- Los comentarios deben explicar la intención y las decisiones que no resulten evidentes, no repetir el código.
- Las funciones exportadas de `db/` y `src/lib/` deben documentar su propósito, parámetros y valores devueltos mediante TSDoc o JSDoc, incluido el argumento `db` inyectable cuando exista.
- Los componentes Astro reutilizables deben documentar sus contratos `Props`, y los comentarios deben mantenerse actualizados cuando cambie el código relacionado.
- Se deben conservar las pautas existentes de formato y lint.

Los archivos de instrucciones proporcionan a Copilot la información necesaria para seguir estas prácticas.

## Archivos de instrucciones

Las instrucciones personalizadas proporcionan a Copilot contexto y preferencias para orientar sus sugerencias. Puedes especificar convenciones, bibliotecas y el tipo de comentarios que prefieres. Hay dos tipos:

- `.github/copilot-instructions.md`, un archivo que se envía a Copilot en **todas** las solicitudes del repositorio y contiene información de ámbito general.
- `.github/instructions/*.instructions.md`, archivos para lenguajes, tipos de archivo o tareas concretas.

> [!NOTE]
> Otros formatos y la compatibilidad con ellos dependen del entorno. Consulta la [referencia de compatibilidad con instrucciones personalizadas][custom-instructions-support] antes de depender de un formato concreto.

## Explora los archivos de instrucciones del proyecto

La plantilla incluye archivos de instrucciones para explorar:

1. Abre `.github/copilot-instructions.md` y examina la descripción del proyecto y las pautas de programación. Se aplican a todas las interacciones con Copilot en este repositorio.
2. Explora `.github/instructions`; hay pautas para archivos Astro, la capa de datos Drizzle, pruebas y más.
3. Abre `.github/instructions/unit-tests.instructions.md` y fíjate en el frontmatter `applyTo`, que limita su aplicación a archivos `**/*.test.ts`.
4. Antes de continuar, revisa los archivos de instrucciones en modo de solo lectura para poder comparar sus efectos más adelante.

## Actualiza los archivos de instrucciones para reflejar las pautas del equipo

La plantilla es un buen punto de partida, pero falta una pauta. Actualiza `copilot-instructions.md` para pedir comentarios TSDoc en el nuevo código TypeScript.

1. En `.github/copilot-instructions.md`, busca la sección **Code formatting guidance**, cerca de la línea 35.
2. Añade este elemento al final de la sección:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

El archivo se guardará automáticamente.

## Aplica las instrucciones actualizadas

Copilot CLI carga las instrucciones del repositorio al comenzar una conversación. Tras guardar las instrucciones, cierra y reanuda la conversación de filtrado para que Copilot cargue las nuevas pautas sin perder el contexto de la funcionalidad.

1. Si la conversación de filtrado sigue abierta, introduce `/exit` para cerrarla. Copilot CLI guarda automáticamente las conversaciones.
2. Desde la raíz del repositorio y en la rama `game-filters-cli`, reanuda la conversación guardada:

   ```bash
   copilot --resume
   ```

   Si se te solicita, selecciona la conversación de filtrado y confirma que está seleccionada la rama `game-filters-cli`.

3. Pide a Copilot que aplique las pautas actualizadas:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Introduce `/diff` y revisa los archivos TypeScript modificados. Observa los comentarios TSDoc y confirma que describen el código con precisión.

## Resumen y pasos siguientes

Has explorado cómo Copilot CLI utiliza el contexto de los archivos de instrucciones y has aplicado un estándar nuevo a la funcionalidad. En esta lección:

- exploraste cómo llegan al agente las instrucciones del repositorio y las aplicables a rutas concretas.
- actualizaste el archivo de instrucciones para garantizar que se sigan las convenciones.
- observaste el efecto de los archivos de instrucciones en el código.

A continuación, [personalizarás y ejecutarás la habilidad reutilizable `quality-checks`][next-lesson] para comprobar que lint y las pruebas se ejecutan de forma coherente.

## Recursos

- [Añadir instrucciones personalizadas para Copilot CLI][instruction-files]
- [Compatibilidad con instrucciones personalizadas][custom-instructions-support]
- [Buenas prácticas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: archivos de instrucciones y otros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
