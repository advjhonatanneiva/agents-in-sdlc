---
title: "Ejercicio 2 - Añadir funcionalidades con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y tareas como ejecutar pruebas. El flujo de trabajo habitual de un desarrollador consiste en localizar todos los archivos necesarios, realizar los cambios, ejecutar pruebas, depurar, averiguar qué archivo falta y volver a modificarlo. Y así sucesivamente.

El modo de agente de Copilot puede ayudarte con estas tareas.

El modo de agente de Copilot está diseñado para trabajar de forma más autónoma en el IDE. Se comporta de forma parecida a un desarrollador: primero explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Explora cómo utilizar el modo de agente para añadir funcionalidades al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo de agente y el agente en la nube ofrecen experiencias distintas. El modo de agente trabaja en el IDE, con ciclos rápidos de comentarios y respuestas. El agente en la nube trabaja de forma asíncrona como otro miembro del equipo e interactúa contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio descubrirás cómo:

- el modo de agente de Copilot explora el proyecto, identifica los archivos relevantes y coordina los cambios.
- el modo de agente de GitHub Copilot implementa funcionalidades en la interfaz de usuario y la capa de datos.
- revisar los cambios y las pruebas generados por el modo de agente de Copilot antes de combinarlos en el código base.

## Escenario

A medida que crece la lista de juegos, quieres que los usuarios puedan filtrarla por categoría y editor. En el ejercicio anterior ya has añadido una función auxiliar para editores. Ahora completarás el trabajo restante en la capa de datos, la interfaz de usuario y las pruebas con el modo de agente de Copilot.

## Ejecuta el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio web de Tailspin Toys para conocer su funcionalidad actual.

El sitio web es una plataforma de financiación colectiva de juegos de mesa con una temática de desarrollo. Permite mostrar juegos y sus detalles. Es una única aplicación Astro que genera páginas en lenguaje de marcado de hipertexto (HTML) estático durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante el asignador relacional de objetos (ORM) Drizzle en su frontmatter; no hay una interfaz de programación de aplicaciones (API) de backend independiente ni un framework de interfaz de usuario en el cliente. Los asistentes reutilizables para acceder a los datos se encuentran en `src/lib/`, y la interactividad se añade mediante un pequeño `<script>` de Astro con ámbito limitado que utiliza las API estándar del modelo de objetos del documento (DOM).

### Inicia el sitio web

Para facilitar la ejecución del sitio web, se ha incluido la tarea `npm run dev`, que inicia el único servidor de desarrollo de Astro. Ejecútala en tu codespace de GitHub con estos pasos:

1. Vuelve a tu codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal en el codespace con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama para el trabajo de filtrado y cámbiate a ella:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio web con este comando:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, aparecerá un mensaje con la dirección web (URL), parecido al siguiente:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir una ventana del navegador en `http://localhost:4321`, ciérralo seleccionando **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En un codespace, al seleccionar el enlace localhost en el terminal, se te redirige a `https://<your-codespace-name>-4321.app.github.dev/`. Se trata de un túnel privado al codespace, que ahora aloja el servidor web.

### Explora el sitio web

Cuando se esté ejecutando el sitio, explora sus funciones principales:

- **Página de inicio**: muestra una lista de juegos de mesa con títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, accederás a una página con más información, como el título, la descripción, el editor y la categoría.

## Explora la lista de trabajo pendiente con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando sea necesario.

La implementación inicial del sitio funciona, pero queremos añadir funciones. Empieza por revisar la lista de trabajo pendiente. Al crear el proyecto a partir de la plantilla, se generaron automáticamente varias incidencias. Pide a GitHub Copilot que te las muestre.

1. Selecciona **Agent** en el selector de agentes de la vista Chat. El agente **Agent** planifica e implementa cambios en varios archivos de forma autónoma, ejecuta comandos de terminal y utiliza herramientas.

   ![Captura de pantalla del selector de agente en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no prefieren un modelo sobre otro. Al crear el taller, utilizamos Claude Sonnet 4.5 y lo incluimos para que las sugerencias de código sean relativamente coherentes y la experiencia resulte satisfactoria. Sin embargo, como los modelos de lenguaje grandes (LLM, del inglés *large language models*) son probabilísticos, es posible que las sugerencias difieran de las indicadas en el taller. Es normal y esperable.

> [!NOTE]
> Debido al carácter probabilístico de los LLM, Copilot puede utilizar un comando MCP diferente, pero debería poder completar la tarea.

3. Pide a Copilot que muestre las incidencias pendientes con esta indicación:

   ```plaintext
   Muéstrame la lista de trabajo pendiente de mi repositorio de GitHub. Ayúdame a priorizar los elementos según cuáles resultarán más útiles para el usuario.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera las incidencias.
5. Revisa la lista generada.

Observa que Copilot también ha priorizado las incidencias según las que considera más útiles para el usuario.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto para su trabajo. Utilizarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funciones al sitio web.

1. En tu codespace, abre `.github/instructions/ui.instructions.md`.
2. Fíjate en las directrices generales para añadir funciones. Incluyen:
   - una descripción general de la arquitectura.
   - principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - enlaces a archivos de instrucciones específicos para Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones permiten hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones. Así puedes reutilizar las directrices, dividir conjuntos grandes en archivos más manejables y señalar ejemplos y plantillas.

## Implementa la funcionalidad de filtrado

Para completar el filtrado, la aplicación necesita al menos tres cambios:

- añadir o ajustar la lógica restante de filtrado en la capa de datos (`src/lib/`).
- añadir o actualizar las pruebas del comportamiento de filtrado.
- actualizar la página de lista de juegos para incluir la interfaz de filtrado.

Además, hay que ejecutar las pruebas y confirmar que se superan antes de combinar los cambios en el código base. El modo de agente de Copilot puede encargarse de estas tareas. Añade la funcionalidad:

1. Puedes continuar la conversación actual con Copilot o iniciar otra seleccionando **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Captura de pantalla de un ejemplo de selección del archivo de instrucciones de UI](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado en el selector de agentes de la vista Chat.

   ![Captura de pantalla del selector de agente en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad de acuerdo con la incidencia correspondiente de la lista de trabajo pendiente:

   ```plaintext
   Actualiza el sitio para incluir filtros por editor y categoría según los requisitos de la incidencia relacionada de la lista de trabajo pendiente. Ya existe una función auxiliar para editores del ejercicio anterior; conserva y ajusta ese trabajo cuando sea necesario mientras completas los cambios restantes en la capa de datos, la interfaz de usuario y las pruebas. Antes de terminar, confirma que todas las pruebas se superan. El servidor ya está en ejecución, así que no hace falta iniciarlo.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la funcionalidad: los asistentes de la capa de datos, la interfaz y las pruebas. Después, modifica los archivos y ejecuta las pruebas.

   ![Captura de pantalla de Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. La duración dependerá de la complejidad de la tarea y del código base. Mientras trabaja, puede que aparezcan los botones **Keep** y **Undo** en el editor. Cuando termine, aparecerá uno de estos botones para cada cambio; no hace falta seleccionarlos mientras el agente sigue trabajando.

7. Cuando Copilot te lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de pantalla de un cuadro de diálogo en el panel Copilot Chat que pide confirmación para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Es posible que haya pausas e incluso que algunas pruebas fallen durante el proceso. No pasa nada: Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea sin detectar errores.

   ![Captura de pantalla de una sesión completa con el modo de agente de Copilot](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado para detectar posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot o por cualquier herramienta de IA.

10. Vuelve al navegador donde se está ejecutando el sitio y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana Copilot Chat.

## Resumen y pasos siguientes

¡Enhorabuena! En este ejercicio has utilizado el modo de agente de GitHub Copilot para añadir funciones al sitio web de Tailspin Toys. Has descubierto cómo:

- el modo de agente de Copilot explora el proyecto, identifica los archivos relevantes y coordina los cambios.
- el modo de agente de GitHub Copilot puede implementar funciones en la interfaz de usuario y la capa de datos.
- revisar los cambios y las pruebas generados por el modo de agente de Copilot antes de combinarlos en el código base.

Ahora [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementar paginación

A medida que crezca la lista de juegos, será necesario habilitar la paginación. Aplica las habilidades de este ejercicio y pide a Copilot que implemente la paginación en el sitio. Al preparar la solicitud:

- sigue las prácticas recomendadas, incluidos los archivos de instrucciones existentes.
- decide cómo debería funcionar la paginación: por ejemplo, si el usuario podrá seleccionar el tamaño de página o si será fijo.
- proporciona a Copilot la orientación necesaria para obtener la implementación que quieres.
- puede que tengas que iterar con GitHub Copilot, pedir cambios y proporcionar contexto. Es el flujo normal cuando se trabaja con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo se utilizan][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
