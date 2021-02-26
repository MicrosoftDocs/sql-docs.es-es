---
title: Plantillas
titleSuffix: SQl Server Profiler
description: Obtenga información sobre las plantillas predefinidas que proporciona SQL Server Profiler y cómo usarlas. Vea cómo crear plantillas definidas por el usuario y cambiar la plantilla predeterminada.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: b674e491-dc58-47a1-acdd-7028e9a201fc
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: ef486049c3a50d2e0b9ba216adcfbcc3881b31b1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340549"
---
# <a name="sql-server-profiler-templates"></a>Plantillas de SQL Server Profiler

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Puede utilizar el [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] para crear plantillas que definan las clases de eventos y columnas de datos que desea incluir en los seguimientos. Después de definir y guardar la plantilla, ejecute un seguimiento que registre los datos de cada clase de evento que ha seleccionado. Una sola plantilla puede utilizarse en varios seguimientos puesto que la plantilla no se ejecuta como tal.  

[!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] incluye plantillas de seguimiento predefinidas para que pueda configurar fácilmente las clases de evento que seguramente necesitará para seguimientos concretos. La plantilla Standard, por ejemplo, le ayuda a crear un seguimiento genérico para registrar inicios y cierres de sesión, lotes finalizados e información de conexión. Esta plantilla permite ejecutar seguimientos sin modificarlos o como punto de inicio para plantillas adicionales con configuraciones de evento distintas.  
  
> [!NOTE]  
>  Además de los seguimientos de las plantillas predefinidas, el [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] también permite crearlas a partir de una plantilla en blanco que no contenga ninguna clase de evento de manera predeterminada. Puede resultar útil utilizar la plantilla de seguimiento en blanco cuando un seguimiento planeado no se parece a la configuración de ninguna de las plantillas predefinidas.  
  
 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] permite realizar un seguimiento de diversos tipos de servidor. Por ejemplo, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  Sin embargo, las clases de eventos que pueden incluirse no son las mismas para cada tipo de servidor. Por lo tanto, el [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] mantiene plantillas distintas para los diferentes tipos de servidor y pone a disposición del usuario la plantilla específica correspondiente al tipo de servidor seleccionado.  
  
## <a name="predefined-templates"></a>Plantillas predefinidas  
 Además de la plantilla Standard (predeterminada), el [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] incluye varias plantillas predefinidas para supervisar determinados tipos de evento. En la siguiente tabla figura una lista de las plantillas predefinidas, su finalidad y las clases de eventos sobre las que capturan información.  
  
|Nombre de la plantilla|Finalidad de la plantilla|Clases de eventos|  
|-------------------|----------------------|-------------------|  
|SP_Counts|Captura el comportamiento de la ejecución de procedimientos almacenados a lo largo del tiempo.|**SP:Starting**|  
|Estándar|Punto de inicio genérico para crear un seguimiento. Captura todos los procedimientos almacenados y lotes de [!INCLUDE[tsql](../../includes/tsql-md.md)] que se ejecutan. Utilice esta plantilla para supervisar la actividad general del servidor de base de datos.|**Audit Login**<br /><br /> **Audit Logout**<br /><br /> **ExistingConnection**<br /><br /> **RPC:Completed**<br /><br /> **SQL:BatchCompleted**<br /><br /> **SQL:BatchStarting**|  
|TSQL|Captura todas las instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] que los clientes envían a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y el momento en que se han emitido. Utilice esta plantilla para depurar las aplicaciones cliente.|**Audit Login**<br /><br /> **Audit Logout**<br /><br /> **ExistingConnection**<br /><br /> **RPC:Starting**<br /><br /> **SQL:BatchStarting**|  
|TSQL_Duration|Captura todas las instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] que los clientes envían a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , el tiempo de ejecución (en milisegundos), y las agrupa por duración. Utilice esta plantilla para identificar consultas de ejecución lenta.|**RPC:Completed**<br /><br /> **SQL:BatchCompleted**|  
|TSQL_Grouped|Captura todas las instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] enviadas a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y el momento en que se han emitido. Agrupa la información por el usuario o cliente que ha enviado la instrucción. Utilice esta plantilla para investigar consultas de un cliente o usuario determinado.|**Audit Login**<br /><br /> **Audit Logout**<br /><br /> **ExistingConnection**<br /><br /> **RPC:Starting**<br /><br /> **SQL:BatchStarting**|  
|TSQL_Locks|Captura todas las instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] que los clientes envían a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , junto con los eventos de bloqueo excepcionales. Úselo para solucionar problemas de eventos de interbloqueos, de tiempo de espera de bloqueo y de extensión de bloqueo.|**Blocked Process Report**<br /><br /> **SP:StmtCompleted**<br /><br /> **SP:StmtStarting**<br /><br /> **SQL:StmtCompleted**<br /><br /> **SQL:StmtStarting**<br /><br /> **Deadlock Graph**<br /><br /> **Lock:Cancel**<br /><br /> **Lock:Deadlock**<br /><br /> **Lock:Deadlock Chain**<br /><br /> **Lock:Escalation**<br /><br /> **Lock:Timeout (timeout>0)**|  
|TSQL_Replay|Captura información detallada acerca de las instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] necesaria para cuando se reproduzca el seguimiento. Utilice esta plantilla para ejecutar optimizaciones iterativas tales como pruebas comparativas.|**CursorClose**<br /><br /> **CursorExecute**<br /><br /> **CursorOpen**<br /><br /> **CursorPrepare**<br /><br /> **CursorUnprepare**<br /><br /> **Audit Login**<br /><br /> **Audit Logout**<br /><br /> **Existing Connection**<br /><br /> **RPC Output Parameter**<br /><br /> **RPC:Completed**<br /><br /> **RPC:Starting**<br /><br /> **Exec Prepared SQL**<br /><br /> **Prepare SQL**<br /><br /> **SQL:BatchCompleted**<br /><br /> **SQL:BatchStarting**|  
|TSQL_SPs|Captura información detallada acerca de todos los procedimientos almacenados en ejecución. Utilice esta plantilla para analizar los pasos de componente de los procedimientos almacenados. Agregue el evento **SP:Recompile** si sospecha que se están volviendo a compilar los procedimientos.|**Audit Login**<br /><br /> **Audit Logout**<br /><br /> **ExistingConnection**<br /><br /> **RPC:Starting**<br /><br /> **SP:Completed**<br /><br /> **SP:Starting**<br /><br /> **SP:StmtStarting**<br /><br /> **SQL:BatchStarting**|  
|Ajuste|Captura información acerca de los procedimientos almacenados y la ejecución de lotes de [!INCLUDE[tsql](../../includes/tsql-md.md)] . Utilice esta plantilla para crear un archivo de salida del seguimiento que el Asistente para la optimización de [!INCLUDE[ssDE](../../includes/ssde-md.md)] pueda utilizar como carga de trabajo para optimizar las bases de datos.|**RPC:Completed**<br /><br /> **SP:StmtCompleted**<br /><br /> **SQL:BatchCompleted**|  
  
 Para obtener información acerca de las clases de eventos, vea [SQL Server Event Class Reference](../../relational-databases/event-classes/sql-server-event-class-reference.md).  
  
## <a name="default-template"></a>Plantilla predeterminada  
 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] designa de forma automática la plantilla **Standard** como plantilla predeterminada para aplicar a cualquier seguimiento nuevo. No obstante, puede cambiar la plantilla predeterminada por cualquier otra predefinida o definida por el usuario. Para cambiar la plantilla predeterminada, active la casilla **Usar como plantilla predeterminada para tipo de servidor seleccionado** cuando cree o edite una plantilla desde la pestaña **General** del cuadro de diálogo **Propiedades de la plantilla de seguimiento** .  
  
 Para obtener acceso al cuadro de diálogo **Propiedades de la plantilla de seguimiento**, en el menú **Archivo** de [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], elija **Plantillas** y haga clic en **Nueva plantilla** o **Editar plantilla**.  
  
> [!NOTE]  
>  La plantilla predeterminada es específica para un tipo de servidor concreto. Si la cambia para un tipo de servidor, seguirá siendo la misma para el resto de tipos de servidor. Para obtener más información sobre cómo configurar una plantilla predeterminada para un servidor específico, vea [Configurar los valores predeterminados de definición de seguimiento &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/set-trace-definition-defaults-sql-server-profiler.md).  
  
## <a name="see-also"></a>Consulte también  
 [Crear una plantilla de seguimiento &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/create-a-trace-template-sql-server-profiler.md)   
 [Modificar una plantilla de seguimiento &#40;SQL Server Profiler&#41;](./modify-trace-templates.md)   
 [Exportar una plantilla de seguimiento &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/export-a-trace-template-sql-server-profiler.md)   
 [Importar una plantilla de seguimiento &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/import-a-trace-template-sql-server-profiler.md)  
  
