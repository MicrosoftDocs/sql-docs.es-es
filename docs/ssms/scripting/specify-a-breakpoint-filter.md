---
title: Especificar un filtro del punto de interrupción
description: Aprenda a implementar un filtro de punto de interrupción para limitar el punto de interrupción para que solo actúe cuando se activa la depuración en los equipos, procesos del sistema operativo y subprocesos especificados.
titleSuffix: T-SQL debugger
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Transact-SQL debugger, breakpoint filter
ms.assetid: 7bf1dddd-7b0b-4c47-8a7b-28a5569b4fa5
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 12/04/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ad78112c5580bcf9f7a08dd80228fad3e3582f35
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344136"
---
# <a name="specify-a-breakpoint-filter"></a>Especificar un filtro del punto de interrupción

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Un filtro del punto de interrupción limita el punto de interrupción de manera que solo actúe en los equipos, procesos del sistema operativo y subprocesos especificados. Los filtros del punto de interrupción suelen utilizarse al depurar aplicaciones en paralelo.

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]
  
##  <a name="filter-considerations"></a><a name="BKMK_ActionConsiderations"></a> Consideraciones de filtrado

Los filtros del punto de interrupción no suelen utilizarse con el depurador de [!INCLUDE[tsql](../../includes/tsql-md.md)] porque los scripts y los procedimientos almacenados de [!INCLUDE[tsql](../../includes/tsql-md.md)] no son aplicaciones en paralelo.  
  
#### <a name="to-specify-a-breakpoint-filter"></a>Para especificar un filtro del punto de interrupción  
  
1.  En la ventana del editor, haga clic con el botón derecho en el glifo de punto de interrupción y, después, haga clic en **Filtrar** en el menú contextual.  
  
     O bien  
  
     En la ventana **Puntos de interrupción** , haga clic con el botón derecho en el glifo de punto de interrupción y, después, haga clic en **Filtrar** en el menú contextual.  
  
2.  En el cuadro de diálogo **Filtros del punto de interrupción** , utilice el cuadro **Filtro** para especificar los equipos por nombre, o los procesos del sistema operativo y los subprocesos por nombre o por número de identificación:  
  
    -   **MachineName** es el equipo que ejecuta la instancia del Motor de base de datos.  
  
    -   **ProcessID** y **ProcessName** son el proceso del sistema operativo que ejecuta la instancia del Motor de base de datos.  
  
    -   **ThreadID** y **ThreadName** son el subproceso del sistema operativo que ejecuta el lote, procedimiento o función de [!INCLUDE[tsql](../../includes/tsql-md.md)] en la instancia del Motor de base de datos.  
  
3.  Haga clic en **Aceptar** para implementar los cambios o en **Cancelar** para salir sin aplicar los cambios.  
  
## <a name="see-also"></a>Consulte también  
 [Especificar una condición de punto de interrupción](./specify-a-breakpoint-condition.md)   
 [Especificar un número de llamadas](./specify-a-hit-count.md)   
 [Especificar una acción del punto de interrupción](./specify-a-breakpoint-action.md)