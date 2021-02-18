---
title: Reproducir hasta un cursor
titleSuffix: SQL Server Profiler
description: Obtenga información sobre cómo usar un cursor en SQL Server Profiler para pausar una reproducción de seguimiento en un punto específico. Reproduzca hasta un cursor para facilitar la depuración.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 89eadc41-4424-4a1c-ba61-0b52c851cdb1
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: a903e7176b4715dcf54db8a55b28a280a825b3bf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353357"
---
# <a name="replay-to-a-cursor-sql-server-profiler"></a>Reproducir hasta un cursor (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

En este tema se describe cómo reproducir archivos o tablas de seguimiento que se ponen en pausa al alcanzar un cursor mediante el [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]. La pausa de seguimientos en cursores es compatible con la depuración porque la reproducción de scripts de seguimiento largos se puede dividir en segmentos cortos que se pueden analizar de forma incremental.  
  
### <a name="to-replay-to-the-cursor"></a>Para reproducir hasta el cursor  
  
1.  Abra el archivo o la tabla de seguimiento que desea reproducir. Para obtener más información, vea [Abrir un archivo de seguimiento &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/open-a-trace-file-sql-server-profiler.md) o el Asistente para la optimización del [Abrir una tabla de seguimiento &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/open-a-trace-table-sql-server-profiler.md).  
  
     Asegúrese de que el archivo o la tabla de seguimiento que abre contiene las clases de evento necesarias para la reproducción. Para más información, consulte [Replay Requirements](../../tools/sql-server-profiler/replay-requirements.md).  
  
2.  En la ventana de seguimiento, haga clic en un evento.  
  
3.  En el menú **Reproducir** , haga clic en **Ejecutar hasta el cursor** y, a continuación, conéctese al servidor en el que desea reproducir el seguimiento.  
  
4.  Compruebe la configuración en el cuadro de diálogo **Configuración de reproducción** y haga clic en **Aceptar**.  
  
     Se inicia la reproducción, que se pone en pausa al alcanzar el primer cursor.  
  
5.  Presione F5 para reanudar el seguimiento.  
  
6.  Repita el paso 5 hasta el final del seguimiento.  
  
## <a name="see-also"></a>Consulte también  
 [Reproducir hasta un punto de interrupción &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/replay-to-a-breakpoint-sql-server-profiler.md)   
 [Reproducir seguimientos](../../tools/sql-server-profiler/replay-traces.md)   
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)  
  
  
