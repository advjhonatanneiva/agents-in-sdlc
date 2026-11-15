---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando utilizas el modo de agente de GitHub Copilot, este trabaja de forma autónoma: explora el código base, propone cambios, edita archivos y ejecuta comandos. Como el trabajo se realiza en tu equipo y en tiempo real, Copilot Chat en VS Code muestra cada llamada a una herramienta y cada edición de archivo a medida que ocurren. Puedes revisar las diferencias en el editor, aceptar o rechazar cambios concretos y orientar la conversación con indicaciones de seguimiento para ajustar o ampliar el trabajo sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás los cambios de archivo propuestos.
- orientarás al agente durante la sesión para que añada un selector de modo claro junto al de contraste alto.

## Escenario

El agente personalizado de accesibilidad que has utilizado en el [ejercicio 4][previous-lesson] está implementando el modo de contraste alto. Mientras trabaja, el equipo de diseño ha solicitado otra función: un selector de modo claro para los usuarios a quienes les resulta demasiado oscuro el tema predeterminado. En lugar de iniciar una conversación desde cero, orientarás la sesión existente para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra todos los pasos del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en el chat con las diferencias y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver cada llamada a una herramienta. Aparecerán lecturas y ediciones de archivos y los comandos de terminal que solicite ejecutar.
3. Cuando el agente proponga una edición, las diferencias aparecerán en el chat:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertirlo y permitir que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** de la parte superior del chat para ver todos los archivos modificados en esta sesión.

> [!NOTE]
> Si una llamada a una herramienta requiere permiso, por ejemplo, para ejecutar un comando de terminal, el agente se detendrá y mostrará una solicitud **Allow**. Aprueba únicamente los comandos que consideres seguros.

5. Espera a que el agente termine de implementar el modo de contraste alto. Al final debería mostrar un resumen de los cambios.

## Pide al agente que añada el modo claro

Ahora que ya está implementado el modo de contraste alto, ampliarás la misma conversación para añadir un selector de modo claro. Como continuarás en la misma sesión de chat, el agente conservará el contexto del trabajo anterior, incluidos los componentes modificados, el enfoque de estilo y el patrón de almacenamiento local.

1. Envía esta indicación de seguimiento en Copilot Chat:

   ```plaintext
   Buen trabajo. Ahora añade un selector de modo claro junto al selector de contraste alto. El modo claro debe seguir el mismo patrón: guardar la configuración en local storage y estar disponible en la misma zona de la interfaz de usuario. El usuario debe poder activar el modo claro y el de contraste alto de forma independiente.
   ```

2. El agente continuará desde donde lo dejó, explorará los cambios realizados y propondrá modificaciones para el modo claro.
3. Revisa cada cambio nuevo cuando aparezca, igual que con el modo de contraste alto. Usa **Keep** o **Undo** en cada diferencia.
4. Si el agente toma un rumbo que no quieres —por ejemplo, intenta reorganizar la interfaz de los selectores de una forma que no te convence—, detenlo con el botón **Stop** junto al campo de chat y envíale una indicación de seguimiento para aclarar lo que necesitas.

> [!TIP]
> Dirigir una sesión en curso es más rápido que empezar de nuevo. Utiliza indicaciones de seguimiento para pedir ajustes («simplifica el CSS y usa variables CSS en lugar de duplicar selectores»), corregir el rumbo («no añadas un tercer selector; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también métodos abreviados de teclado para los dos selectores»).

## Revisa los cambios

Antes de confirmar el trabajo, revisa rápidamente todos los cambios realizados por el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en los componentes de Astro, los estilos y las pruebas relacionadas.
3. Abre algunos archivos modificados y revisa las diferencias. Confirma que se aplican los patrones de accesibilidad del agente personalizado: atributos de aplicaciones de Internet enriquecidas y accesibles (ARIA), navegación por teclado, HTML semántico y almacenamiento en local storage.
4. Cuando estés conforme, prepara y confirma los cambios en el panel Source Control. Los publicarás en [la lección siguiente][next-lesson].

## Resumen y pasos siguientes

Has utilizado el modo de agente de Copilot Chat para supervisar y dirigir una sesión en curso: has revisado cada edición, aceptado o rechazado cambios y enviado indicaciones de seguimiento para ampliar el trabajo sin perder el contexto.

En esta lección has explorado:

- la supervisión del trabajo de un agente en tiempo real.
- la aceptación y el rechazo de ediciones de archivos concretas.
- la orientación de una sesión en curso mediante indicaciones de seguimiento.

En [la lección siguiente][next-lesson], publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios realizados por el agente personalizado local durante el taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo de agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Lección siguiente: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
