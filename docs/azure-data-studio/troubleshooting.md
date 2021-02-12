---
title: Solución de problemas de Azure Data Studio
description: Obtenga información sobre cómo obtener registros y solucionar problemas de Azure Data Studio, lo que resulta útil para notificar informes de errores.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: dzsquared
ms.author: drskwier
ms.reviewer: hanqin, maghan
ms.custom: seodec18
ms.date: 11/24/2020
ms.openlocfilehash: 81fb4957672fa2008582b0ab2397322fb2b30043
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039975"
---
# <a name="azure-data-studio-troubleshooting"></a>Solución de problemas de Azure Data Studio
Azure Data Studio realiza un seguimiento de los problemas y las solicitudes de características mediante un [rastreador de problemas de repositorios de GitHub](https://github.com/Microsoft/azuredatastudio/issues) para el repositorio `azuredatastudio`. 

## <a name="if-youve-experienced-any-issue"></a>Si ha experimentado algún problema

Informe de los problemas en el [rastreador de problemas de GitHub](https://github.com/Microsoft/azuredatastudio/issues) y comuníquenos los detalles que nos permitan reproducir el error. Incluya cualquier [información de registro](#how-to-set-the-logging-level) del archivo de registro.

## <a name="writing-good-bug-reports-and-feature-requests"></a>Redacción de buenos informes de errores y solicitudes de características

Presente una sola incidencia por problema y solicitud de características.

* No enumere varios errores o solicitudes de características en la misma incidencia.
* No agregue la incidencia como comentario a una incidencia existente, a menos que se trate de una entrada idéntica. Muchos problemas son similares, pero tienen diferentes causas.

Cuanta más información pueda proporcionar, más probable será que alguien reproduzca correctamente el problema y encuentre una solución. 

Incluya la siguiente información con cada incidencia:

* Versión de Azure Data Studio
* Pasos reproducibles (1... 2... 3...) y lo que esperaba ver en comparación con lo que apareció realmente. 
* Imágenes, animaciones o un vínculo a un vídeo. Las imágenes y las animaciones ilustrarán los pasos de reproducción, pero no los reemplazarán.
* Un fragmento de código que demuestre el problema o un vínculo a un repositorio de código que podamos descargar fácilmente en nuestro equipo para recrear el problema. 

> [!NOTE]
>  Dado que es necesario copiar y pegar el fragmento de código, la inclusión de un fragmento de código como archivo multimedia (es decir, .gif) no es suficiente. 

* Errores en la consola de herramientas de desarrollo (Ayuda | Alternar herramientas de desarrollo)

Recuerde que debe hacer lo siguiente:

* Busque en el repositorio de problemas para ver si ya existe uno igual. 
* Simplifique el código relacionado con la incidencia, de modo que podamos aislarla mejor. 

¡No se sienta mal si no podemos reproducir el problema y solicite más información!

## <a name="how-to-set-the-logging-level"></a>Establecimiento del nivel de registro

### <a name="azure-data-studio"></a>Azure Data Studio
Ejecute el comando `Developer: Set Log Level...` para seleccionar el nivel de registro de la sesión actual. Este valor NO se conserva entre varias sesiones, por lo que si reinicia Azure Data Studio, volverá al nivel predeterminado `Info`. 

Si desea habilitar el registro de depuración para el inicio, establezca el nivel de registro en `Debug` y ejecute el comando `Developer: Reload Window`.

### <a name="mssql-built-in-extension"></a>MSSQL (extensión integrada)

Si el parámetro de usuario `Mssql: Log Debug Info` está establecido en true, la información de registro de depuración se enviará al canal de salida `MSSQL`.

El parámetro de usuario `Mssql: Tracing Level` se utiliza para controlar el nivel de detalle del registro.

## <a name="debug-log-location"></a>Ubicación del registro de depuración
En Azure Data Studio, ejecute el comando `Developer: Open Logs Folder` para abrir la ruta de acceso a los registros. Hay muchos tipos diferentes de archivos de registro que se escriben en esta ubicación, algunos de los más usados son los siguientes:

1. `renderer#.log` (por ejemplo, renderer1.log): este archivo es el archivo de registro del proceso principal.
1. `telemetry.log`: cuando el nivel de registro se establece en `Trace` este archivo contendrá los eventos de telemetría enviados por Azure Data Studio.
1. `exthost#/exthost.log`: archivo de registro para el proceso de host de extensiones (solo es el propio proceso, no las extensiones que se ejecutan en él).
1. `exthost#/Microsoft.mssql`: registros de la extensión MSSQL, que contiene gran parte de la lógica principal de las características relacionadas con MSSQL.
   * sqltools.log es el registro de QL Tools Service.
1. `exthost#/output_logging_#######`: estas carpetas contienen los mensajes mostrados en el panel `Output` de Azure Data Studio. Cada archivo se denomina `#-<Channel Name>`; así, por ejemplo, el canal de salida `Notebooks` puede generar un archivo denominado `3-Notebooks.log`.

Si se le pide que proporcione registros, comprima toda la carpeta para asegurarse de que se incluyen los registros correctos. 

## <a name="next-steps"></a>Pasos siguientes
- [Notificar un problema](https://github.com/Microsoft/azuredatastudio/issues)
- [Qué es Azure Data Studio](what-is-azure-data-studio.md)