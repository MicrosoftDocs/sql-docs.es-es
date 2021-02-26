---
title: Filtrar eventos en función de la hora de finalización del evento
titleSuffix: SQL Server Profiler
description: Filtre los eventos por hora de finalización durante un seguimiento. Aprenda a configurar un filtro en la hora de finalización del evento en SQL Server Profiler.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 74628f9e-2d39-496a-a443-0a3887db223d
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 4306910b10fe1ac014226b9013af41b235b5e628
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345828"
---
# <a name="filter-events-based-on-the-event-end-time-sql-server-profiler"></a>Filtrar eventos basándose en la hora de finalización del evento (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe el modo de filtrar eventos de seguimiento basándose en la hora de finalización del evento mediante el [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].  
  
### <a name="to-filter-events-based-on-the-event-end-time"></a>Para filtrar eventos basándose en la hora de finalización del evento  
  
1.  En el menú **Archivo** , haga clic en **Nuevo seguimiento** y conéctese a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
     Aparecerá el cuadro de diálogo **Propiedades de seguimiento**.  
  
    > [!NOTE]  
    >  Si se selecciona **Iniciar el seguimiento inmediatamente tras realizar la conexión**, el cuadro de diálogo **Propiedades de seguimiento** no aparecerá y, en su lugar, se iniciará el seguimiento. Para desactivar esta configuración, en el menú **Herramientas**, haga clic en **Opciones** y desactive la casilla **Iniciar el seguimiento inmediatamente tras realizar la conexión** .  
  
2.  En el cuadro de diálogo **Propiedades de seguimiento** , asegúrese de que la pestaña **General** está seleccionada y especifique un nombre en el cuadro de texto **Nombre de seguimiento** .  
  
3.  En la lista **Usar la plantilla**, seleccione una plantilla de seguimiento.  
  
4.  Opcionalmente, especifique una tabla o un archivo de destino donde guardar los resultados del seguimiento.  
  
5.  En el menú **Selección de eventos**, haga clic en la columna de datos **Hora de finalización** para iniciar el cuadro de diálogo **Editar filtro** . También puede hacer clic con el botón derecho en el encabezado de la columna y seleccionar **Editar filtro de columna**.  
  
6.  Expanda **Mayor que** o **Menor que**, y especifique un valor **datetime** en el campo que aparece debajo del operador de comparación.  
  
## <a name="see-also"></a>Consulte también  
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)   
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)  
  
  
