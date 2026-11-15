---
title: "Ejercicio 3 - Probar la funcionalidad con Model Context Protocol (MCP) de Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo de agente][previous-lesson] · [Lección siguiente: Agentes personalizados →][next-lesson]

Acabas de crear la funcionalidad de filtrado con el modo de agente. Antes de abrir una solicitud de incorporación de cambios (PR), comprueba que funciona en el navegador. En lugar de explorar la aplicación manualmente, conectarás el **servidor MCP de Playwright** y permitirás que Copilot controle un navegador real para probar la funcionalidad. Después, publicarás la rama y abrirás la PR.

En este ejercicio:

- conocerás Model Context Protocol (MCP) y cómo amplían los servidores MCP las herramientas de Copilot.
- añadirás el servidor MCP de Playwright al espacio de trabajo.
- pedirás a Copilot que lo utilice para probar manualmente la funcionalidad en un navegador.
- publicarás la rama y abrirás una solicitud de incorporación de cambios para el trabajo de filtrado.

## ¿Qué es Model Context Protocol (MCP)?

El modo de agente es mucho más potente cuando puede acceder a recursos externos al editor. Copilot lo consigue mediante Model Context Protocol (MCP), un estándar que permite al agente comunicarse con herramientas y servicios externos.

![Diagrama que muestra el funcionamiento interno del modo de agente y cómo interactúa con el contexto, el modelo de lenguaje grande (LLM) y las herramientas, incluidas las herramientas proporcionadas por servidores MCP y extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de IA se comuniquen con herramientas y servicios externos. Con MCP, los agentes pueden acceder a información actualizada mediante recursos y realizar acciones en tu nombre mediante herramientas.

Se accede a estas herramientas y recursos a través de un servidor MCP, que actúa como puente entre el agente de IA y las herramientas y servicios externos. El servidor MCP gestiona la comunicación entre el agente y las herramientas externas, como las interfaces de programación de aplicaciones (API) existentes o herramientas locales en forma de paquetes de npm. Cada servidor MCP representa un conjunto distinto de herramientas y recursos a los que puede acceder el agente.

Algunos servidores MCP conocidos son:

- **[Servidor MCP de GitHub](https://github.com/github/github-mcp-server)**: proporciona acceso a varias API para gestionar repositorios de GitHub. Permite al agente realizar tareas como crear repositorios, actualizarlos y gestionar incidencias y solicitudes de incorporación de cambios.
- **[Servidor MCP de Playwright](https://github.com/microsoft/playwright-mcp)**: proporciona funciones de automatización del navegador mediante Playwright. Permite al agente navegar por páginas web, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP disponibles que proporcionan acceso a diferentes herramientas y recursos. GitHub aloja un [registro de MCP](https://github.com/mcp) para facilitar el descubrimiento y las contribuciones al ecosistema.

> [!CAUTION]
> Por motivos de seguridad, trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de utilizar uno, revisa detenidamente su código fuente, verifica quién lo publica y considera las implicaciones de seguridad. Utiliza únicamente servidores MCP de confianza y ten cuidado al conceder acceso a recursos u operaciones sensibles.

## Revisa la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en tu codespace.
2. Deberías encontrar un servidor `github` ya configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

La entrada `github` viene con la plantilla del proyecto. Por eso Copilot pudo acceder a las incidencias pendientes en el ejercicio anterior. Utiliza el [servidor MCP remoto de GitHub][remote-github-mcp-server], así que no hace falta instalar nada en local. VS Code se conecta mediante HTTP y te autentica con GitHub la primera vez que Copilot utiliza una de sus herramientas.

## Añade el servidor MCP de Playwright

Ahora añadirás un segundo servidor. El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador que puede controlar, justo lo que necesitas para probar la funcionalidad.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github` para que el archivo quede así:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Guarda el archivo.

A diferencia del servidor remoto `github`, `playwright` es un servidor **local**: VS Code lo inicia en tu equipo con `npx @playwright/mcp@latest`. La opción `--headless` indica a Playwright que ejecute el navegador sin una ventana visible. Esto es necesario en un codespace, donde no hay un escritorio que pueda mostrarlo.

> [!NOTE]
> El proyecto Tailspin Toys ya utiliza Playwright para las pruebas de extremo a extremo, así que normalmente el navegador que necesita Playwright ya está instalado. Si Copilot informa de que falta un navegador, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

## Inicia el servidor y confía en él

VS Code inicia los servidores MCP **cuando se necesitan**: la primera vez que Copilot necesita las herramientas de un servidor, VS Code lo inicia y te pregunta si confías en él.

1. En `.vscode/mcp.json`, aparece una acción **Start** encima de la entrada `playwright`. Selecciónala para iniciar el servidor y confirmar que la configuración es correcta.
2. Si VS Code te pregunta si confías en el servidor, revisa la configuración y confirma que confías en él para que se inicie.

El inicio manual es opcional. Si omites este paso, VS Code iniciará el servidor y te pedirá que confirmes si confías en él la primera vez que Copilot necesite sus herramientas.

## Prueba la funcionalidad de filtrado

El [servidor MCP de Playwright][playwright-mcp-server] permite que Copilot controle un navegador real. En lugar de recorrer la aplicación para comprobar el trabajo, el agente puede abrir una página, navegar, aplicar filtros, leer los resultados y resumirlos. Es una forma rápida de confirmar que la funcionalidad se comporta como esperas sin salir de la conversación.

Internamente, el servidor MCP de Playwright utiliza el [árbol de accesibilidad][playwright-mcp-server] de la página en lugar de capturas de pantalla. Así, el agente trabaja con elementos estructurados y etiquetados, como botones, enlaces y elementos de lista, de forma similar a las tecnologías de asistencia. Por tanto, una comprobación funcional rápida también ofrece una comprobación básica de accesibilidad.

Con el servidor conectado y la aplicación en ejecución, pide a Copilot que pruebe la funcionalidad de filtrado.

1. Envía esta indicación a Copilot:

   ```text
   Utiliza el servidor MCP de Playwright para abrir un navegador en la aplicación que está en ejecución en http://localhost:4321 y verificar la nueva función de filtrado de juegos:

   1. Ve a la página de juegos y anota cuántos aparecen.
   2. Aplica un filtro de categoría y confirma que la lista solo muestra los juegos de esa categoría.
   3. Borra el filtro y, después, aplica uno de editor y confirma que la lista solo muestra los juegos de ese editor.
   4. Combina un filtro de categoría y otro de editor, y confirma que los resultados cumplen ambos.

   Indica lo que observas en cada paso y señala cualquier comportamiento inesperado.
   ```

Copilot abrirá un navegador mediante el servidor MCP de Playwright, seguirá los pasos y te informará de lo que encuentre. Compara el resumen con los criterios de aceptación de la incidencia. Si algo no parece correcto, haz preguntas de seguimiento o pide a Copilot que corrija el código antes de abrir una PR.

> [!NOTE]
> La aplicación debe estar en ejecución en `http://localhost:4321` para esta prueba. Si has detenido el servidor de desarrollo, vuelve a iniciarlo antes de enviar la indicación. La primera vez que Copilot utilice el servidor MCP de Playwright, es posible que tenga que descargar un navegador. Si informa de que falta uno, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publica la rama y crea una solicitud de incorporación de cambios

Ya has confirmado que la funcionalidad funciona. Ahora abrirás una solicitud de incorporación de cambios (PR) para que el equipo la revise. El primer paso es publicar la rama `filtering-vscode`.

1. Abre el panel **Source Control** del codespace y revisa los cambios de Copilot.
2. Prepara los cambios seleccionando el icono **+**.
3. Genera un mensaje de confirmación con el botón **Sparkle**.

   ![Captura de pantalla del panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para enviar la rama al repositorio.

## Crea la solicitud de incorporación de cambios

Hay varias formas de crear una solicitud de incorporación de cambios, como usar github.com o la interfaz de línea de comandos (CLI) de GitHub. Como ya estás trabajando con GitHub Copilot, pídele que la cree. Puede buscar la incidencia correspondiente y asociarla a la PR.

1. Abre el panel Copilot Chat y selecciona **New Chat** para iniciar una sesión nueva.
2. Confirma que **Agent** está seleccionado en el selector de agentes para que Copilot pueda utilizar las herramientas de GitHub.
3. Pide a Copilot que cree la PR:

   ```plaintext
   Busca en el repositorio la incidencia relacionada con el filtrado por categoría y editor. Crea una solicitud de incorporación de cambios para la rama actual y asóciala a la incidencia correcta.
   ```

4. Cuando sea necesario, selecciona **Continue** para permitir que Copilot reúna información y realice operaciones. La primera vez que utilice una herramienta de GitHub, es posible que se te pida que te autentiques; sigue las instrucciones para permitirlo.
5. Observa cómo Copilot busca entre las incidencias, encuentra la adecuada y crea la PR.
6. Selecciona el enlace que genera Copilot para revisar la solicitud, pero **no la combines todavía**.

## Resumen y pasos siguientes

¡Enhorabuena! En este ejercicio:

- has conocido Model Context Protocol (MCP) y cómo amplían los servidores MCP las herramientas de Copilot.
- has añadido el servidor MCP de Playwright al espacio de trabajo.
- lo has utilizado para probar manualmente la funcionalidad de filtrado en un navegador antes de publicarla.
- has publicado la rama y abierto una solicitud de incorporación de cambios para el trabajo de filtrado.

Has utilizado un servidor MCP para *probar* una funcionalidad, pero MCP es solo una forma de ampliar Copilot. A continuación, [crearás un agente personalizado][next-lesson] para agilizar tareas específicas, como las revisiones de accesibilidad.

## Recursos

- [¿Qué es MCP y por qué habla todo el mundo de ello?][mcp-blog-post]
- [Servidor MCP de Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP de GitHub][github-mcp-server]
- [Registro de MCP de GitHub][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
