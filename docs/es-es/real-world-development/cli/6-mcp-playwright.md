---
title: "Lección 6 - Validar la funcionalidad con Model Context Protocol (MCP) de Playwright"
description: "Inspecciona o configura Model Context Protocol (MCP) de Playwright en Copilot CLI y úsalo para explorar en un navegador la funcionalidad de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como ya hemos señalado, programar no consiste únicamente en escribir código. También hay que trabajar con datos y servicios externos, y permitir que Copilot utilice automatizaciones adicionales. Para ello sirven los servidores Model Context Protocol (MCP): permiten que Copilot vaya más allá de las funciones integradas en la CLI y acceda a más herramientas y servicios.

En esta lección:

- conocerás el Model Context Protocol (MCP) y cómo lo utiliza Copilot CLI.
- añadirás el servidor MCP de Playwright si todavía no está disponible.
- pedirás al agente que controle un navegador y explore la funcionalidad de filtrado.

## Escenario

Las pruebas unitarias y de extremo a extremo son importantes, pero para validar los cambios en la interfaz de usuario hay que interactuar con ella. Quieres que Copilot pueda utilizar el sitio web como lo haría una persona y automatizar más tareas, para tener mayor confianza en que los cambios se comportan como se espera.

## ¿Qué es Model Context Protocol (MCP)?

[Model Context Protocol (MCP)][mcp-blog-post] permite que los agentes de IA se comuniquen con herramientas y servicios externos. Con MCP, los agentes pueden acceder a información actualizada y realizar acciones en tu nombre.

Se accede a estas herramientas y recursos a través de un servidor MCP, que actúa como puente entre el agente de IA y las herramientas y servicios externos. Cada servidor MCP representa un conjunto distinto de herramientas y recursos a los que puede acceder el agente.

Algunos servidores MCP conocidos son:

- **[Servidor MCP de GitHub][github-mcp]**: proporciona acceso a las API para gestionar repositorios, incidencias y solicitudes de incorporación de cambios de GitHub.
- **[Servidor MCP de Playwright][playwright-mcp-server]**: proporciona funciones de automatización del navegador mediante Playwright.

Hay muchos otros servidores MCP disponibles. GitHub aloja un [registro de MCP][mcp-registry] para facilitar el descubrimiento y las contribuciones al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa su código fuente, verifica quién lo publica y considera las implicaciones de seguridad.

## Añade el servidor MCP de Playwright

Si el servidor MCP de Playwright todavía no está disponible, añádelo para que Copilot pueda interactuar con el sitio como lo haría una persona.

> [!NOTE]
> Si MCP de Playwright ya está configurado en Copilot CLI, omite estos pasos de configuración y continúa con la sección siguiente.

1. Vuelve a tu codespace.
2. Abre el cuadro de diálogo para añadir un servidor MCP con este comando en Copilot CLI:

   ```plaintext
   /mcp add
   ```

3. Escribe `playwright` como nombre y pulsa <kbd>Tab</kbd>.
4. Confirma el tipo de servidor **STDIO** con <kbd>Enter</kbd> y, a continuación, pulsa <kbd>Tab</kbd>.
5. Pega lo siguiente en el cuadro de diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pulsa <kbd>Control</kbd>+<kbd>S</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para guardar el servidor MCP.
7. Pulsa <kbd>Esc</kbd> para salir del cuadro de diálogo MCP.

## Pide a Copilot que explore la funcionalidad con Playwright

Ya has comprobado manualmente que la funcionalidad se comporta como esperas. Ahora utilizarás el servidor MCP de Playwright disponible en Copilot para hacer lo mismo.

1. Usa esta indicación para pedir a Copilot que utilice el servidor MCP de Playwright para validar la funcionalidad:

   ```plaintext
   Inicia la aplicación y utiliza MCP de Playwright para comprobar el filtrado según la incidencia y el plan. Dime qué funciona y qué no, sin hacer cambios. Cuando termines, detén el servidor que has iniciado.
   ```

> [!NOTE]
> No es imprescindible indicar a Copilot que use el servidor MCP; normalmente lo averiguará por su cuenta. Sin embargo, puedes especificar la herramienta para obtener resultados más coherentes y ahorrar algunas instrucciones.

2. Observa cómo Copilot enumera los pasos que realiza en el navegador para comprobar que la funcionalidad funciona.
3. Lee el informe y confirma que todo se comporta como esperas.

Copilot iniciará el servidor, utilizará Playwright para interactuar con el sitio, detendrá el servidor y te entregará un informe.

## Resumen y pasos siguientes

¡Enhorabuena! Has utilizado el servidor MCP de Playwright para explorar la funcionalidad en un navegador real desde Copilot CLI. En resumen:

- has conocido Model Context Protocol (MCP) y cómo lo utiliza Copilot CLI.
- has añadido el servidor MCP de Playwright si todavía no estaba disponible.
- has pedido al agente que controle un navegador y explore la funcionalidad de filtrado.

A continuación, [crearás un agente personalizado de QA][next-lesson] que combinará la habilidad y las herramientas del navegador en un rol especializado.

## Recursos

- [¿Qué es MCP y por qué habla todo el mundo de ello?][mcp-blog-post]
- [Servidor MCP de Microsoft Playwright][playwright-mcp-server]
- [Añadir servidores MCP a Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
