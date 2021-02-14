---
description: sys.dm_os_sys_info (Transact-SQL)
title: sys.dm_os_sys_info (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/24/2018
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_os_sys_info_TSQL
- dm_os_sys_info
- dm_os_sys_info_TSQL
- sys.dm_os_sys_info
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_sys_info dynamic management view
- time [SQL Server], instance started
- starting time
ms.assetid: 20f6bc9c-839a-4fa4-b3f3-a6c47d1b69af
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a4f43c899f066713d3a2b4277e3c000c7d5bcb35
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2021
ms.locfileid: "100344345"
---
# <a name="sysdm_os_sys_info-transact-sql"></a>sys.dm_os_sys_info (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve diversos datos útiles sobre el equipo y los recursos disponibles y consumidos por [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)].  
  
> **Nota:** Para llamar a este método desde [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , use el nombre **Sys.dm_pdw_nodes_os_sys_info**.  
  
|Nombre de la columna|Tipo de datos|Descripción y notas específicas de la versión |  
|-----------------|---------------|-----------------|  
|**cpu_ticks**|**bigint**|Especifica el contador actual de CPU. Los tics de CPU se obtienen del contador de RDTSC del procesador. Es un número que aumenta regularmente. No acepta valores NULL.|  
|**ms_ticks**|**bigint**|Especifica el número de milisegundos transcurridos desde que se inició el equipo. No acepta valores NULL.|  
|**cpu_count**|**int**|Especifica el número de CPUs lógicas en el sistema. No acepta valores NULL.|  
|**hyperthread_ratio**|**int**|Especifica la proporción del número de núcleos lógicos o físicos expuestos por un paquete de procesadores físicos. No acepta valores NULL.|  
|**physical_memory_in_bytes**|**bigint**|**Se aplica a:** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] a través de [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] .<br /><br /> Especifica la cantidad total de memoria física en el equipo. No acepta valores NULL.|  
|**physical_memory_kb**|**bigint**|**Se aplica a:** [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.<br /><br /> Especifica la cantidad total de memoria física en el equipo. No acepta valores NULL.|  
|**virtual_memory_in_bytes**|**bigint**|**Se aplica a:** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] a través de [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] .<br /><br /> Cantidad de memoria virtual disponible para el proceso en modo usuario. Se puede utilizar para determinar si SQL Server se inició utilizando un modificador 3-GB.|  
|**virtual_memory_kb**|**bigint**|**Se aplica a:** [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.<br /><br /> Especifica la cantidad total de espacio de direcciones virtuales disponible para el proceso en modo usuario. No acepta valores NULL.|  
|**bpool_committed**|**int**|**Se aplica a:** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] a través de [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] .<br /><br /> Representa la memoria confirmada en kilobytes (KB) en el administrador de memoria. No incluye la memoria reservada del administrador de memoria. No acepta valores NULL.|  
|**committed_kb**|**int**|**Se aplica a:** [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.<br /><br /> Representa la memoria confirmada en kilobytes (KB) en el administrador de memoria. No incluye la memoria reservada del administrador de memoria. No acepta valores NULL.|  
|**bpool_commit_target**|**int**|**Se aplica a:** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] a través de [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] .<br /><br /> Representa la cantidad de memoria, en kilobytes (KB), que el administrador de memoria de SQL Server puede utilizar.|  
|**committed_target_kb**|**int**|**Se aplica a:** [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.<br /><br /> Representa la cantidad de memoria, en kilobytes (KB), que el administrador de memoria de SQL Server puede utilizar. La cantidad de destino se calcula utilizando una serie de entradas como las siguientes:<br /><br /> -el estado actual del sistema, incluida su carga<br /><br /> -la memoria solicitada por los procesos actuales<br /><br /> -la cantidad de memoria instalada en el equipo<br /><br /> -parámetros de configuración<br /><br /> Si **committed_target_kb** es mayor que **committed_kb**, el administrador de memoria intentará obtener memoria adicional. Si **committed_target_kb** es menor que **committed_kb**, el administrador de memoria intentará reducir la cantidad de memoria confirmada. El **committed_target_kb** siempre incluye memoria robada y reservada. No acepta valores NULL.|  
|**bpool_visible**|**int**|**Se aplica a:** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] a través de [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] .<br /><br /> Número de búferes de 8 KB del grupo de búferes accesibles directamente en el espacio de direcciones virtuales de proceso. Cuando no se utilizan las Extensiones de ventana de dirección (AWE), si el grupo de búferes ha obtenido el destino de memoria (bpool_committed = bpool_commit_target), el valor de bpool_visible es igual al valor de bpool_committed. Si se utiliza AWE en una versión de 32 bits de SQL Server, bpool_visible representa el tamaño de la ventana de la asignación AWE utilizada para tener acceso a la memoria física asignada por el grupo de búferes. El tamaño de esta ventana de asignación está limitado por el espacio de direcciones de proceso y, por tanto, la cantidad visible será inferior a la asignada, y puede verse reducida aún más por componentes internos que consumen memoria para propósitos no relacionados con las páginas de base de datos. Si el valor de bpool_visible es demasiado bajo, es posible que se produzcan errores de memoria insuficiente.|  
|**visible_target_kb**|**int**|**Se aplica a:** [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.<br /><br /> Es igual que **committed_target_kb**. No acepta valores NULL.|  
|**stack_size_in_bytes**|**int**|Especifica el tamaño de la pila de llamadas de cada subproceso creado por [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. No acepta valores NULL.|  
|**os_quantum**|**bigint**|Representa el cuanto de una tarea no preferente medido en milisegundos. Quantum (en segundos) = **os_quantum** /velocidad del reloj de la CPU. No acepta valores NULL.|  
|**os_error_mode**|**int**|Especifica el modo de error para el proceso de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. No acepta valores NULL.|  
|**os_priority_class**|**int**|Especifica la clase de prioridad del proceso de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Acepta valores NULL.<br /><br /> 32 = Normal (el registro de errores indicará que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se está iniciando con una prioridad base normal (=7)).<br /><br /> 128 = Alto (el registro de errores indicará que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se está ejecutando con una prioridad base alta.  (=13).)<br /><br /> Para más información, consulte [Establecer la opción de configuración del servidor Aumento de prioridad](../../database-engine/configure-windows/configure-the-priority-boost-server-configuration-option.md).|  
|**max_workers_count**|**int**|Representa el número máximo de subprocesos de trabajo que se pueden crear. No acepta valores NULL.|  
|**scheduler_count**|**int**|Representa el número de programadores de usuario configurados en el proceso de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. No acepta valores NULL.|  
|**scheduler_total_count**|**int**|Representa el número total de programadores en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. No acepta valores NULL.|  
|**deadlock_monitor_serial_number**|**int**|Especifica el identificador de la secuencia del monitor de interbloqueos actual. No acepta valores NULL.|  
|**sqlserver_start_time_ms_ticks**|**bigint**|Representa el número de **ms_tick** cuando se [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] inició por última vez. Comparar a la columna actual ms_ticks. No acepta valores NULL.|  
|**sqlserver_start_time**|**datetime**|Especifica la fecha y la hora del sistema local que se [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] iniciaron por última vez. No acepta valores NULL.|  
|**affinity_type**|**int**|**Se aplica a:** [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] y versiones posteriores.<br /><br /> Especifica el tipo de la afinidad de proceso de la CPU de servidor actualmente en uso. No acepta valores NULL. Para obtener más información, vea [ALTER Server CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-configuration-transact-sql.md).<br /><br /> 1 = MANUAL<br /><br /> 2 = AUTO|  
|**affinity_type_desc**|**varchar(60)**|**Se aplica a:** [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] y versiones posteriores.<br /><br /> Describe el **affinity_type** columna. No acepta valores NULL.<br /><br /> MANUAL = la afinidad se ha establecido para al menos una CPU.<br /><br /> AUTO = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puede mover libremente los subprocesos entre las CPU.|  
|**process_kernel_time_ms**|**bigint**|**Se aplica a:** [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] y versiones posteriores.<br /><br /> Tiempo total en milisegundos que han tardado todos los subprocesos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en modo kernel. Este valor puede ser mayor que el de un único reloj de procesador porque incluye el tiempo para todos los procesadores del servidor. No acepta valores NULL.|  
|**process_user_time_ms**|**bigint**|**Se aplica a:** [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] y versiones posteriores.<br /><br /> Tiempo total en milisegundos que han tardado todos los subprocesos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en modo usuario. Este valor puede ser mayor que el de un único reloj de procesador porque incluye el tiempo para todos los procesadores del servidor. No acepta valores NULL.|  
|**time_source**|**int**|**Se aplica a:** [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] y versiones posteriores.<br /><br /> Indica la API que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utiliza para recuperar el tiempo de reloj. No acepta valores NULL.<br /><br /> 0 = QUERY_PERFORMANCE_COUNTER<br /><br /> 1 = MULTIMEDIA_TIMER|  
|**time_source_desc**|**nvarchar(60)**|**Se aplica a:** [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] y versiones posteriores.<br /><br /> Describe el **time_source** columna. No acepta valores NULL.<br /><br /> QUERY_PERFORMANCE_COUNTER = la API de [QueryPerformanceCounter](/windows/win32/api/profileapi/nf-profileapi-queryperformancecounter) recupera el tiempo de reloj.<br /><br /> MULTIMEDIA_TIMER = API de [temporizador multimedia](/previous-versions//ms713418(v=vs.85)) que recupera el tiempo de reloj.|  
|**virtual_machine_type**|**int**|**Se aplica a:** [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] y versiones posteriores.<br /><br /> Indica si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se ejecuta en un entorno virtualizado.  No acepta valores NULL.<br /><br /> 0 = NONE<br /><br /> 1 = HYPERVISOR<br /><br /> 2 = OTHER|  
|**virtual_machine_type_desc**|**nvarchar(60)**|**Se aplica a:** [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] y versiones posteriores.<br /><br /> Describe el **virtual_machine_type** columna. No acepta valores NULL.<br /><br /> NINGUNO = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no se está ejecutando en una máquina virtual.<br /><br /> HIPERVISOR = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se está ejecutando en una máquina virtual hospedada por un sistema operativo que ejecuta el hipervisor (un sistema operativo host que emplea la virtualización asistida por hardware).<br /><br /> OTRO = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se está ejecutando en una máquina virtual hospedada por un sistema operativo que no emplea Asistente de hardware como Microsoft Virtual PC.|  
|**softnuma_configuration**|**int**|**Se aplica a:** [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores.<br /><br /> Especifica la forma en que se configuran los nodos NUMA. No acepta valores NULL.<br /><br /> 0 = desactivado indica el valor predeterminado de hardware<br /><br /> 1 = Soft-NUMA automatizado<br /><br /> 2 = Soft-NUMA manual a través del registro|  
|**softnuma_configuration_desc**|**nvarchar(60)**|**Se aplica a:** [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores.<br /><br /> OFF = la característica Soft-NUMA está desactivada<br /><br /> ON = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] determina automáticamente los tamaños de nodo Numa de Soft-Numa<br /><br /> MANUAL = NUMA de software configurado manualmente|
|**process_physical_affinity**|**nvarchar (a.** |**Se aplica a:** A partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] .<br /><br />Información aún. |
|**sql_memory_model**|**int**|**Se aplica a:** [!INCLUDE[sssql11](../../includes/sssql11-md.md)] SP4, [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 y versiones posteriores.<br /><br />Especifica el modelo de memoria utilizado por [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para asignar memoria. No acepta valores NULL.<br /><br />1 = modelo de memoria convencional<br />2 = bloquear páginas en la memoria<br /> 3 = páginas grandes en memoria|
|**sql_memory_model_desc**|**nvarchar(120)**|**Se aplica a:** [!INCLUDE[sssql11](../../includes/sssql11-md.md)] SP4, [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 y versiones posteriores.<br /><br />Especifica el modelo de memoria utilizado por [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para asignar memoria. No acepta valores NULL.<br /><br />  =  Convencional [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa el modelo de memoria convencional para asignar memoria. Este es el modelo de memoria de SQL predeterminado cuando la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cuenta de servicio no tiene privilegios de bloqueo de páginas en memoria durante el inicio.<br />  =  LOCK_PAGES [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] está utilizando bloquear páginas en la memoria para asignar memoria. Este es el administrador de memoria de SQL predeterminado cuando SQL Server cuenta de servicio posee el privilegio bloquear páginas en memoria durante SQL Server Inicio.<br />   =  LARGE_PAGES [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utiliza páginas grandes en memoria para asignar memoria. SQL Server utiliza el asignador de páginas grandes para asignar memoria solo con Enterprise Edition cuando SQL Server cuenta de servicio posee el privilegio bloquear páginas en memoria durante el inicio del servidor y cuando la marca de seguimiento 834 está activada.|
|**pdw_node_id**|**int**|**Se aplica a:** [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificador del nodo en el que se encuentra esta distribución.|  
|**socket_count** |**int** | **Se aplica a:** [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 y versiones posteriores<br /><br />Especifica el número de sockets de procesador disponibles en el sistema. |  
|**cores_per_socket** |**int** | **Se aplica a:** [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 y versiones posteriores<br /><br />Especifica el número de procesadores por socket disponibles en el sistema. |  
|**numa_node_count** |**int** | **Se aplica a:** [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 y versiones posteriores<br /><br />Especifica el número de nodos Numa disponibles en el sistema. Esta columna incluye los nodos Numa físicos y los nodos Numa de software. |  
  
## <a name="permissions"></a>Permisos

En [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiere el `VIEW SERVER STATE` permiso.   
En SQL Database objetivos de servicio Basic, S0 y S1, y para las bases de datos de grupos elásticos, se requiere la cuenta de [Administrador del servidor](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) o la cuenta de [Administrador de Azure Active Directory](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . En el resto de los objetivos del servicio SQL Database, `VIEW DATABASE STATE` se requiere el permiso en la base de datos.   

## <a name="see-also"></a>Consulte también  
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [SQL Server vistas de administración dinámica relacionadas con el sistema operativo &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
