---
title: "Ejercicio 0: Requisitos previos"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Antes de empezar las lecciones de Copilot CLI, debes prepararlo todo. Crearás una copia del repositorio Tailspin Toys y pondrás en marcha un [codespace][codespaces], cuyo terminal integrado utilizarás para instalar y ejecutar Copilot CLI en la lección siguiente.

## Configurar el repositorio del laboratorio

Para crear una copia del repositorio para el código que vas a crear, generarás una instancia a partir de la [plantilla][template-repository]. La nueva instancia contendrá todos los archivos necesarios para el laboratorio y la usarás a medida que avances por los ejercicios.

1. En una nueva ventana del navegador, ve al repositorio de GitHub de este laboratorio: `https://github.com/github-samples/tailspin-toys`.
2. Crea tu propia copia del repositorio seleccionando el botón **Use this template** en la página del repositorio del laboratorio. Después, selecciona **Create a new repository**.

    ![Captura del botón Use this template](../../../_images/ex0-use-template.png)

3. Si estás realizando el taller como parte de un evento dirigido por GitHub o Microsoft, sigue las instrucciones proporcionadas por el personal mentor. En caso contrario, puedes crear el nuevo repositorio en una organización en la que tengas acceso a GitHub Copilot.

    ![Captura de la configuración de la plantilla del repositorio](../../../_images/ex0-repository-settings.png)

4. Anota la ruta del repositorio que has creado (`organization-or-user-name/repository-name`), ya que la consultarás más adelante en el taller.

> [!NOTE]
> **Tu backlog está listo**
>
> Cuando creas tu repositorio a partir de la plantilla, se crea automáticamente un backlog de incidencias de GitHub para ti. Trabajarás con esas incidencias durante todo el taller; no tienes que crear ninguna por tu cuenta.

## Crear un codespace

Ahora usarás un codespace para completar los ejercicios del laboratorio.

[GitHub Codespaces][codespaces] es un entorno de desarrollo en la nube que permite escribir, ejecutar y depurar código directamente en el navegador. Ofrece un editor completo con compatibilidad con varios lenguajes de programación, extensiones y herramientas.

1. Ve a tu repositorio recién creado.
2. Selecciona el botón verde **Code**.

    ![Botón Code](../../../_images/ex0-code-button.png)

3. Selecciona la pestaña **Codespaces** y, después, selecciona **Create codespace on main**.

    ![Crear un codespace nuevo](../../../_images/ex0-create-codespace.png)

4. Espera a que termine la configuración. La plantilla instalará las dependencias del proyecto, Chromium de Playwright y la base de datos local.
5. Si aparece el mensaje **Do you trust the authors of the files in this folder?**, selecciona **Trust Folder & Continue**.
6. Abre un terminal en la raíz del repositorio e inicia la aplicación:

   ```bash
   npm run dev
   ```

7. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser** y confirma que se carga Tailspin Toys.
8. Vuelve al terminal y detén el servidor de desarrollo con <kbd>Ctrl</kbd>+<kbd>C</kbd>.

> [!CAUTION]
> Ya tienes todo preparado. En esta lección has creado una copia del proyecto a partir de la plantilla, has creado un codespace y has confirmado que el proyecto está listo.

> [!NOTE]
> Este taller está diseñado para ejecutarse dentro de un codespace.

## Siguiente paso

A continuación, [instalarás GitHub Copilot CLI][next-lesson] en tu codespace y te autenticarás con tu cuenta de GitHub.

## Recursos

- [Información general de GitHub Codespaces][codespaces]
- [Crear un repositorio a partir de una plantilla][template-repository]
- [Primeros pasos con Codespaces][codespaces-quickstart]

[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-install-copilot-cli/
