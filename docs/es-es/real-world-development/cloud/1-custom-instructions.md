---
title: "Ejercicio 1 - Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Lección siguiente: Agente en la nube de Copilot →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta o Copilot necesita información adicional, conviene proporcionarle ese contexto. Los [archivos de instrucciones][instruction-files] permiten transmitir esas directrices para que Copilot entienda no solo *qué* quieres que haga, sino también *cómo* quieres que lo haga.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante instrucciones personalizadas del repositorio y archivos de instrucciones que se aplican a rutas concretas.
- añadirás un estándar nuevo para todo el repositorio en `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los recorridos de VS Code y CLI, aquí no compararás los resultados de una indicación antes y después del cambio. El agente en la nube de Copilot trabaja de forma asíncrona en incidencias de GitHub, por lo que resulta más difícil mostrar su efecto en tiempo real. Verás cómo influye el archivo de instrucciones cuando revises las solicitudes de incorporación de cambios que genere el agente en los ejercicios posteriores.

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

- `.github/copilot-instructions.md` es un único archivo de instrucciones que se envía a Copilot en todas las solicitudes del repositorio. Debe contener información del proyecto relevante para la mayoría de las solicitudes, como la pila tecnológica, una descripción general de lo que se está creando, las prácticas recomendadas y otras directrices generales.
- Los archivos `.github/instructions/*.instructions.md` pueden crearse para tareas o tipos de archivos concretos. Permiten proporcionar directrices para lenguajes concretos, como TypeScript o Astro, o para tareas como crear un componente de interfaz de usuario o un conjunto nuevo de pruebas unitarias.

> [!NOTE]
> En un entorno de desarrollo integrado (IDE), los archivos de instrucciones solo se utilizan para generar código en Copilot Chat, no para las sugerencias de código ni las sugerencias de edición siguiente.
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
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y señalar ejemplos que Copilot pueda seguir. (Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio).

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero todavía no exige documentación en el código. En los pasos siguientes añadirás reglas para los comentarios TSDoc y los comentarios de cabecera de los archivos.

## Añade un estándar nuevo para el repositorio

Este es el único cambio que realizarás en este ejercicio: añadirás una regla general para que la documentación se incluya en el código mediante comentarios TSDoc y un comentario de cabecera del archivo. El agente en la nube utilizará esta regla al trabajar en las incidencias que le asignes más adelante.

Antes de editar el archivo, crea una rama. El agente en la nube leerá las instrucciones de la rama de destino de la incidencia, pero confirmarás los cambios en una rama propia:

1. Desde el terminal del codespace, crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

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

## Confirma, envía y combina la actualización de instrucciones

El agente en la nube lee los archivos de instrucciones de la rama a la que apunta la incidencia. En el siguiente ejercicio, asignarás una incidencia a Copilot y este creará una rama a partir de `main`. Por eso, los cambios en las instrucciones deben llegar a `main` para que el agente los utilice.

1. Prepara y confirma los cambios:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envía la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de incorporación de cambios de `custom-instructions-cloud` a `main` y combínala. Así, el agente en la nube podrá utilizar estas instrucciones cuando le asignes incidencias en el siguiente ejercicio.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante este taller, puedes omitir los pasos de la rama y confirmar los cambios directamente en `main`. La rama permite practicar un flujo de trabajo habitual en un proyecto real.

## Resumen y pasos siguientes

Has explorado cómo Copilot utiliza los archivos de instrucciones del proyecto y has añadido un estándar nuevo para todo el repositorio en `.github/copilot-instructions.md`. En este ejercicio:

- has explorado cómo llegan a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante instrucciones personalizadas del repositorio y archivos de instrucciones que se aplican a rutas concretas.
- has añadido un estándar nuevo para todo el repositorio en `.github/copilot-instructions.md`.

Durante el resto de este recorrido verás cómo el agente en la nube aplica ese estándar en las solicitudes de incorporación de cambios que genera.

A continuación, [asignarás tu primera incidencia al agente en la nube de Copilot][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
