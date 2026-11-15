---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores has pedido al agente en la nube de Copilot que se encargue de tres tareas distintas para mejorar la experiencia de usuario y añadir funciones. Aunque el agente en la nube trabaja de forma autónoma y asíncrona, sigue siendo importante supervisar las tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente en la nube, como la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver las tareas de los agentes que tienen solicitudes de incorporación de cambios abiertas. También puedes explorar las operaciones realizadas y orientar las sesiones que sigan en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- orientarás una sesión en curso para pedir una funcionalidad adicional.

## Escenario

Después de asignar al agente la implementación del modo de contraste alto, el equipo se dio cuenta de que también sería un buen momento para añadir el modo claro. Como ya se estaba trabajando en los estilos del sitio y en un selector, parecía razonable incluir esta función. Orientarás el trabajo del agente para que añada el modo claro además del contraste alto.

## Revisa las tareas del agente en la nube de Copilot

Comprueba el estado de las tareas asignadas al agente en la nube de Copilot.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas del panel principal y del panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación del código base.
   - generar interfaces de programación de aplicaciones (API) para modificar juegos.
   - añadir un modo de contraste alto al sitio web.
3. Selecciona una tarea en curso y revisa las acciones que ha realizado Copilot. Pueden incluir:
   - obtener el código del repositorio.
   - crear el entorno de trabajo de Copilot.
   - configurar servidores MCP.
   - realizar distintas tareas para completar el trabajo asignado.

> [!NOTE]
> Los pasos concretos varían según el estado del trabajo de Copilot y el enfoque que haya utilizado.

4. Observa también el panel de solicitudes de incorporación de cambios (PR), que aparece a la derecha. Desde allí puedes ver la PR y los archivos modificados para supervisar el trabajo.

## Dirige el agente en la nube

Ya has visto las tareas activas. Ahora pide a Copilot que añada un selector de modo claro mientras trabaja en el modo de contraste alto.

1. Selecciona la sesión relacionada con el modo de contraste alto. El título exacto dependerá del nombre que utilice Copilot y del estado del trabajo.

    ![Sesión de accesibilidad en mission control](../../../_images/ex6-accessibility-session.png)

2. Supervisa la sesión durante unos minutos hasta que indique que ha terminado de configurar el entorno y ha empezado a trabajar. Verás mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe esta indicación:

    ```plaintext
    Mientras estamos trabajando en el modo de contraste alto, añadamos también un modo claro. Debe haber un selector para este modo, de forma que los usuarios puedan elegir la visualización que prefieran.
    ```

    ![Captura de pantalla de la tarea del agente en la nube en la página de agentes, con el cuadro de diálogo para dirigir una sesión activa de Copilot resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar la indicación.
5. Observa cómo Copilot la confirma y la incorpora al flujo de trabajo.

## Deja que Copilot trabaje

Copilot continuará con la tarea actualizada. Incorporará la nueva solicitud cuando termine el paso que estaba realizando en el momento en que enviaste el mensaje.

Como antes, el trabajo tardará varios minutos. Aprovecha para hacer una pausa y reflexionar sobre lo que has aprendido y explorado hasta ahora.

## Resumen y pasos siguientes

En esta lección has explorado la página de agentes de Copilot, el centro para supervisar y orientar las tareas del agente en la nube de GitHub Copilot. Permite controlar las tareas activas y completadas, revisar el trabajo y redirigir las tareas en curso para ajustar su alcance o añadir instrucciones.

Has explorado estos conceptos:

- has explorado mission control y la página de agentes para supervisar las tareas del agente en la nube.
- has redirigido una sesión en curso para pedir una función adicional.

Mientras Copilot termina el trabajo en las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Lección siguiente: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
