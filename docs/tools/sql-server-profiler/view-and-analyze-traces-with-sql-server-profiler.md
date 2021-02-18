---
title: Visualización y análisis de los seguimientos
titleSuffix: SQL Server Profiler
description: Obtenga información sobre cómo usar SQL Server Profiler para ver datos de seguimiento, buscar eventos específicos, mostrar nombres de objetos y solucionar problemas.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 17e821ca-a12e-4192-acc1-96765d9ae266
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: b95380a4a99e525cc97588b2051d1812c6e2d34a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338638"
---
# <a name="view-and-analyze-traces-with-sql-server-profiler"></a>Ver y analizar seguimientos con SQL Server Profiler

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Utilice [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] para ver los datos de eventos capturados en un seguimiento. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] muestra datos basados en propiedades de seguimiento definidas. Una manera de analizar datos del [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consiste en copiarlos a otro programa, como [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o el Asistente para la optimización del [!INCLUDE[ssDE](../../includes/ssde-md.md)] . [!INCLUDE[ssDE](../../includes/ssde-md.md)] El Asistente para la optimización puede usar un archivo de seguimiento que contenga lotes SQL y eventos RPC (llamada a procedimiento remoto) si el seguimiento incluye la columna de datos **Text** . Para asegurarse de que se capturan las columnas y los eventos correctos para su utilización con el Asistente para la optimización del [!INCLUDE[ssDE](../../includes/ssde-md.md)] , utilice la plantilla predefinida Tuning que se proporciona con el [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].  
  
 Cuando abra un seguimiento mediante el [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], no es necesario que el archivo de seguimiento tenga la extensión de archivo .trc si se creó mediante el [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] o los procedimientos almacenados del sistema de Seguimiento de SQL.  
  
> [!NOTE]  
>  [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] también puede leer archivos .log de Seguimiento de SQL y archivos de scripts SQL genéricos. Si abre un archivo .log de Seguimiento de SQL que no tiene la extensión .log, por ejemplo, trace.txt, deberá especificar **SQLTrace_Log** como formato del archivo.  
  
 Puede configurar el formato de visualización de la fecha y hora del [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] como ayuda en el análisis de seguimientos.  
  
## <a name="troubleshooting-data"></a>Solución de problemas de los datos  
 El [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]permite solucionar problemas de datos mediante la agrupación de seguimientos o archivos de seguimiento según las columnas de datos **Duration**, **CPU**, **Reads** o **Writes** . Algunos ejemplos son consultas que no se ejecutan satisfactoriamente o que tienen una cantidad excepcionalmente alta de lecturas lógicas.  
  
 Puede buscar información adicional si guarda los seguimientos en tablas y utiliza [!INCLUDE[tsql](../../includes/tsql-md.md)] para realizar consultas en los datos de eventos. Por ejemplo, para determinar los eventos **SQL:BatchCompleted** con un tiempo de espera excesivo, ejecute lo siguiente:  
  
```  
SELECT  TextData, Duration, CPU  
FROM    trace_table_name  
WHERE   EventClass = 12 -- SQL:BatchCompleted events  
AND     CPU < (Duration * 1000)  
```  
  
> [!NOTE]  
>  El servidor informa de la duración de un evento en microsegundos (10 ^-6 segundos) y de la cantidad de tiempo de CPU utilizado por el evento en milisegundos (10 ^-3 segundos). La interfaz gráfica de usuario de [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] muestra la columna **Duración** en milisegundos de manera predeterminada, pero cuando se guarda un seguimiento en un archivo o en una tabla de base de datos, el valor de la columna **Duración** se escribe en microsegundos.  
  
## <a name="displaying-object-names-when-viewing-traces"></a>Mostrar nombres de objetos al ver seguimientos  
 Si quiere visualizar el nombre de un objeto en lugar de su identificador (**Object ID**), debe capturar las columnas de datos **Server Name** y **Database ID** junto con **Object Name** .  
  
 Si decide realizar una agrupación mediante la columna de datos **Object ID** , realícela primero mediante las columna de datos **Server Name** y **Database ID** , y luego mediante **Object ID** . Del mismo modo, si decide realizar una agrupación mediante la columna de datos **Index ID** , realícela primero mediante las columna de datos **Server Name**, **Database ID** y **Object ID** , y luego mediante **Index ID** . Es necesario que siga este orden ya que los Id. de objeto y de índice no son exclusivos entre servidores y bases de datos (ni entre objetos, en el caso de los Id. de índice).  
  
## <a name="finding-specific-events-within-a-trace"></a>Buscar eventos específicos en un seguimiento  
 Para encontrar y agrupar eventos en un seguimiento, siga estos pasos:  
  
1.  Cree el seguimiento.  
  
    -   Una vez definida, capture las columnas de datos **Event Class**, **ClientProcessID** y **Start Time** además de cualquier otra columna de datos que desee capturar. Para obtener más información, vea [Crear un seguimiento &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/create-a-trace-sql-server-profiler.md).  
  
    -   Agrupe los datos capturados por la columna de datos **Event Class** y capture el seguimiento en un archivo o una tabla. Para agrupar los datos capturados, haga clic en la opción **Organizar columnas** de la pestaña **Selección de eventos** del cuadro de diálogo Propiedades de seguimiento. Para obtener más información, vea [Organizar las columnas mostradas en un seguimiento &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/organize-columns-displayed-in-a-trace-sql-server-profiler.md).  
  
    -   Inicie el seguimiento y deténgalo una vez pasado el tiempo apropiado o cuando se haya capturado el número de eventos.  
  
2.  Busque los eventos de destino.  
  
    -   Abra el archivo o la tabla de seguimiento y expanda el nodo de la clase de evento que desee, por ejemplo, **Deadlock Chain**. Para obtener más información, vea [Abrir un archivo de seguimiento &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/open-a-trace-file-sql-server-profiler.md) o el Asistente para la optimización del [Abrir una tabla de seguimiento &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/open-a-trace-table-sql-server-profiler.md).  
  
    -   Busque en los datos del seguimiento hasta que encuentre los eventos que busca. Para facilitar la búsqueda, puede utilizar el comando **Buscar** del menú **Edición** del [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]. Anote los valores de las columnas de datos **ClientProcessID** y **Start Time** correspondientes a los eventos de los que se realiza un seguimiento.  
  
3.  Muestre los eventos en el contexto.  
  
    -   Muestre las propiedades del seguimiento y agrupe por la columna de datos **ClientProcessID** en lugar de **Event Class** .  
  
    -   Expanda los nodos de cada Id. de proceso de cliente que desee ver. Realice una búsqueda manual en el seguimiento o utilice **Buscar** hasta que encuentre los valores de **Start Time** anotados anteriormente de los eventos de destino. Los eventos se muestran en orden cronológico con los demás eventos que pertenecen a cada identificador de proceso de cliente seleccionado. Por ejemplo, los eventos **Deadlock** y **Deadlock Chain**, capturados dentro del seguimiento, aparecen inmediatamente después de los eventos **SQL:BatchStarting** events within the expyed client process ID.  
  
 La misma técnica se puede emplear para buscar cualquier evento agrupado. Una vez que haya encontrado los eventos que busca, agrúpelos por **ClientProcessID**, **ApplicationName** u otra clase de eventos para ver la actividad relacionada en orden cronológico.  
  
## <a name="see-also"></a>Consulte también  
 [Ver un seguimiento guardado &#40;Transact-SQL&#41;](../../relational-databases/sql-trace/view-a-saved-trace-transact-sql.md)   
 [sys.fn_trace_getinfo &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-trace-getinfo-transact-sql.md)   
 [Ver información de un filtro &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/view-filter-information-sql-server-profiler.md)   
 [Ver información de un filtro &#40;Transact-SQL&#41;](../../relational-databases/sql-trace/view-filter-information-transact-sql.md)   
 [Abrir un archivo de seguimiento &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/open-a-trace-file-sql-server-profiler.md)   
 [Abrir una tabla de seguimiento &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/open-a-trace-table-sql-server-profiler.md)  
  
  
