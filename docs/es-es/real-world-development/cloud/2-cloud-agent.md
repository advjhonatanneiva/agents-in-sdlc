---
title: "Ejercicio 2 - Agente en la nube de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que casi todas las organizaciones tengan problemas de deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que necesita actualizaciones o funcionalidades que llevan tiempo en la lista de trabajo pendiente porque no ha habido tiempo para implementarlas. El agente en la nube de GitHub Copilot está diseñado para tareas como actualizar código y añadir funciones de forma autónoma. Cuando termina, crea un borrador de solicitud de incorporación de cambios (PR) para que lo revise un desarrollador. Así puedes delegar tareas tediosas, acelerar el desarrollo y centrarte en objetivos más amplios.

Con el agente en la nube de Copilot explorarás:

- cómo personalizar el entorno de generación de código.
- cómo asegurarte de que las operaciones se realicen de forma segura.
- la importancia de delimitar claramente las incidencias.
- cómo asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica que quiere abordar. Los contratistas que crearon la primera versión del sitio dejaron la documentación en mal estado: comprobarás que falta por completo. Como primer paso, el equipo quiere añadir comentarios de documentación de TypeScript (TSDoc) a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar la búsqueda de juegos. Quiere mostrar juegos relacionados —otros títulos de la misma categoría— en la página de detalles de cada juego para que los usuarios puedan seguir explorando. No necesita una implementación refinada; solo una versión que permita probar la experiencia de usuario. Este trabajo bloquea otras tareas, pero ahora mismo hay incidencias de mayor prioridad.

Son dos ejemplos de tareas que pueden quedar relegadas y que se pueden asignar al agente en la nube de Copilot. El agente trabajará en ellas de forma asíncrona mientras tú te ocupas de otras tareas. Después podrás volver para revisar su trabajo y confirmar que cumple los requisitos.

## Presentación del agente en la nube de GitHub Copilot

El [agente en la nube de GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma parecida a un desarrollador. Al igual que trabajarías con otro desarrollador, puedes [asignar una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Cuando se le asigna una tarea, Copilot crea una solicitud de incorporación de cambios en borrador para registrar el progreso, configura un entorno y empieza a trabajar. Puedes inspeccionar la sesión mientras sigue en curso o después de que termine. Cuando la solución esté lista para revisarse, Copilot te mencionará en la solicitud.

## La importancia de delimitar las instrucciones

GitHub Copilot no hace magia: no te exime de pensar. Incluso las tareas aparentemente sencillas pueden ser complejas cuando se examinan de cerca. Por eso, [delimita con cuidado las tareas que asignas al agente en la nube de Copilot][cloud-agent-best-practices]. Trátalo como a un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. La IA generativa no cambia los fundamentos del desarrollo de software.

## Instrucciones personalizadas en este repositorio

En ejercicios anteriores has conocido las instrucciones personalizadas y cómo orientan a Copilot. Si ya has completado un ejercicio práctico sobre instrucciones personalizadas, habrás visto cómo funciona `.github/copilot-instructions.md`. Si no, este es un buen momento para revisar los archivos de instrucciones que ya incluye el repositorio. Antes de asignar trabajo al agente en la nube, ábrelos en modo de solo lectura para poder observar su efecto más adelante.

Para ver cómo influyen estas instrucciones más adelante, abre los siguientes archivos en la interfaz web de GitHub o en un codespace que ya esté en ejecución:

1. Abre `.github/copilot-instructions.md` y revisa la sección **Code standards**, en especial las convenciones de TypeScript y los requisitos de comentarios TSDoc.
2. Abre `.github/instructions/unit-tests.instructions.md` y fíjate en el frontmatter `applyTo`, que aplica estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes al agente en la nube la incidencia `Code lacks documentation` en la sección siguiente, observa si la PR incluye comentarios TSDoc, convenciones de TypeScript y comentarios de cabecera. Los archivos de instrucciones explican esas decisiones. Volverás a revisarlas en un ejercicio posterior.

## Configura el entorno de desarrollo del agente en la nube de Copilot

La creación de código suele requerir un entorno específico y la ejecución de scripts de configuración, independientemente de quién realice el trabajo. Lo mismo ocurre cuando asignas tareas a Copilot, que funciona de forma parecida a un ingeniero de software.

El agente en la nube utiliza [GitHub Actions][github-actions] como entorno de trabajo. Puedes personalizarlo mediante un [flujo de trabajo de configuración especial][setup-workflow], definido en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que el agente empiece a trabajar. Así puede acceder a las herramientas y dependencias de desarrollo necesarias. Este flujo ya está configurado para facilitar el taller y darte la oportunidad de explorarlo. Proporciona Node.js, las dependencias del proyecto, los navegadores de las pruebas de extremo a extremo y la base de datos SQLite migrada y precargada para la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Establece los permisos mínimos necesarios para tus pasos. Copilot utilizará su propio token para sus operaciones.
    permissions:
      # Si quieres clonar el repositorio como parte de los pasos de configuración, por ejemplo para instalar dependencias, necesitarás el permiso `contents: read`. Si no clonas el repositorio en esos pasos, Copilot lo hará automáticamente cuando terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de la aplicación: Node.js (ahora toda la aplicación utiliza Astro y Drizzle/libSQL)
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

      # Migra y precarga la base de datos SQLite local para que las compilaciones y las pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero tiene algunos aspectos importantes:

- Contiene un único trabajo llamado `copilot-setup-steps`. Este trabajo se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la solicitud de incorporación de cambios.
- Observa el desencadenador `workflow_dispatch`, que permite ejecutar manualmente el flujo de trabajo desde la pestaña **Actions** del repositorio. Resulta útil para comprobar que funciona correctamente sin tener que esperar a que Copilot lo ejecute.

## Añade documentación

Aunque todos reconocen la importancia de la documentación, muchos proyectos tienen información obsoleta o carecen de ella. Este tipo de deuda técnica suele quedar desatendida, reduce la productividad y dificulta el mantenimiento del código base y la incorporación de nuevos desarrolladores. Copilot puede ayudar a crear documentación, así que esta es una buena tarea para asignar al agente en la nube. Trabajará en segundo plano y podrás revisar el resultado en un ejercicio posterior.

1. En una nueva pestaña del navegador, ve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue** para crear una incidencia vacía.
5. Escribe `Code lacks documentation` en el campo **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    Nuestra organización requiere que las funciones y los métodos incluyan comentarios TSDoc cuando sean útiles. Sin embargo, las actualizaciones recientes no han seguido este estándar. Hay que actualizar el código existente para incluir comentarios de documentación cuando ayuden a aclarar el comportamiento.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

  ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests**.
11. Abre la solicitud de incorporación de cambios (PR) recién generada, cuyo título será parecido a `[WIP]: Code lacks documentation`. Si no aparece ninguna PR nueva, espera un momento y actualiza la ventana del navegador.
12. Después de unos minutos, Copilot debería crear una lista de tareas.

> [!NOTE]
> Es posible que la lista de tareas tarde varios minutos en aparecer en la PR. Copilot está configurando su entorno mediante el flujo de trabajo anterior, analizando el proyecto y determinando el mejor enfoque.

13. Revisa la lista y las tareas que va a completar.
14. Desplázate por la cronología de la PR. Deberías ver un aviso de que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

  ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Es posible que tengas que actualizar la ventana para ver el indicador actualizado.

16. Observa que puedes recorrer la sesión en directo y ver cómo Copilot resuelve el problema. Explora el código y comprende el estado del proyecto, observa cómo Copilot se detiene a planificar el enfoque y cómo crea código.

El proceso puede tardar varios minutos. Una de las principales ventajas del agente en la nube de Copilot es que trabaja de forma asíncrona y te permite ocuparte de otras tareas. Aprovecharemos esa función y, mientras Copilot trabaja en otra incidencia, añadiremos funciones a la aplicación.

## Añade una sección de juegos relacionados a la página de detalles

Una de las ventajas del agente en la nube de GitHub Copilot es que permite dividir el trabajo: tú puedes ocuparte de unas tareas mientras el agente trabaja en otras. Aunque añadir una sección de juegos relacionados para el equipo de diseño no debería llevar mucho tiempo, podrías aprovechar ese tiempo para otras tareas. Asigna el trabajo al agente en la nube de Copilot.

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue** para utilizar la plantilla vacía.
5. Escribe `Show related games on the game details page` en el campo **Title**.
6. Escribe lo siguiente en **Description**:

    ```markdown
    Queremos ayudar a los usuarios a descubrir más juegos mostrando juegos relacionados en la página de detalles de cada juego. El equipo de diseño quiere explorar la experiencia de usuario y realizar pruebas de aceptación. Estos son los requisitos:

   - Añadir una función auxiliar de acceso a datos en `src/lib/` que devuelva otros juegos de la misma categoría que un juego determinado, sin incluir ese juego
   - Mostrar una sección "Related games" en la página de detalles del juego que utilice el asistente
   - Gestionar el caso en que un juego no tenga juegos relacionados
   - Crear pruebas unitarias para el nuevo asistente
   - Confirmar que todas las pruebas se superan antes de crear la PR
   ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

  ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Poco después, deberías ver una serie de 👀 en el primer comentario de la incidencia. Indica que Copilot está trabajando en ella.

![Copilot utiliza el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en la segunda tarea. El agente en la nube de Copilot funciona de forma parecida a un ingeniero de software, así que no hace falta que lo supervises continuamente; puedes revisar el resultado cuando termine. Mientras tanto, crea y utiliza agentes personalizados.

## Resumen y pasos siguientes

En este ejercicio has explorado el [agente en la nube de GitHub Copilot][copilot-agents]. Puedes asignarle incidencias para que trabaje en ellas de forma asíncrona. Copilot puede abordar deuda técnica, crear funciones o ayudar a migrar código entre frameworks.

Has explorado estos conceptos:

- cómo personalizar el entorno de generación de código.
- cómo asegurarte de que las operaciones se realicen de forma segura.
- la importancia de delimitar claramente las incidencias.
- cómo asignar incidencias a Copilot.

Mientras el agente en la nube trabaja, puedes crear y utilizar agentes personalizados. El [agente en la nube de Copilot también puede utilizar servidores de Model Context Protocol (MCP)][cloud-agent-mcp] y puede acceder a las instrucciones personalizadas que has explorado en ejercicios anteriores.

## Recursos

- [Acerca del agente en la nube de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube de Copilot][cloud-agent-best-practices]

| [Lección siguiente: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
