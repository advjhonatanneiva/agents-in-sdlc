---
title: "Lección 12 - Publicar el sitio con GitHub Pages"
description: "Usa la aplicación GitHub Copilot para configurar un flujo de trabajo de Pages, revisar su solicitud de incorporación de cambios y comprobar que Space Quiz está disponible."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Publica Space Quiz con GitHub Pages para que todo el mundo pueda acceder a él desde una dirección web pública (URL). Copilot puede preparar el flujo de trabajo y guiarte durante la publicación, pero eres responsable de revisar los cambios y confirmar que el sitio está disponible.

En esta lección:

- pedirás a Copilot que configure la publicación con GitHub Pages.
- revisarás el flujo de trabajo y la solicitud de incorporación de cambios antes de combinarla.
- seguirás el proceso de publicación y comprobarás el sitio publicado.

## Escenario

Space Quiz ya está en un repositorio de GitHub, pero hace falta una forma sencilla de probarlo. Publicar el sitio de un solo archivo con GitHub Pages permite acceder a él desde una URL pública sin añadir un servidor ni una dependencia del entorno de ejecución.

## Pide a Copilot que prepare la publicación

> [!WARNING]
> Crear una solicitud de incorporación de cambios puede confirmar y enviar código. Revisa la configuración de aprobación y el repositorio de destino antes de enviar esta indicación. Si has seleccionado **Approve all**, no esperes otro aviso de confirmación.

Usa la sesión de la aplicación Copilot para `space-quiz` y pide que prepare la publicación sin autorizar todavía la combinación ni el despliegue.

1. Envía esta indicación a Copilot:

   ```plaintext
   Prepara la publicación de este repositorio con GitHub Pages. Crea un flujo de trabajo de GitHub Actions que compile el sitio web si es necesario y lo publique en GitHub Pages cuando los cambios lleguen a la rama predeterminada. Añade un README que describa la aplicación y cómo ejecutarla en local, si todavía no existe. Crea una solicitud de incorporación de cambios con estos cambios e indica su enlace y cualquier configuración o permiso necesario de Pages. No combines la solicitud, no habilites la combinación automática, no cambies la configuración de Pages ni publiques el sitio. Detente y espera a que lo revise.
   ```

## Revisa el flujo de trabajo y la solicitud de incorporación de cambios

Antes de autorizar la combinación, confirma que el flujo de trabajo solo publique los cambios revisados desde la rama predeterminada.

1. Confirma que el README solo se ha añadido si el repositorio no tenía ya uno.
2. Revisa los cambios de la solicitud, incluidos los pasos de compilación y publicación del flujo de trabajo, sus desencadenadores y permisos.
3. Confirma que el flujo de trabajo publica desde la rama predeterminada, no desde una solicitud de incorporación de cambios que todavía no se ha combinado.
4. Solicita las correcciones necesarias y revisa las diferencias actualizadas. Espera a que se superen las comprobaciones obligatorias de la solicitud antes de autorizar la combinación.

## Autoriza la combinación y la publicación

> [!WARNING]
> La indicación siguiente autoriza al agente a combinar la solicitud revisada, cambiar la configuración de Pages del repositorio y publicar el sitio. Envíala solo después de revisar los cambios y confirmar el repositorio y la solicitud. Es posible que la configuración de aprobación permita estas acciones sin otro aviso.

Autoriza la combinación y la publicación únicamente después de revisar los cambios, las comprobaciones y el repositorio de destino.

1. Sustituye `<PR_NUMBER>` por el número de la solicitud que has revisado y envía esta indicación por separado:

   ```plaintext
   He revisado la solicitud de incorporación de cambios <PR_NUMBER> de este repositorio y autorizo su combinación y publicación. Habilita GitHub Pages mediante GitHub Actions, combina únicamente esa solicitud cuando se hayan superado las comprobaciones obligatorias y supervisa la publicación hasta que termine correctamente. Indica la URL final del sitio web publicado. Si no tienes permiso para habilitar Pages o combinar la solicitud, dime exactamente qué debo aprobar o hacer. No indiques que ha terminado correctamente hasta que el sitio esté disponible.
   ```

![Diagrama en tres fases: una solicitud de space-quiz combinada; una ejecución correcta de Deploy website en GitHub Actions con los pasos Build website, Upload artifact y Deploy to Pages; y Space Quiz publicado en yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

## Comprueba la publicación

Comprueba tanto el resultado del flujo de trabajo como el sitio publicado antes de dar la tarea por terminada.

1. Sigue la ejecución de publicación de GitHub Actions hasta que termine correctamente.
2. Abre la URL indicada y confirma que Space Quiz se carga.

Si el agente no puede habilitar Pages ni combinar la solicitud, sigue las instrucciones concretas que te dé. No des la tarea por terminada hasta que la publicación se complete y el sitio esté disponible.

## Resumen y pasos siguientes

Space Quiz ya está disponible como sitio publicado y has comprobado la publicación en lugar de confiar únicamente en el informe del agente. Continúa con la [lección 13: Revisión y pasos siguientes][next-lesson].

[next-lesson]: ../13-review/
