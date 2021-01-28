---
title: Supervisar el uso de la memoria
description: 'Supervise una instancia de SQL Server para confirmar que el uso de memoria se encuentra dentro de los rangos normales. Uso de memoria: Bytes y memoria disponibles: Contadores de páginas por segundo.'
ms.custom: ''
ms.date: 01/20/2021
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- tuning databases [SQL Server], memory
- monitoring server performance [SQL Server], memory usage
- isolating memory [SQL Server]
- paging rate [SQL Server]
- memory [SQL Server], monitoring usage
- monitoring [SQL Server], memory usage
- low-memory conditions
- database monitoring [SQL Server], memory usage
- available memory [SQL Server]
- page faults [SQL Server]
- monitoring performance [SQL Server], memory usage
- server performance [SQL Server], memory
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 8954573b0c32eef45ec655b27d26e03e72be96ed
ms.sourcegitcommit: 713e5a709e45711e18dae1e5ffc190c7918d52e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2021
ms.locfileid: "98688775"
---
# <a name="monitor-memory-usage"></a>Supervisión del uso de la memoria
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Supervise una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] periódicamente para confirmar que la utilización de la memoria se encuentra dentro de los intervalos normales. 

## <a name="configuring-ssnoversion-max-memory"></a>Configuración de la memoria máxima de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

De forma predeterminada, con el tiempo una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puede consumir la mayor parte de la memoria del sistema operativo Windows disponible en el servidor. Una vez que se ha adquirido la memoria, no se liberará a menos que se detecte presión de memoria. Esto es así por diseño y no indica una fuga de memoria en el proceso de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Use [la opción **memoria de servidor máxima**](../../database-engine/configure-windows/server-memory-server-configuration-options.md) para limitar la cantidad de memoria que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puede adquirir para la mayoría de sus usos. Para obtener más información, vea la [Guía de arquitectura de administración de memoria](../../relational-databases/memory-management-architecture-guide.md#changes-to-memory-management-starting-with-).

En SQL Server para Linux, [establezca el límite de memoria](../../linux/sql-server-linux-performance-best-practices.md#advanced-configuration) con la herramienta mssql-conf y el [valor memory.memorylimitmb](../../linux/sql-server-linux-configure-mssql-conf.md#memorylimit).  

## <a name="monitor-operating-system-memory"></a>Supervisión de la memoria del sistema operativo   
 Para supervisar las condiciones de memoria insuficiente, use los contadores de servidor de Windows siguientes. Muchos contadores de memoria del sistema operativo se pueden consultar a través de las vistas de administración dinámica [sys.dm_os_process_memory](../system-dynamic-management-views/sys-dm-os-process-memory-transact-sql.md) y [sys.dm_os_sys_memory](../system-dynamic-management-views/sys-dm-os-sys-memory-transact-sql.md).

-   **Memoria: Bytes disponibles**  
Este contador indica cuántos bytes de memoria están disponibles actualmente para usarse en los procesos. Los valores bajos del contador **Bytes disponibles** pueden indicar una escasez general de memoria del sistema operativo. Este valor se puede consultar mediante T-SQL con [sys.dm_os_sys_memory](../system-dynamic-management-views/sys-dm-os-sys-memory-transact-sql.md).available_physical_memory_kb.

-   **Memoria: Páginas/s**  
Este contador indica el número de páginas que se han recuperado del disco debido a errores de página no recuperables, o bien que se han escrito en disco para liberar espacio en el espacio de trabajo debido a errores de página. Un valor alto en el contador **Páginas/s** puede indicar una paginación excesiva.  

-   **Memoria: Errores de página/s** Este contador indica la tasa de errores de página para todos los procesos, incluidos los del sistema. Una tasa baja pero distinta de cero de paginación en disco (y, por tanto, de errores de página) es normal, incluso si el equipo tiene mucha memoria disponible. El Administrador de memoria virtual (VMM) de Microsoft Windows sustrae páginas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y otros procesos a medida que recorta los tamaños del espacio de trabajo para estos procesos, lo que suele provocar errores de página.  

-   **Proceso: Errores de página/s** Este contador indica la tasa de errores de página para un proceso de usuario concreto. Supervise **Proceso: Errores de página/s** para determinar si la actividad de disco está causada por la paginación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para determinar si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] u otro proceso son la causa de una paginación excesiva, supervise el contador **Proceso: Errores de página/s** para la instancia del proceso de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Para obtener más información sobre cómo solucionar la paginación excesiva, vea la documentación del sistema operativo.  
  
## <a name="isolating-memory-used-by-ssnoversion"></a>Aislamiento de la memoria que usa [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 

 Para supervisar el uso de memoria de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], utilice los siguientes [contadores de objeto de SQL Server](use-sql-server-objects.md). Muchos contadores de objeto de SQL Server se pueden consultar a través de las vistas de administración dinámica [sys.dm_os_performance_counters](../system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md) o [sys.dm_os_process_memory](../system-dynamic-management-views/sys-dm-os-process-memory-transact-sql.md).

 De forma predeterminada, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cambia dinámicamente sus requisitos de memoria, según los recursos del sistema disponibles. Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] necesita más memoria, consulta el sistema operativo para determinar si hay memoria física disponible y la utiliza. Si el sistema operativo tiene poca memoria libre, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] le devolverá memoria hasta que se solucione esa condición, o bien hasta que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] alcance el límite de **memoria de servidor mínima**. Pero se puede invalidar el uso dinámico de la memoria mediante las opciones de configuración del servidor **memoria de servidor mínima** y **memoria de servidor máxima**. Para obtener más información, vea el documento sobre las [opciones de memoria del servidor](../../database-engine/configure-windows/server-memory-server-configuration-options.md).  
  
 Para supervisar la cantidad de memoria que utiliza [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , examine los siguientes contadores de rendimiento:  
  
-   **SQL Server: Administrador de memoria: memoria total del servidor (KB)**  
Este contador indica la cantidad de memoria del sistema operativo que el administrador de memoria de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ha confirmado actualmente a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Se espera que este número crezca en función de las necesidades de la actividad real y lo haga después del inicio de SQL Server. Consulte este contador mediante la vista de administración dinámica [sys.dm_os_sys_info](../system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md) y observe la columna **committed_kb**.

-   **SQL Server: Administrador de memoria: Memoria del servidor de destino (KB)**  
Este contador indica una cantidad de memoria ideal que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] podría consumir, en función de la carga de trabajo reciente. Compárelo con **Memoria total del servidor** después de un período de funcionamiento normal para determinar si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tiene una cantidad de memoria deseada asignada. Después del funcionamiento normal, los valores **Memoria total del servidor** y **Memoria del servidor de destino** deben ser similares. Si el valor de **Memoria total del servidor** es significativamente menor que el de **Memoria del servidor de destino**, es posible que la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] experimente presión de memoria. Durante un período posterior al inicio de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], se espera que el valor de **Memoria total del servidor** sea inferior al de **Memoria del servidor de destino** mientras aumenta **Memoria total del servidor**. Consulte este contador mediante la vista de administración dinámica [sys.dm_os_sys_info](../system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md) y observe la columna **committed_target_kb**. Para obtener más información y los procedimientos recomendados para configurar la memoria, vea [Opciones de configuración de memoria del servidor](../../database-engine/configure-windows/server-memory-server-configuration-options.md#manually).

-   **Proceso: espacio de trabajo**  
Este contador indica la cantidad de memoria física que un proceso usa actualmente, según el sistema operativo. Observe la instancia de sqlservr.exe de este contador. Consulte este contador mediante la vista de administración dinámica [sys.dm_os_process_memory](../system-dynamic-management-views/sys-dm-os-process-memory-transact-sql.md) y observe la columna **physical_memory_in_use_kb**.

-   **Proceso: Bytes privados**  
Este contador indica la cantidad de memoria que un proceso ha solicitado al sistema operativo para uso propio. Observe la instancia de sqlservr.exe de este contador. Como este contador incluye todas las asignaciones de memoria solicitadas por sqlservr.exe, incluidas las que no están limitadas por [la opción **memoria de servidor máxima**](../../database-engine/configure-windows/server-memory-server-configuration-options.md), puede notificar valores mayores que los de [la opción **memoria de servidor máxima**](../../database-engine/configure-windows/server-memory-server-configuration-options.md).

-   **SQL Server: Administrador de búfer: páginas de base de datos**  
Este contador indica el número de páginas en el grupo de búferes con contenido de la base de datos. No incluye otra memoria de grupo que no sea de búfer en el proceso de SQL Server. Consulte este contador con la vista de administración dinámica [sys.dm_os_performance_counters](../system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md).

-   **SQL Server: Administrador de búfer: frecuencia de aciertos de caché del búfer**  
Este contador es específico de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Lo recomendables es una proporción de 90 o superior. Un valor mayor de 90 indica que se ha atendido más del 90% de todas las solicitudes de datos desde la caché de datos en memoria sin tener que leer del disco. Para obtener más información sobre Buffer Manager de SQL Server, vea [Buffer Manager (objeto de SQL Server)](sql-server-buffer-manager-object.md). Consulte este contador con la vista de administración dinámica [sys.dm_os_performance_counters](../system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md).  
 
-   **SQL Server: Administrador de búfer: Duración prevista de la página**  
Este contador mide la cantidad de tiempo en segundos que la página más antigua permanece en el grupo de búferes. En los sistemas que usan una arquitectura NUMA, es el promedio de todos los nodos de NUMA. Se recomienda un valor creciente más alto. Un descenso repentino indica una renovación considerable de los datos dentro y fuera del grupo de búferes, lo que indica que la carga de trabajo no se ha beneficiado por completo de los datos que ya están en la memoria. Cada nodo de NUMA tiene su propio nodo del grupo de búferes. En servidores con más de un nodo de NUMA, vea la duración prevista de la página de cada nodo del grupo de búferes mediante **SQL Server: Nodo del búfer: Duración prevista de la página**. Consulte este contador con la vista de administración dinámica [sys.dm_os_performance_counters](../system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md).

  
## <a name="examples"></a>Ejemplos 

### <a name="determining-current-memory-allocation"></a>Determinación de la asignación de memoria actual  
 Las consultas siguientes devuelven información sobre la memoria asignada actualmente.  
  
```  
SELECT
(total_physical_memory_kb/1024) AS Total_OS_Memory_MB,
(available_physical_memory_kb/1024)  AS Available_OS_Memory_MB
FROM sys.dm_os_sys_memory;

SELECT  
(physical_memory_in_use_kb/1024) AS Memory_used_by_Sqlserver_MB,  
(locked_page_allocations_kb/1024) AS Locked_pages_used_by_Sqlserver_MB,  
(total_virtual_address_space_kb/1024) AS Total_VAS_in_MB,
process_physical_memory_low,  
process_virtual_memory_low  
FROM sys.dm_os_process_memory;  
```  

### <a name="determining-current-sql-server-memory-utilization"></a>Determinación del uso de memoria actual de SQL Server   
 La consulta siguiente devuelve información sobre el uso de memoria actual de SQL Server.  
```  
SELECT
sqlserver_start_time,
(committed_kb/1024) AS Total_Server_Memory_MB,
(committed_target_kb/1024)  AS Target_Server_Memory_MB
FROM sys.dm_os_sys_info;
```   

### <a name="determining-page-life-expectancy"></a>Determinación de la duración prevista de la página
 En la consulta siguiente se usa **sys.dm_os_performance_counters** para observar el valor **Duración prevista de la página** actual de la instancia de SQL Server en el nivel general del administrador de búfer y en cada nivel de nodo de NUMA.
```
SELECT
CASE instance_name WHEN '' THEN 'Overall' ELSE instance_name END AS NUMA_Node, cntr_value AS PLE_s
FROM sys.dm_os_performance_counters    
WHERE counter_name = 'Page life expectancy';
```

## <a name="see-also"></a>Consulte también
- [sys.dm_os_sys_memory (Transact-SQL)](../system-dynamic-management-views/sys-dm-os-sys-memory-transact-sql.md)
- [sys.dm_os_process_memory (Transact-SQL)](../system-dynamic-management-views/sys-dm-os-process-memory-transact-sql.md)
- [sys.dm_os_sys_info (Transact-SQL)](../system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md)
- [sys.dm_os_performance_counters (Transact-SQL)](../system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md)
- [Memory Manager (objeto de SQL Server)](sql-server-memory-manager-object.md)
- [Buffer Manager (objeto de SQL Server)](sql-server-buffer-manager-object.md)   
- [Opciones de configuración de la memoria del servidor](../../database-engine/configure-windows/server-memory-server-configuration-options.md)
- [Guía de arquitectura de administración de memoria](../../relational-databases/memory-management-architecture-guide.md)
