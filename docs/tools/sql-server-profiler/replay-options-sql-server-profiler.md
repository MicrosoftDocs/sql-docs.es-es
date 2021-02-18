---
title: Opciones de reproducción
titleSuffix: SQL Server Profiler
description: Explore la configuración que usa SQL Server Profiler al reproducir un seguimiento capturado. Obtenga información sobre cómo usar el cuadro de diálogo Configuración de reproducción para ajustar los valores.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 58761a25-a84f-4a90-9c61-97700bc5ad9c
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: af342941ca797d9054492e59a5004096f901bf69
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353371"
---
# <a name="replay-options-sql-server-profiler"></a>Opciones de reproducción (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Antes de reproducir un seguimiento capturado con [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], puede reproducir las opciones en el cuadro de diálogo **Configuración de reproducción** . Para iniciar este cuadro de diálogo, abra el archivo o tabla de seguimiento en el [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], y en el menú **Reproducir** , haga clic en **Inicio**. Para obtener información acerca de los permisos necesarios para reproducir un seguimiento, vea [Permissions Required to Run SQL Server Profiler](../../tools/sql-server-profiler/permissions-required-to-run-sql-server-profiler.md).  
  
 En este tema se describen las opciones especificadas con el cuadro de diálogo **Configuración de reproducción** .  
  
> [!NOTE]  
>  Recomendamos usar la Utilidad de reproducción distribuida para reproducir una aplicación de OLTP que se use mucho (con muchas conexiones simultáneas activas o un alto rendimiento). La utilidad puede reproducir datos de seguimiento desde varios equipos, simulando mejor una carga de trabajo esencial. Para obtener más información, vea [SQL Server Distributed Replay](../../tools/distributed-replay/sql-server-distributed-replay.md).  
  
## <a name="basic-replay-options"></a>Opciones básicas de reproducción  
 **Servidor de reproducción**  
 El servidor es el nombre de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la que desea reproducir el seguimiento. El servidor debe cumplir los requisitos de reproducción que se describen en [Replay Requirements](../../tools/sql-server-profiler/replay-requirements.md).  
  
 **Guardar en el archivo**  
 El archivo de salida en el que se escriben los resultados de la reproducción del seguimiento para verlos posteriormente. De manera predeterminada, [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] solo muestra en pantalla los resultados de la reproducción del seguimiento.  
  
 **Guardar en la tabla**  
 La tabla de base de datos en la que se escriben los resultados de la reproducción del seguimiento para verlos posteriormente.  
  
 **Número de subprocesos de reproducción**  
 Especifique el número de subprocesos de reproducción que se utilizarán simultáneamente. Un número alto consume más recursos durante la reproducción, pero ésta es más rápida. El orden de los eventos no se mantiene completamente cuando se utilizan varios subprocesos.  
  
 **Reproducir eventos en el orden del seguimiento**  
 Le permite utilizar métodos de depuración, como recorrer paso a paso cada seguimiento. Si no se activa esta opción, la reproducción no garantiza que los eventos se reproduzcan en un orden coherente con el orden en el que se capturaron originalmente.  
  
 **Reproducir eventos mediante múltiples subprocesos**  
 Optimiza el rendimiento y deshabilita la depuración. Los eventos se reproducen en el orden en el que se registraron para un Id. de proceso de servidor (SPID) determinado, pero no se garantiza el orden de los SPID.  
  
 **Mostrar los resultados de la reproducción**  
 Muestra los resultados de la reproducción. Ésta es la opción predeterminada. Si el seguimiento que está reproduciendo es muy grande, puede que le interese deshabilitar la opción para ahorrar espacio en disco.  
  
> [!NOTE]  
>  Para conseguir un rendimiento de reproducción óptimo, se recomienda seleccionar la opción de reproducir eventos con múltiples subprocesos y no seleccionar la opción de mostrar los resultados de la reproducción.  
  
## <a name="advanced-replay-options"></a>Opciones avanzadas de reproducción  
 **Reproducir los SPID del sistema**  
 Reproduce los SPID del sistema Ésta es la opción predeterminada.  
  
 **Reproducir solo un SPID**  
 Reproduce el número de SPID que elija en la lista.  
  
 **Limitar reproducción por fecha y hora**  
 Reproduce el seguimiento para la **Hora de inicio** y la **Hora de finalización** especificadas.  
  
 **Intervalo de espera del monitor de estado**  
 Establece la cantidad de tiempo que se permite la ejecución de un proceso antes de que el monitor de estado lo finalice.  
  
 **Intervalo de sondeo del monitor de estado**  
 Establece la frecuencia con la que el monitor de estado sondea los procesos que puede tener que finalizar.  
  
 **Habilitar monitor de procesos bloqueados de SQL Server**  
 Establece la frecuencia con la que el monitor de procesos bloqueados busca procesos bloqueados o de bloqueo.  
  
## <a name="about-the-health-monitor"></a>Acerca del monitor de estado  
 El monitor de estado es un subproceso de aplicación que supervisa los procesos simulados que participan en la reproducción de un seguimiento y finaliza los procesos que están bloqueados en la reproducción. En la pestaña **Opciones avanzadas de reproducción** del cuadro de diálogo **Configuración de reproducción** , puede especificar cuánto tiempo en segundos debe esperar el monitor de mantenimiento antes de finalizar un proceso bloqueado (**Intervalo de espera del monitor de mantenimiento**). Si el intervalo se configura en 0, el monitor de estado nunca finaliza los procesos de bloqueo simulados en el seguimiento de reproducción.  
  
## <a name="see-also"></a>Consulte también  
 [Reproducir seguimientos](../../tools/sql-server-profiler/replay-traces.md)   
 [Requisitos de reproducción](../../tools/sql-server-profiler/replay-requirements.md)   
 [Consideraciones para reproducir seguimientos &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/considerations-for-replaying-traces-sql-server-profiler.md)  
  
  
