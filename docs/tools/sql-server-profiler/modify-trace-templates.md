---
title: Modificar plantillas de seguimiento
titleSuffix: SQL Server Profiler
description: Obtenga información sobre cómo modificar una plantilla de seguimiento (SQL Server Profiler). Agregue o quite clases de eventos y columnas de datos, y cambie filtros.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 75b62a54-8d16-4599-bd2d-c42cfcc209f4
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 07/12/2017
ms.openlocfilehash: 0fca2ba1e0f8b1e979ff94c9c3900ec2f73c6292
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351090"
---
# <a name="modify-trace-templates"></a>Modificar plantillas de seguimiento
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Puede modificar las plantillas que están guardadas en un archivo en el equipo local en el que se ejecuta el [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] . También puede modificar plantillas derivadas de esos archivos. Al modificar plantillas existentes, edite las propiedades de la plantilla, como clases de evento y columnas de datos, en el mismo orden en que se definieron originalmente, en la pestaña **Selección de eventos** del cuadro de diálogo **Propiedades de seguimiento** . Puede agregar o quitar clases de eventos y columnas de datos, así como cambiar filtros. Después de modificar la plantilla, se crea una plantilla específica del usuario y la original se deja intacta. Para obtener más información, vea [Guardar seguimientos y plantillas de seguimiento](../../tools/sql-server-profiler/save-traces-and-trace-templates.md).  
  
 Es posible que tenga que derivar una plantilla de un archivo de seguimiento existente si no puede recordar (o no ha guardado) la plantilla original que se utilizó para crear el seguimiento o si desea volver a ejecutar el mismo seguimiento posteriormente. Si trabaja con seguimientos existentes, puede ver las propiedades, pero no modificarlas. Para modificar las propiedades, detenga o pause el seguimiento. Para obtener más información, vea [Derivar una plantilla a partir de un archivo o tabla de seguimiento &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/derive-a-template-from-a-trace-file-or-trace-table-sql-server-profiler.md) y [Derivar una plantilla a partir de un seguimiento en ejecución &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/derive-a-template-from-a-running-trace-sql-server-profiler.md).  
  
## <a name="to-modify-a-trace-template"></a>Para modificar una plantilla de seguimiento  
  
1.  En el menú **Archivo** , seleccione **Plantillas** y haga clic en **Editar plantilla**.  
  
2.  En el cuadro de diálogo **Propiedades de la plantilla de seguimiento** , en la pestaña **General** , modifique el tipo de servidor y el nombre de la plantilla si es necesario o elija una plantilla predeterminada para el tipo de servidor.  
  
3.  En la pestaña **Selección de eventos**, utilice el control de cuadrículas para agregar o quitar eventos y columnas de datos del archivo de seguimiento tal como se indica a continuación.  
  
    -   Para agregar un evento, expanda la categoría de evento correspondiente en la columna **Eventos** y seleccione el nombre del evento.  
  
    -   Cuando se agrega un evento se incluyen de manera predeterminada todas las columnas de datos pertinentes. Para eliminar una columna de datos de un evento del seguimiento, desactive la casilla de la columna de datos del evento.  
  
    -   Para agregar filtros, haga clic en el nombre de la columna de datos y especifique los criterios del filtro en el cuadro de diálogo **Editar filtro** . También puede hacer clic con el botón derecho en el nombre de la columna de datos y hacer clic en **Editar filtro de columna** para abrir el cuadro de diálogo **Editar filtro** . Haga clic en **Aceptar** para agregar el filtro.  
  
4.  Haga clic en **Guardar** o bien en **Guardar como** si desea guardar la plantilla de seguimiento con otro nombre.  
  
## <a name="next-steps"></a>Pasos siguientes  
[Iniciar un seguimiento](../../tools/sql-server-profiler/start-a-trace.md)  
[Crear un seguimiento](../../tools/sql-server-profiler/create-a-trace-sql-server-profiler.md)  
[Modificar un seguimiento existente (con Transact-SQL)](../../relational-databases/sql-trace/modify-an-existing-trace-transact-sql.md)  
[Especificar eventos y columnas de datos para un seguimiento mediante SQL Server Profiler](../../tools/sql-server-profiler/specify-events-and-data-columns-for-a-trace-file-sql-server-profiler.md)  
[sp-trace-setevent-transact-sql](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)  
  
