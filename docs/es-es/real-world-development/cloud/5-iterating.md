---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Supervisar y gestionar agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funciones. Has pedido a Copilot que documente el código, cree una función de juegos relacionados para que el equipo de diseño pueda probarla e implemente funciones de accesibilidad, incluidos los modos de contraste alto y claro. Revisa los cambios de código y, si hace falta, pide a Copilot que mejore el resultado.

### Escenario

Como se ha señalado varias veces, la IA generativa no cambia los fundamentos del diseño de software ni de DevOps. Hay que revisar el código generado y seguir el proceso habitual de DevOps. En este ejercicio revisarás las propuestas de GitHub Copilot para documentar el código, añadir juegos relacionados e implementar las funciones de accesibilidad antes de activar la revisión para el resto del equipo.

## Seguridad y el agente en la nube de GitHub Copilot

El agente en la nube de Copilot trabaja de forma autónoma y asíncrona, por lo que se aplican algunas restricciones de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utiliza para el código.
- El agente en la nube se ejecuta en GitHub Actions, donde crea un entorno independiente y efímero para trabajar.
- Una persona debe aprobar los flujos de trabajo de GitHub Actions antes de que puedan ejecutarse.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores de Model Context Protocol (MCP).

## Revisa la documentación generada

Empieza por explorar la primera solicitud de incorporación de cambios (PR) creada por el agente en la nube de GitHub Copilot: la que añade documentación al código. Utilizarás la interfaz estándar de PR de GitHub.com.

> [!NOTE]
> Es posible que veas un aviso de que un firewall ha bloqueado a GitHub Copilot. Es **esperable**: el acceso del agente a recursos externos está limitado de forma predeterminada, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el firewall del agente en la nube de Copilot][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de solicitudes.
3. Abre la solicitud con un título parecido a **Add missing documentation** u otro más específico.

> [!NOTE]
> Si Copilot sigue trabajando, el título de la solicitud incluirá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar varios minutos; puedes hacer una pausa o repasar lo que has aprendido.

4. Cuando la solicitud esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluirá nuevos comentarios de documentación de TypeScript (TSDoc) y otra documentación. Los cambios concretos pueden variar.

    Al revisar los cambios, busca comentarios TSDoc, convenciones de TypeScript y comentarios de cabecera. Proceden de los archivos de instrucciones personalizadas que has revisado al principio del ejercicio 2.

6. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un aviso de que algunos flujos de trabajo están pendientes de aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo aparecerán en cola en la sección de comprobaciones de la PR. Si todo va bien, se superarán las comprobaciones del proyecto para la aplicación Astro. El proceso puede tardar varios minutos.

## Pide cambios a GitHub Copilot

La colaboración con Copilot en una solicitud no es unidireccional. Puedes mencionarlo en comentarios de la PR o en comentarios insertados en el código, como harías con otros miembros del equipo. Copilot verá el comentario e iniciará otra sesión para resolverlo. Como los resultados no son deterministas, no podemos recomendar una indicación exacta. Por ejemplo, podrías pedirle que:

- añada al principio de cada archivo de código comentarios que describan brevemente su función.
- añada comentarios TSDoc a los archivos TypeScript y Astro.
- cree un README con una descripción de la estructura de la aplicación Astro.

1. Añade un comentario para solicitar un cambio en la documentación generada y menciona a **@copilot** como lo harías con cualquier otro usuario. Usa una de las ideas anteriores u otra sugerencia sobre la documentación del código base.
2. Selecciona **View Session** para observar el trabajo de Copilot. Verás que inicia una sesión nueva para realizar los cambios.
3. Puedes seleccionar **Back to pull request** para volver a la solicitud.

    ![Botón Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine los cambios, aparecerá un commit nuevo en la PR.
5. Selecciona la pestaña **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme.

6. Cuando esté todo listo, marca la PR como lista para revisión y combínala en `main`.

![Marcar una PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la función de juegos relacionados

Vuelve a la PR que Copilot ha creado para resolver la incidencia sobre los juegos relacionados en la página de detalles.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR con un título parecido a **Show related games on the game details page** u otro más específico.
4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Deberías ver un aviso de que algunos flujos de trabajo están pendientes de aprobación.
7. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Los flujos de trabajo aparecerán en cola en la sección de comprobaciones de la PR. Si todo va bien, se superarán las comprobaciones del proyecto para la aplicación Astro. El proceso puede tardar varios minutos.
9. **Opcional:** puedes cambiar a esta rama en el codespace para probar manualmente la función de juegos relacionados. Ve al codespace, abre el terminal y ejecuta estos comandos. Sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la función de juegos relacionados. Como antes, puedes iterar con el agente en la nube de Copilot y pedirle que haga cambios. Por ejemplo, puedes modificar el número de juegos relacionados que se muestran o pedir que añada comentarios de cabecera y comentarios TSDoc (recuerda que la incidencia se asignó **antes** de actualizar las instrucciones personalizadas). Para ello, añade un comentario en la pestaña **Conversation**; Copilot lo verá e iniciará una sesión nueva.

## Revisa las funciones de accesibilidad

Por último, revisa las funciones implementadas con el agente personalizado de accesibilidad. La PR debería incluir el modo de contraste alto que has asignado en el ejercicio 3 y el modo claro que has solicitado desde mission control en el ejercicio 4.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR con un título parecido a **Add high contrast mode to website** u otro más específico.

> [!NOTE]
> Si Copilot sigue trabajando, el título de la solicitud incluirá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar varios minutos.

4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Revisa la implementación y presta atención a:
   - los componentes de interfaz para cambiar de modo.
   - el uso de local storage para guardar las preferencias del usuario.
   - los cambios de CSS o de estilo para los modos de contraste alto y claro.
   - los atributos de accesibilidad (atributos de aplicaciones de Internet enriquecidas y accesibles (ARIA), navegación por teclado, etc.).
   - el código JavaScript/TypeScript que gestiona el cambio de modo.

6. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un aviso de que algunos flujos de trabajo están pendientes de aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo aparecerán en cola en la sección de comprobaciones de la PR. Si todo va bien, se superarán las comprobaciones del proyecto para la aplicación Astro. El proceso puede tardar varios minutos.
10. **Opcional:** puedes cambiar a esta rama en tu codespace para probar manualmente las funciones de accesibilidad. Ve al codespace, abre el terminal y ejecuta estos comandos. Sustituye `<branch-name>` por el nombre de la rama creada por Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba los selectores de contraste alto y modo claro en el navegador. Confirma que funcionan y que la configuración se conserva al volver a cargar las páginas.

Observa cómo el agente personalizado de accesibilidad ha orientado a Copilot para implementar estas funciones siguiendo las prácticas recomendadas de accesibilidad. Si detectas problemas o mejoras posibles, puedes mencionarlo en un comentario con **@copilot** y pedirle cambios, como en las PR anteriores.

## Ejercicio opcional: sigue delegando

El agente en la nube resulta útil cuando puedes asignarle incidencias reales de la lista de trabajo pendiente y ocuparte de otras tareas. Para practicarlo, crea algunas incidencias más en el repositorio y asígnaselas a Copilot. Algunas ideas:

- crear un formulario para registrar el interés de los patrocinadores en la página de detalles del juego.
- implementar la paginación en la página de lista de juegos.
- añadir validación de entradas y gestión de errores a los asistentes de acceso a datos.

## Resumen

Has completado el recorrido del agente en la nube. En estas lecciones:

- **Has inspeccionado los archivos de instrucciones personalizados del repositorio** para observar más adelante su efecto en el trabajo del agente en la nube.
- **Has asignado incidencias al agente en la nube de Copilot** y has visto cómo configura el entorno, planifica y trabaja de forma asíncrona.
- **Has creado y utilizado un agente personalizado** de accesibilidad para añadir los modos de contraste alto y claro.
- **Has utilizado la página de agentes de Copilot como centro de control** para supervisar y dirigir la sesión de accesibilidad mientras estaba en curso.
- **Has revisado las solicitudes de incorporación de cambios del agente en la nube y has iterado sobre ellas**, mencionando a `@copilot` para solicitar cambios y aprobando flujos de trabajo.

## Revisión y pasos siguientes

Has completado el recorrido del agente en la nube. Si quieres seguir explorando, los otros recorridos complementan lo que has practicado:

- 🖥️ **[Recorrido de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración de MCP directamente en el IDE.
- 💻 **[Recorrido de CLI](../../cli/)**: entrega desde el terminal dos cambios revisados con los modos Plan y Autopilot, habilidades de agente, un agente QA personalizado, MCP de Playwright, Agent Merge y una integración opcional con el agente en la nube mediante `/delegate`.

En tu propio repositorio, prueba estas ideas:

- asigna al agente en la nube una incidencia de refactorización o de cobertura de pruebas.
- crea un agente personalizado para otro ámbito.
- configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube de Copilot][cloud-agent-best-practices]
- [Configurar el firewall del agente en la nube de Copilot][agent-firewall]

---

| [← Lección anterior: Supervisar y gestionar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
