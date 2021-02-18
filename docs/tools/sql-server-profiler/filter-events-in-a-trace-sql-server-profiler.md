---
title: Filtrar los eventos de un seguimiento
titleSuffix: SQL Server Profiler
description: Aprenda a establecer un filtro para limitar los eventos que SQL Server Profiler captura durante un seguimiento. Obtenga información sobre los formatos necesarios para determinados filtros.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 0fd63573-3b35-4f67-9e1e-ed9aabee11a8
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: c80703ac5c23ba40007528c6285eb74e15e80264
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345065"
---
# <a name="filter-events-in-a-trace-sql-server-profiler"></a>Filtrar eventos en un seguimiento (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Los filtros limitan los eventos que se recopilan en el seguimiento. Si no se establece un filtro, se devolverán todos los eventos de las clases de eventos seleccionadas en el resultado del seguimiento. No es obligatorio establecer un filtro para un seguimiento. Sin embargo, un filtro minimiza la sobrecarga que comporta una traza.  
  
 Para agregar filtros a las definiciones de seguimiento, utilice la pestaña **Selección de eventos** del cuadro de diálogo **Propiedades de seguimiento** o **Propiedades de la plantilla de seguimiento** .  
  
### <a name="to-filter-events-in-a-trace"></a>Para filtrar los eventos de un seguimiento  
  
1.  En el cuadro de diálogo **Propiedades de seguimiento** o **Propiedades de la plantilla de seguimiento** , haga clic en la pestaña **Selección de eventos** .  
  
     La pestaña **Selección de eventos** contiene un control de cuadrícula. El control de cuadrícula es una tabla que contiene todas las clases de eventos en las que se puede realizar un seguimiento. La tabla contiene una fila para cada clase de evento. Las clases de eventos pueden diferir ligeramente dependiendo del tipo y la versión del servidor al que esté conectado. Las clases de eventos se identifican en la columna **Eventos** de la cuadrícula y se agrupan por categoría de eventos. En las demás columnas se enumeran las columnas de datos que pueden devolverse para cada clase de evento.  
  
2.  Haga clic en **Filtros de columna.**  
  
     Aparecerá el cuadro de diálogo **Editar filtro**. El cuadro de diálogo **Editar filtro** incluye una lista de operadores de comparación que se pueden utilizar para filtrar los eventos en un seguimiento.  
  
3.  Para aplicar un filtro, haga clic en el operador de comparación y escriba un valor para utilizarlo con el filtro.  
  
4.  Haga clic en **OK**.  
  
 **Consideraciones:**  
  
-   Si establece condiciones de filtro en las columnas de datos **StartTime** y **EndTime** de la pestaña Selección de eventos, asegúrese de lo siguiente:  
  
    -   La fecha especificada tiene el formato `YYYY/MM/DD HH:mm:sec`.  
  
         O  
  
    -   Se ha seleccionado la opción **Usar la configuración regional para mostrar valores de fecha y hora** del cuadro de diálogo **Opciones generales** . Para ver el cuadro de diálogo **Opciones generales**, en el menú **Herramientas** de [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], haga clic en **Opción**.  
  
         Y  
  
    -   La fecha especificada se encuentra entre el 1 de enero de 1753 y el 31 de diciembre de 9999.  
  
-   Si se realiza un seguimiento de los eventos con la utilidad **osql** o **sqlcmd** , agregue siempre **%** a los filtros de la columna de datos **TextData** .  
  
## <a name="see-also"></a>Consulte también  
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)  
  
  
