---
title: "Ejercicio 1 - Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Agente de Copilot en la nube →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta o Copilot necesita información adicional, conviene que ese contexto esté disponible. Los [archivos de instrucciones][instruction-files] proporcionan indicaciones para que Copilot entienda no solo *qué* debe hacer, sino también *cómo*.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto del proyecto, las pautas de programación y los estándares de documentación mediante instrucciones personalizadas del repositorio y archivos aplicables a rutas concretas.
- añadirás un estándar global a `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los talleres de VS Code y CLI, aquí no compararás prompts de antes y después. El agente de Copilot en la nube trabaja de forma asíncrona con incidencias de GitHub, por lo que su influencia es más difícil de mostrar en tiempo real. Verás cómo afectan tus instrucciones a las solicitudes de incorporación de cambios que genere más adelante.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene pautas y requisitos para el desarrollo:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz de usuario debe utilizar un tema oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Cada archivo debe empezar con un bloque de comentarios que explique su función.

Con los archivos de instrucciones, proporcionarás a Copilot el contexto necesario para que realice las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan a Copilot contexto y preferencias para que comprenda el estilo de programación y los requisitos. Permiten orientar sus sugerencias y fragmentos de código. Puedes especificar convenciones, bibliotecas y el tipo de comentarios que prefieres. Puedes crear instrucciones para todo el repositorio o para tareas y tipos de archivo concretos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información de ámbito general, como la pila tecnológica, una descripción del proyecto, buenas prácticas y otras indicaciones aplicables globalmente.
- `.github/instructions/*.instructions.md`, archivos para tareas concretas o tipos de archivo, como TypeScript, Astro, componentes de interfaz o pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones se utilizan para generar código con Copilot Chat, pero no para completar código ni sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente en la nube utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` con frontmatter `applyTo` al generar código.
>
> Copilot también admite otros formatos de instrucciones, como AGENTS.md y CLAUDE.md; consulta la [referencia de compatibilidad con instrucciones personalizadas][custom-instructions-support].

### Buenas prácticas para gestionar archivos de instrucciones

Los ejemplos del proyecto muestran un enfoque representativo:

- Mantén `copilot-instructions.md` centrado en información general del proyecto, como qué se desarrolla, la estructura del proyecto y las convenciones generales.
- Utiliza archivos `*.instructions.md` para pautas de tipos de archivo (pruebas unitarias, componentes Astro o capa de datos) o tareas concretas.
- Utiliza lenguaje natural y pautas claras, con ejemplos del aspecto que debería y no debería tener el código.

No existe una única forma correcta de crear archivos de instrucciones ni de utilizar la IA. La experimentación permitirá descubrir qué funciona mejor en cada proyecto.

> [!TIP]
> Todo proyecto que utilice GitHub Copilot debería tener una colección sólida de archivos de instrucciones. En este proyecto encontrarás archivos para distintas tareas, como las [actualizaciones de interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede generar archivos de instrucciones. Cada interfaz lo permite de forma diferente, por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. El taller correspondiente lo indicará cuando sea pertinente.
>
> Para encontrar plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones del proyecto

Lee los archivos de instrucciones incluidos en este repositorio: el archivo principal `copilot-instructions.md` y los archivos `*.instructions.md` de diversas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Examina la descripción del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, fíjate en las pautas anidadas para **GitHub Actions Workflows**. Se aplican a cualquier interacción con Copilot.
3. Explora la carpeta `.github/instructions`, donde hay pautas para archivos Astro, la capa de datos Drizzle, pruebas y más.
4. Abre `.github/instructions/unit-tests.instructions.md` y observa el campo `applyTo`. Define un glob relativo a la raíz del repositorio para los archivos aplicables; por ejemplo, incluye archivos de pruebas TypeScript que coincidan con `**/*.test.ts`.
5. Examina las instrucciones para crear pruebas unitarias.
6. Abre `.github/instructions/drizzle.instructions.md` y ve al final. Verás enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así se pueden dividir instrucciones grandes en archivos reutilizables y mostrar a Copilot ejemplos que seguir. Las rutas de esos enlaces son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación, pero todavía no exige documentación en el código. En los siguientes pasos añadirás reglas para comentarios TSDoc y encabezados de archivo.

## Añade un estándar global

Este es el único cambio que realizarás en el ejercicio: indicarás que la documentación debe incluir comentarios TSDoc y encabezados en los archivos. El agente de la nube aplicará estas indicaciones a las incidencias que le asignes más adelante.

Antes de editar, crea una rama. El agente de la nube leerá las instrucciones de la rama a la que se asigne la incidencia, pero confirmarás los cambios de forma ordenada.

1. En el terminal del codespace, crea una rama nueva y cambia a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. Abre `.github/copilot-instructions.md`.
3. Busca la sección **Code formatting requirements**, cerca de la línea 27. Documenta las convenciones del proyecto, pero aún no incluye pautas de documentación del código.
4. Añade estas líneas Markdown después de las pautas actuales:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. Guarda `copilot-instructions.md`.

> [!TIP]
> Los archivos de instrucciones pueden estar en `.github/copilot-instructions.md` para proporcionar pautas globales o en archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretas. El archivo del repositorio es el lugar adecuado para estándares globales, como la regla de comentarios que acabas de añadir.

## Confirma, publica y combina la actualización

El agente de la nube obtiene instrucciones de la rama a la que se dirige la incidencia. En el próximo ejercicio, partirá de `main`; por eso, los cambios deben llegar a `main` para que pueda utilizarlos.

1. Prepara y confirma los cambios:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Publica la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de incorporación de cambios desde `custom-instructions-cloud` hacia `main` y combínala. Así, el agente en la nube podrá leer las instrucciones cuando trabaje en incidencias asignadas en el siguiente ejercicio.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante el taller, puedes omitir la rama y confirmar allí los cambios. La rama sirve para practicar un flujo de trabajo habitual.

## Resumen y pasos siguientes

Has explorado cómo Copilot utiliza los archivos de instrucciones del proyecto y has añadido un estándar global a `.github/copilot-instructions.md`. Verás cómo se aplica a las solicitudes de incorporación de cambios que genere el agente en la nube durante el resto del recorrido.

A continuación, [asignarás tu primera incidencia al agente de Copilot en la nube][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Buenas prácticas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para redactar mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
