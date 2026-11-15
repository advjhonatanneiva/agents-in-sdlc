---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Lección siguiente: Modo de agente →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta o Copilot necesita información adicional, conviene proporcionarle ese contexto. Hay varias formas de hacerlo. Una de las principales son los [archivos de instrucciones][instruction-files], que permiten orientar la generación de código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante instrucciones personalizadas del repositorio y archivos de instrucciones que se aplican a rutas concretas.
- generarás la primera parte de los datos para el filtrado (una función auxiliar para editores) con las instrucciones actuales.
- añadirás un estándar nuevo para todo el repositorio en `.github/copilot-instructions.md`.
- repetirás la misma indicación y observarás cómo el código generado incorpora el nuevo estándar.
- confirmarás la actualización de instrucciones y la parte de filtrado en `main` para que Copilot pueda utilizar las nuevas directrices en el ejercicio siguiente.

> [!CAUTION]
> El código generado puede no seguir todos los estándares indicados. Copilot no es determinista. El objetivo del ejercicio es observar la tendencia del comportamiento después de actualizar las instrucciones, no obtener exactamente el mismo resultado carácter por carácter.

## Archivos de instrucciones

### Escenario

Como cualquier empresa de desarrollo, Tailspin Toys tiene directrices y requisitos para las prácticas de desarrollo. Entre ellos:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz de usuario debe utilizar un tema oscuro y tener un aspecto moderno.
- El código debe incluir comentarios de documentación de TypeScript (TSDoc).
- Al principio de cada archivo debe haber un bloque de comentarios que describa su función.

Con los archivos de instrucciones, te asegurarás de que Copilot dispone de la información adecuada para realizar las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan a Copilot contexto y preferencias para ayudarle a entender mejor el estilo de programación y los requisitos. Esta función permite orientar las sugerencias y los fragmentos de código. Puedes especificar convenciones de programación, bibliotecas e incluso el tipo de comentarios que quieres incluir en el código. Puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md` es un único archivo de instrucciones que se envía a Copilot en todas las solicitudes del repositorio. Debe contener información del proyecto relevante para la mayoría de las solicitudes de chat o CLI, como la pila tecnológica, una descripción general de lo que se está creando, las prácticas recomendadas y otras directrices generales.
- Los archivos `.github/instructions/*.instructions.md` pueden crearse para tareas o tipos de archivos concretos. Permiten proporcionar directrices para lenguajes concretos, como TypeScript o Astro, o para tareas como crear un componente de interfaz de usuario o un conjunto nuevo de pruebas unitarias.

> [!NOTE]
> En un IDE, los archivos de instrucciones solo se utilizan para generar código en Copilot Chat, no para las sugerencias de código ni las sugerencias de edición siguiente.
>
> Copilot Chat, Copilot CLI y Copilot cloud agent utilizan las instrucciones del repositorio y los archivos `*.instructions.md` que tienen frontmatter `applyTo`.
>
> Copilot también [admite otros formatos de archivos de instrucciones][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

La creación de archivos de instrucciones queda fuera del alcance de este ejercicio. Sin embargo, los ejemplos del proyecto muestran un enfoque representativo. A grandes rasgos:

- Limita las instrucciones de `copilot-instructions.md` a directrices del proyecto, como la descripción de lo que se está creando, la estructura del proyecto y los estándares de programación generales.
- Utiliza archivos `*.instructions.md` para proporcionar directrices específicas de tipos de archivos, como pruebas unitarias, componentes de Astro y la capa de datos, o para tareas concretas.
- Escribe en lenguaje natural. Expresa las directrices con claridad e incluye ejemplos de cómo debería y no debería ser el código.

No hay una única forma correcta de crear archivos de instrucciones, igual que tampoco hay una sola forma correcta de utilizar la IA. La experimentación te ayudará a encontrar el enfoque que mejor se adapte a tu proyecto.

> [!TIP]
> Todos los proyectos que utilizan GitHub Copilot deberían tener un conjunto sólido de archivos de instrucciones. Al explorar los archivos de este proyecto, puede que veas instrucciones para varias tareas, como las [actualizaciones de la interfaz de usuario][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta opción de forma diferente (por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI). El ejercicio correspondiente al entorno indicará cuándo utilizarla.
>
> Si buscas plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas del proyecto

Dedica un momento a leer los archivos de instrucciones incluidos en este repositorio: hay un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la breve descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. Dentro de **Code standards**, observa las directrices anidadas de **GitHub Actions Workflows**. Se aplican a las interacciones con Copilot.
3. Abre la carpeta `.github/instructions` y explora su contenido. Hay instrucciones para archivos de Astro, la capa de datos de Drizzle, las pruebas y otros elementos.
4. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` de la parte superior, que define un patrón global con respecto a la raíz del repositorio y determina a qué archivos se aplican las instrucciones. En este caso, coincide con cualquier archivo de pruebas TypeScript (por ejemplo, los que coincidan con `**/*.test.ts`).
5. Lee las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y señalar ejemplos que Copilot pueda seguir al generar código. (Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio).

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero todavía no exige documentación en el código. En los pasos siguientes añadirás reglas para los comentarios TSDoc y los comentarios de cabecera de los archivos.

## Crea una rama para los cambios

Crea una rama para tus cambios siguiendo las prácticas recomendadas.

1. Vuelve al codespace del ejercicio anterior.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama nueva y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Utiliza Copilot Chat antes de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando sea necesario.

Para observar el efecto de las instrucciones personalizadas, empieza enviando una indicación con el archivo de instrucciones actual. Más adelante, lo actualizarás y volverás a enviar la misma indicación para comparar el resultado.

1. Cierra las pestañas abiertas del editor para que Copilot solo utilice el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para indicar a Copilot dónde debe ubicarse el asistente.
3. Selecciona **Agent** en el selector de agentes de la vista Chat para que Copilot pueda aplicar cambios a los archivos.

   ![Captura de pantalla del selector de agente en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envía esta indicación:

   ```plaintext
   Crea o actualiza src/lib/publishers.ts con una función auxiliar de acceso a datos que devuelva la lista de todos los editores con su nombre e identificador. Aplica los cambios al archivo.
   ```

5. Copilot explora el proyecto y aplica cambios al código, que a menudo abarcan el archivo de la función auxiliar y sus pruebas.
6. Observa que la función auxiliar propuesta es una función tipada que recibe un cliente `db` como primer argumento y devuelve una matriz tipada de editores. Esto se debe a las convenciones de la capa de datos en `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observa que al código propuesto le faltan los comentarios TSDoc y el comentario de cabecera del archivo.

> [!CAUTION]
> Como Copilot es probabilístico, puede que añada comentarios de documentación aunque no se lo pidas. No pasa nada; el objetivo es observar la mejora de coherencia después de actualizar las instrucciones.

## Añade un estándar nuevo para el repositorio

Como se ha indicado, `.github/copilot-instructions.md` proporciona a Copilot información general del proyecto. Añade una directriz a los estándares de programación del repositorio para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Observa que documenta los estándares de programación, pero todavía no incluye reglas para documentar el código; por eso el asistente generado no tenía comentarios.
3. Añade las siguientes líneas Markdown debajo de los estándares existentes. Así indicarás a Copilot que añada comentarios de cabecera de archivo y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en el ejercicio anterior, los archivos de instrucciones se pueden crear en el nivel del repositorio (`.github/copilot-instructions.md`) para proporcionar directrices generales o como archivos `*.instructions.md` para lenguajes, tipos de archivos o tareas concretos. El archivo del repositorio es el lugar adecuado para estándares generales como la regla sobre los comentarios de documentación.

## Repite la indicación y observa el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para vaciar el contexto.
2. Selecciona `src/lib/publishers.ts` para que Copilot se centre en el archivo adecuado.
3. Envía la **misma indicación** que antes:

   ```plaintext
   Crea o actualiza src/lib/publishers.ts con una función auxiliar de acceso a datos que devuelva la lista de todos los editores con su nombre e identificador. Aplica los cambios al archivo.
   ```

4. Observa que el archivo propuesto ahora empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Funciones auxiliares de acceso a datos de editores para la plataforma de financiación colectiva de Tailspin Toys.
    * Proporciona funciones para recuperar información de los editores de la base de datos.
    */
   ```

5. Observa que la función propuesta ahora incluye un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Devuelve una lista de todos los editores con su identificador y nombre.
    *
    * @param db - Cliente de base de datos de Drizzle.
    * @returns Promesa que se resuelve en una matriz de objetos que representan editoriales.
    */
   ```

Has orientado a Copilot para que siga un estándar nuevo del proyecto y lo aplique a código real que utilizarás en el ejercicio siguiente.

## Confirma los cambios de instrucciones y envía la rama

Los archivos de instrucciones se gestionan mediante el control de código fuente como cualquier otro recurso del repositorio. Confirma los cambios y envía la rama.

1. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
2. Desde el terminal, confirma que los cambios en las instrucciones y el asistente estén presentes:

   ```bash
   git status
   ```

3. Desde el terminal, prepara y confirma la actualización de instrucciones y todos los archivos que Copilot haya modificado para el asistente:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra otros cambios relacionados, como pruebas, añádelos también.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envía la rama al repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y combina una solicitud de incorporación de cambios

Ahora que has enviado la rama, crea una solicitud de incorporación de cambios (PR) asociada a la incidencia del estándar de documentación de la lista de trabajo pendiente. Copilot puede buscar la incidencia, crear la PR y combinarla.

> [!NOTE]
> Este es el primer ejercicio en el que Copilot actúa en GitHub en tu nombre. La plantilla del proyecto ya conecta las herramientas de GitHub a tu espacio de trabajo, así que no hace falta configurar nada. La primera vez que Copilot utilice una herramienta, es posible que VS Code te pida que inicies sesión en GitHub. Sigue las indicaciones para permitirlo. En un ejercicio posterior conocerás esta conexión, que utiliza Model Context Protocol.

1. Abre Copilot Chat en tu codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y confirma que **Agent** está seleccionado en el selector de agentes.
3. Pide a Copilot que busque la incidencia relacionada con la actualización de los archivos de instrucciones y que cree una PR desde la rama actual. En la descripción, debe indicar tanto los cambios en las instrucciones como la base de la función auxiliar para editores:

   ```plaintext
   Busca la incidencia relacionada con la actualización del archivo de instrucciones. Crea una PR nueva desde la rama actual, indica que la PR cierra esa incidencia e incluye también la implementación inicial de la función auxiliar para editores, que servirá de base para el próximo trabajo de filtrado.
   ```

4. Copilot buscará la incidencia y creará la PR.
5. Cuando Copilot te pida permiso para realizar acciones de GitHub, revisa el comando y selecciona **Allow** si procede.
6. Cuando se haya creado la PR, pídele a Copilot que la combine y vuelva a cambiar la rama local a `main`:

   ```plaintext
   Combina la PR en main. Después, vuelve a main en local y obtén el código más reciente para que esté actualizado.
   ```

7. Cuando Copilot te pida permiso para realizar acciones de GitHub y ejecutar comandos de shell, revisa el comando y selecciona **Allow** si procede.

¡Ya has creado y combinado una solicitud de incorporación de cambios con GitHub Copilot!

## Resumen y pasos siguientes

Has explorado cómo Copilot utiliza los archivos de instrucciones del proyecto y has usado Copilot Chat en VS Code para:

- explorar cómo llegan a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante instrucciones personalizadas del repositorio y archivos de instrucciones que se aplican a rutas concretas.
- enviar una indicación de generación de código y observar el resultado con las instrucciones *existentes*.
- añadir un estándar nuevo para todo el repositorio en `.github/copilot-instructions.md`.
- repetir la misma indicación y observar cómo el código propuesto incorpora el nuevo estándar.
- confirmar los cambios de instrucciones y la función auxiliar para editores en `main` para que el ejercicio siguiente pueda continuar el trabajo.

A continuación, aplicarás las instrucciones en el [modo de agente][next-lesson] mientras Copilot añade una funcionalidad nueva en el código.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
