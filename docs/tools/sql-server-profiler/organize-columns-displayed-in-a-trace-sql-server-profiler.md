---
title: Organizar las columnas mostradas en un seguimiento
titleSuffix: SQL Server Profiler
description: Facilite el análisis de la salida de seguimiento de SQL Server Profiler mediante la agrupación y agregación de eventos al definir o ver un seguimiento.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 6b923f94-0eb1-467e-82f6-ceed43f77017
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 2727aa7ed5e95881e21eddb73a94ddef97795c08
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353409"
---
# <a name="organize-columns-displayed-in-a-trace-sql-server-profiler"></a>Organizar las columnas mostradas en un seguimiento (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Puede agrupar las columnas de datos de una seguimiento seleccionando **Organizar columnas** en la tabla de seguimiento o el cuadro de diálogo **Propiedades del archivo de seguimiento** , o bien al definir un seguimiento. La agrupación de las columnas de datos permite analizar mejor la salida del seguimiento del [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] . Para obtener más información, vea [Ver y analizar seguimientos con SQL Server Profiler](../../tools/sql-server-profiler/view-and-analyze-traces-with-sql-server-profiler.md).  
  
 **Organizar columnas** permite agrupar los eventos de seguimiento o agruparlos y agregarlos por las columnas de datos que seleccione.  
  
-   Elija varias columnas de datos para agrupar solo eventos de seguimiento. Al elegir varias columnas de datos para su agrupación, en la ventana de seguimiento se muestran los eventos agrupados por los valores de las columnas de datos seleccionadas. En el siguiente ejemplo se muestra el modo en que se verá la cuadrícula de la ventana de seguimiento si elige las columnas de datos **Duration** y **StartTime** para su agrupación. Tenga en cuenta que los valores de la columna **Duration** se muestran en orden ascendente; a continuación, los valores de **StartTime** .  
  
|Duration|StartTime|EventClass|ClientProcessID|  
|--------------|---------------|----------------|---------------------|  
||12/12/2006 3:16:43 PM|SQL:StmtStarting|2124|  
|0|12/12/2006 5:39:23 PM|Audit Login|648|  
|1|12/12/2006 5:24:44 PM|SQL:StmtStarting|2124|  
|25|12/12/2006 5:24:44 PM|SQL:StmtCompleted|648|  
  
-   Elija solo una columna para agrupar y agregar eventos de seguimiento. Al elegir solo una columna de datos para su agrupación, en la ventana del seguimiento se muestran los eventos agrupados por los valores de esa columna de datos y se contraen todos los eventos bajo ella. Aparece un signo más ( **+** ) a la izquierda del evento en la columna de datos elegida para la agrupación, mientras que el número de eventos contraídos bajo ella aparece entre paréntesis a la derecha del evento. En el siguiente ejemplo se muestra el modo en que se verá la cuadrícula de la ventana del seguimiento si elige solo la columna de datos **EventClass** para su agrupación. Tenga en cuenta que todos los eventos se organizan en la columna de datos **EventClass** . Para ver todos los eventos, haga clic en el signo más para expandir y mostrar todas las clases de eventos de ese tipo.  
  
|EventClass|StartTime|Duration|ClientProcessID|  
|----------------|---------------|--------------|---------------------|  
|+ ExistingConnection (6)||||  
|+ SQL:BatchStarting (25)||||  
|+ SQL:StmtCompleted (11)||||  
|+ SQL:SmtStarting (21)||||  
  
### <a name="to-group-data-columns-displayed-in-a-trace"></a>Para agrupar columnas de datos mostradas en un seguimiento  
  
1.  Abra un archivo o una tabla de seguimiento.  
  
2.  En el menú **Archivo** , haga clic en **Propiedades**.  
  
3.  En el cuadro de diálogo **Propiedades del archivo de seguimiento** o **Propiedades de la tabla de seguimiento** , haga clic en la pestaña **Selección de eventos** .  
  
4.  En la pestaña **Selección de eventos** , haga clic en **Organizar columnas**.  
  
5.  En el cuadro de diálogo **Organizar columnas** , seleccione las columnas que desee mostrar en un grupo y haga clic en **Subir** para moverlas a **Grupos**. Cuando haya movido todas las columnas que desee mover a **Grupos**, puede usar los botones **Subir** y **Bajar** para cambiar el orden.  
  
     Mover los nombres de las columnas de datos a la lista **Groups** significa que el seguimiento mostrado se organiza primero por los valores de la columna de datos superior que aparece en la lista **Groups** y, luego, por la segunda columna de datos de la lista **Groups** , etc.  
  
6.  Haga clic en **Aceptar** en el cuadro de diálogo **Organizar columnas** y, a continuación, en **Aceptar** en el cuadro de diálogo **Propiedades de la tabla de seguimiento** o **Propiedades del archivo de seguimiento** .  
  
     Después de hacer clic en **Aceptar** en el cuadro de diálogo **Propiedades de la tabla de seguimiento** o **Propiedades del archivo de seguimiento** , las columnas de datos se reorganizan en el seguimiento mostrado. La columna de datos que movió a la posición superior de la lista **Groups** se sitúa en primer lugar en la pantalla de seguimiento al leer la cuadrícula de izquierda a derecha. Las filas de seguimiento se organizan en orden ascendente por los valores contenidos en las columnas de datos incluidas en la lista **Grupos** . Las columnas elegidas para su agrupación permanecen fijas en la pantalla, pero puede desplazarse a la derecha o la izquierda para ver las demás columnas.  
  
7.  Para desagrupar los datos del seguimiento mostrado, haga clic en **Vista agrupada** en el menú **Ver** para cancelar la selección. Si desea revertir la vista agrupada, vuelva a hacer clic en **Vista agrupada** en el menú **Ver** para volver a seleccionarla.  
  
### <a name="to-group-and-aggregate-data-columns-in-a-trace"></a>Para agrupar y agregar columnas de datos en un seguimiento  
  
1.  Abra un archivo o una tabla de seguimiento.  
  
2.  En el menú **Archivo** , haga clic en **Propiedades**.  
  
3.  En el cuadro de diálogo **Propiedades del archivo de seguimiento** o **Propiedades de la tabla de seguimiento** , haga clic en la pestaña **Selección de eventos** .  
  
4.  En la pestaña **Selección de eventos** , haga clic en **Organizar columnas**.  
  
5.  En el cuadro de diálogo **Organizar columnas** , seleccione una columna por la que desee agrupar y agregar los eventos del seguimiento mostrado. Haga clic en **Subir** para mover el nombre de la columna a **Grupos**. Puede usar los botones **Subir** y **Bajar** para reorganizar las demás columnas en **Columnas** , si es necesario.  
  
6.  Haga clic en **Aceptar** en el cuadro de diálogo **Organizar columnas** y, a continuación, en **Aceptar** en el cuadro de diálogo **Propiedades de la tabla de seguimiento** o **Propiedades del archivo de seguimiento** .  
  
     Después de hacer clic en **Aceptar** en el cuadro de diálogo **Propiedades de la tabla de seguimiento** o **Propiedades del archivo de seguimiento** , las columnas de datos se reorganizan en el seguimiento mostrado. Todos los demás eventos de las columnas de datos se agregan bajo la columna de datos que ha movido a la lista **Grupos** . Haga clic en el signo más ( **+** ) situado a la izquierda del evento en la columna de datos elegida para la agregación para expandirla y ver todos los eventos de ese tipo. La columna elegida para su agregación permanece fija en la pantalla, pero puede desplazarse a la derecha o la izquierda para ver las demás columnas.  
  
7.  Para volver a una vista normal de los datos del seguimiento, haga clic en **Vista agrupada** en el menú **Ver** , lo que cancela la selección. Si desea volver a la vista agregada, haga clic de nuevo en **Vista agregada** en el menú **Ver** para volver a seleccionarla. Recuerde que también puede hacer clic en **Vista agrupada** en el menú **Ver** para mostrar los eventos del seguimiento sin contraerlos.  
  
## <a name="see-also"></a>Consulte también  
 [Crear un seguimiento &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/create-a-trace-sql-server-profiler.md)   
 [Abrir una tabla de seguimiento &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/open-a-trace-table-sql-server-profiler.md)   
 [Abrir un archivo de seguimiento &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/open-a-trace-file-sql-server-profiler.md)  
  
  
