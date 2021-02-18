---
title: Filtrado de los identificadores de proceso de servidor (SPID) en un archivo de seguimiento
titleSuffix: SQL Server Profiler
description: Obtenga información acerca de cómo limitar la salida del seguimiento en SQL Server Profiler aplicando un filtro en el identificador de proceso del servidor (SPID).
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: f5945c39-be6b-4632-91cb-92066c80e188
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: de293e3db8fa0ec05039ecac98534338375fd89f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100335938"
---
# <a name="filter-server-process-ids-spids-in-a-trace-sql-server-profiler"></a>Filtrar los Id. de proceso de servidor (SPID) en un seguimiento (SQL Server Profiler)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo filtrar identificadores de proceso de servidor (SPID) en un seguimiento mediante el [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].  
  
### <a name="to-filter-system-ids-in-a-trace"></a>Para filtrar identificadores de sistema en un seguimiento  
  
1.  En el menú **Archivo** , haga clic en **Nuevo seguimiento** y, a continuación, conéctese a una instancia de SQL Server.  
  
     Aparecerá el cuadro de diálogo **Propiedades de seguimiento** .  
  
    > [!NOTE]  
    >  Si se selecciona **Iniciar el seguimiento inmediatamente tras realizar la conexión**, el cuadro de diálogo **Propiedades de seguimiento** no aparecerá y, en su lugar, se iniciará el seguimiento. Para desactivar esta configuración, en el menú **Herramientas**, haga clic en **Opciones** y desactive la casilla **Iniciar el seguimiento inmediatamente tras realizar la conexión**.  
  
2.  En el cuadro **Nombre de seguimiento** , escriba un nombre para el seguimiento.  
  
3.  En la lista de nombres **Usar la plantilla**, seleccione una plantilla de seguimiento.  
  
4.  Opcionalmente, especifique una tabla o un archivo de destino donde guardar los resultados del seguimiento.  
  
5.  En la pestaña **Selección de eventos**, haga clic en el encabezado de la columna **SPID** para mostrar el cuadro de diálogo **Editar filtro**. También puede hacer clic con el botón derecho en el encabezado de la columna y seleccionar **Editar filtro de columna**. Si no aparece la columna **SPID** , active la casilla **Mostrar todas las columnas** .  
  
6.  En el cuadro de diálogo **Editar filtro** , expanda el operador de comparación adecuado y especifique un SPID como valor para la comparación.  
  
## <a name="see-also"></a>Consulte también  
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)  
  
  
