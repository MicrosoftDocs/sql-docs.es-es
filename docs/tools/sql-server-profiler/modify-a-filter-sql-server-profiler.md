---
title: Modificar un filtro
titleSuffix: SQL Server Profiler
description: Obtenga información sobre cómo modificar un filtro de seguimiento en SQL Server Profiler para poder recopilar la información que necesita. Obtenga información sobre cómo los filtros de seguimiento afectan al rendimiento de la base de datos.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 8b317813-4918-4485-b930-77b1951aa00c
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: dc2171ea2aaf418eae0fa277a8f4144dc4e7ca9d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345038"
---
# <a name="modify-a-filter-sql-server-profiler"></a>Modificar un filtro (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Se agregan filtros a las plantillas de seguimiento, que contienen las definiciones de los seguimiento, para limitar el número de eventos que recopila un seguimiento. Esta limitación puede reducir las consecuencias sobre el rendimiento de las trazas. Si establece filtros para una plantilla de seguimiento y averigua que el seguimiento no recopila el tipo de información que necesita, puede modificar el filtro.  
  
### <a name="to-modify-a-filter"></a>Para modificar un filtro  
  
1.  En el [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], abra la plantilla del filtro de seguimiento que desea modificar. En el menú **Archivo** , haga clic en **Plantillas** y, a continuación, elija **Editar plantilla**.  
  
2.  En la pestaña **General** del cuadro de diálogo **Propiedades de la plantilla de seguimiento** , seleccione una plantilla en la lista **Seleccionar nombre de plantilla** .  
  
3.  Haga clic en la pestaña **Selección de eventos** .  
  
     La pestaña **Selección de eventos** contiene un control de cuadrícula. El control de cuadrícula es una tabla que contiene todas las clases de eventos en las que se puede realizar un seguimiento. La tabla contiene una fila para cada clase de evento. Las clases de eventos pueden diferir ligeramente dependiendo del tipo y la versión del servidor al que esté conectado. Las clases de eventos se identifican en la columna **Events** de la cuadrícula y se agrupan por categorías de eventos. En las demás columnas se enumeran las columnas de datos que pueden devolverse para cada clase de evento.  
  
4.  Haga clic en **Filtros de columna**.  
  
5.  En el cuadro de diálogo **Editar filtro** , haga clic en el valor junto al operador de comparación que desea modificar y escriba el valor nuevo o elimine un valor. También puede agregar otros filtros.  
  
6.  Haga clic en **Aceptar** y guarde la plantilla.  
  
## <a name="see-also"></a>Consulte también  
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)  
  
  
