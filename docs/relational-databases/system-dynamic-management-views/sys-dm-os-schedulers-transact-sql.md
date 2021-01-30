---
description: sys.dm_os_schedulers (Transact-SQL)
title: sys.dm_os_schedulers (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_os_schedulers
- sys.dm_os_schedulers_TSQL
- sys.dm_os_schedulers
- dm_os_schedulers_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_schedulers dynamic management view
ms.assetid: 3a09d81b-55d5-416f-9cda-1a3a5492abe0
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0384c81a0ff80e9dda9c97d793c99d1ee8e28f54
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184801"
---
# <a name="sysdm_os_schedulers-transact-sql"></a>sys.dm_os_schedulers (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve una fila por programador en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] donde cada programador está asignado a un determinado procesador. Use esta vista para supervisar la condición de un programador o identificar tareas descontroladas. Para obtener más información sobre los programadores, vea la [Guía de arquitectura de subprocesos y tareas](../../relational-databases/thread-and-task-architecture-guide.md).  
  
> [!NOTE]  
>  Para llamar a este método desde [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , use el nombre **Sys.dm_pdw_nodes_os_schedulers**.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|scheduler_address|**varbinary(8**|Dirección de memoria del programador. No admite valores NULL.|  
|parent_node_id|**int**|Identificador del nodo al que pertenece el programador, también denominado nodo primario. Representa un nodo de acceso de memoria no uniforme (NUMA). No admite valores NULL.|  
|scheduler_id|**int**|IDENTIFICADOR del programador. Todos los programadores que se utilizan para ejecutar las consultas normales tienen números de identificación inferiores a 1048576. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utiliza internamente todos los programadores con identificadores mayores o iguales que 1048576, como el programador de conexión de administrador dedicada. No admite valores NULL.|  
|cpu_id|**smallint**|Identificador de CPU asignado al programador.<br /><br /> No admite valores NULL.<br /><br /> **Nota:** 255 no indica ninguna afinidad como en [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] . Vea [sys.dm_os_threads &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-threads-transact-sql.md) para obtener información adicional sobre la afinidad.|  
|status|**nvarchar(60)**|Indica el estado del programador. Puede ser uno de los siguientes valores:<br /><br /> -OCULTO EN LÍNEA<br />-OCULTO SIN CONEXIÓN<br />-VISIBLE EN LÍNEA<br />-VISIBLE SIN CONEXIÓN<br />-VISIBLE EN LÍNEA (DAC)<br />-HOT_ADDED<br /><br /> No admite valores NULL.<br /><br /> Los programadores HIDDEN se utilizan para procesar solicitudes internas de [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Los programadores VISIBLE se utilizan para procesar solicitudes de usuario.<br /><br /> Los programadores OFFLINE se asignan a procesadores sin conexión en la máscara de afinidad y, por tanto, no se utilizan para procesar solicitudes. Los programadores ONLINE se asignan a procesadores con conexión en la máscara de afinidad y están disponibles para procesar subprocesos.<br /><br /> DAC indica que el programador se está ejecutando con una conexión de administrador dedicada.<br /><br /> HOT ADDED indica los programadores que se agregaron en respuesta a un evento de CPU instalada en cliente.|  
|is_online|**bit**|Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] está configurado para utilizar solo algunos de los procesadores disponibles en el servidor, esta configuración puede significar que algunos programadores están asignados a procesadores que no están en la máscara de afinidad. Si así fuera, esta columna devuelve 0. Este valor indica que el programador no se utiliza para procesar consultas o lotes.<br /><br /> No admite valores NULL.|  
|is_idle|**bit**|1 = El programador está inactivo. No se está ejecutando ningún trabajador. No admite valores NULL.|  
|preemptive_switches_count|**int**|Número de veces que los trabajadores de este programador han cambiado al modo preferente.<br /><br /> Para ejecutar código situado fuera de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (por ejemplo, en procedimientos almacenados extendidos y consultas distribuidas), se tiene que ejecutar un subproceso fuera del control del programador no preferente. Para hacerlo, un trabajador se cambia al modo preferente.|  
|context_switches_count|**int**|Número de cambios de contexto que se han producido en este programador. No admite valores NULL.<br /><br /> Para permitir que se ejecuten otros trabajadores, el trabajador en ejecución actual tiene que renunciar al control del programador o el cambio de contexto.<br /><br /> **Nota:** Si un trabajador produce el programador y se coloca en la cola de ejecutables y luego no encuentra ningún otro trabajo, el trabajo se seleccionará por sí mismo. En este caso, context_switches_count no se actualiza, sino que se actualiza yield_count.|  
|idle_switches_count|**int**|Número de veces que el programador ha esperado por un evento mientras estaba inactivo. Esta columna es similar a context_switches_count. No admite valores NULL.|  
|current_tasks_count|**int**|Número de tareas actuales que están asociadas con este programador. Este recuento incluye lo siguiente:<br /><br /> -Tareas que esperan a que un trabajador las ejecute.<br />-Tareas actualmente en espera o en ejecución (en estado suspendido o ejecutable).<br /><br /> Cuando una tarea se completa, este contador disminuye. No admite valores NULL.|  
|runnable_tasks_count|**int**|Número de trabajadores, con tareas asignadas, que están esperando a ser programados en la cola de ejecutables. No admite valores NULL.|  
|current_workers_count|**int**|Número de trabajadores que están asociados a este programador. Este recuento incluye a los trabajadores que no están asignados a ninguna tarea. No admite valores NULL.|  
|active_workers_count|**int**|Número de trabajadores activos. Un trabajador activo nunca es preferente, debe tener una tarea asociada, y está ejecutándose, es ejecutable o está suspendido. No admite valores NULL.|  
|work_queue_count|**bigint**|Número de tareas en la cola de tareas pendientes. Estas tareas esperan a que un trabajador las ejecute. No admite valores NULL.|  
|pending_disk_io_count|**int**|Número de E/S pendientes que esperan a completarse. Cada programador tiene una lista de E/S pendientes que se comprueban para determinar si se han completado cada vez que hay un cambio de contexto. El recuento aumenta cuando se inserta la solicitud. Este recuento disminuye cuando la solicitud se completa. Este número no indica el estado de las operaciones de E/S. No admite valores NULL.|  
|load_factor|**int**|Valor interno que indica la carga detectada en este programador. Este valor se utiliza para determinar si una nueva tarea debe colocarse es este programador o en otro. Este valor es útil para tareas de depuración cuando parece que los programadores no están cargados equitativamente. La decisión de enrutamiento se toma en función de la carga del programador. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] también utiliza un factor de carga de nodos y programadores para ayudar a determinar la mejor ubicación para adquirir recursos. Cuando una tarea se pone en cola, el factor de carga aumenta. Cuando una tarea se completa, el factor de carga disminuye. El uso de los factores de carga ayuda a que el SO de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] equilibre mejor la carga de trabajo. No admite valores NULL.|  
|yield_count|**int**|Valor interno que se utiliza para indicar el progreso en este programador. El monitor de programadores utiliza este valor para determinar si un trabajador del programador no está generando otros trabajadores a tiempo. Este valor no indica que el trabajador o la tarea se transfirieron a un nuevo trabajador. No admite valores NULL.|  
|last_timer_activity|**bigint**|En tics de CPU, la última vez que el programador comprobó la cola del temporizador del programador. No admite valores NULL.|  
|failed_to_create_worker|**bit**|Se establece en 1 si no se pudo crear un trabajador en este programador. Generalmente se produce debido a restricciones de memoria. Acepta valores NULL.|  
|active_worker_address|**varbinary(8**|Dirección de memoria del trabajador que está activo actualmente. Acepta valores NULL. Para obtener más información, vea [sys.dm_os_workers &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-workers-transact-sql.md).|  
|memory_object_address|**varbinary(8**|Dirección de memoria del objeto de memoria del programador. No acepta valores NULL.|  
|task_memory_object_address|**varbinary(8**|Dirección de memoria del objeto de memoria de la tarea. No admite valores NULL. Para obtener más información, vea [sys.dm_os_memory_objects &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-objects-transact-sql.md).|  
|quantum_length_us|**bigint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)] Expone el cuanto del programador que utiliza SQLOS.|  
| total_cpu_usage_ms |**bigint**|**Válido para** [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] y versiones posteriores. <br><br> CPU total consumida por este programador tal y como lo indican los trabajadores no preferentes. No admite valores NULL.|
|total_cpu_idle_capped_ms|**bigint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)] Indica que la limitación se basa en el [objetivo de nivel de servicio](/azure/sql-data-warehouse/what-is-a-data-warehouse-unit-dwu-cdwu#service-level-objective); siempre será 0 para las versiones que no sean de Azure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Acepta valores NULL.|
|total_scheduler_delay_ms|**bigint**|**Válido para** [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] y versiones posteriores. <br><br> El tiempo entre un trabajador que está desactivando y otro que está cambiando. Puede deberse a que los trabajadores preferentemente retrasan la programación del siguiente trabajo no preferente o debido a los subprocesos de programación del sistema operativo de otros procesos. No admite valores NULL.|
|ideal_workers_limit|**int**|**Válido para** [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] y versiones posteriores. <br><br> El número de trabajadores que idealmente deberían estar en el programador. Si los trabajadores actuales superan el límite debido a la carga de tareas desequilibrada, una vez que estén inactivas, se recortarán. No admite valores NULL.|
|pdw_node_id|**int**|**Se aplica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificador del nodo en el que se encuentra esta distribución.|  
  
## <a name="permissions"></a>Permisos
En [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiere el `VIEW SERVER STATE` permiso.   
En SQL Database objetivos de servicio Basic, S0 y S1, y para las bases de datos de grupos elásticos, `Server admin` `Azure Active Directory admin` se requiere la cuenta o. En el resto de los objetivos del servicio SQL Database, `VIEW DATABASE STATE` se requiere el permiso en la base de datos.   

## <a name="examples"></a>Ejemplos  
  
### <a name="a-monitoring-hidden-and-nonhidden-schedulers"></a>A. Supervisar programadores ocultos y no ocultos  
 En la siguiente consulta se muestra el estado de los trabajadores y las tareas en todos los programadores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Esta consulta se ejecutó en un sistema que incluye lo siguiente:  
  
-   Dos procesadores (CPU)  
  
-   Dos nodos (NUMA)  
  
-   Una CPU por nodo NUMA  
  
-   Máscara de afinidad establecida en `0x03`.  
  
```sql  
SELECT  
    scheduler_id,  
    cpu_id,  
    parent_node_id,  
    current_tasks_count,  
    runnable_tasks_count,  
    current_workers_count,  
    active_workers_count,  
    work_queue_count  
  FROM sys.dm_os_schedulers;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
scheduler_id cpu_id parent_node_id current_tasks_count  
------------ ------ -------------- -------------------  
0            1      0              9                    
257          255    0              1                    
1            0      1              10                   
258          255    1              1                    
255          255    32             2                    
  
runnable_tasks_count current_workers_count  
-------------------- ---------------------  
0                    11                     
0                    1                      
0                    18                     
0                    1                      
0                    3                      
  
active_workers_count work_queue_count  
-------------------- --------------------  
6                    0  
1                    0  
8                    0  
1                    0  
1                    0  
```  
  
 La salida proporciona la siguiente información:  
  
-   Hay cinco programadores. Dos programadores tienen un valor de identificador < 1048576. Los programadores con ID >= 1048576 se conocen como programadores ocultos. El programador `255` representa la conexión de administrador dedicada (DAC). Existe un programador DAC por sesión. Los monitores de recursos que coordinan la presión de memoria utilizan los programadores `257` y `258`, uno por nodo NUMA.  
  
-   La salida muestra 23 tareas activas. Estas tareas incluyen solicitudes de usuario además de las tareas de administración de recursos iniciadas por [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ejemplos de tareas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] son RESOURCE MONITOR (una por nodo NUMA), LAZY WRITER (una por nodo NUMA), LOCK MONITOR, CHECKPOINT y LOG WRITER.  
  
-   El nodo NUMA `0` se asigna a la CPU `1` y el nodo NUMA `1` se asigna a la CPU `0`. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se inicia normalmente en un nodo NUMA distinto del nodo 0.  
  
-   Cuando `runnable_tasks_count` devuelve `0`, no hay ninguna tarea activa en ejecución. Sin embargo, pueden existir sesiones activas.  
  
-   El programador `255` que representa la conexión DAC incluye `3` trabajadores asociados. Estos trabajadores se asignan en el inicio de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y no cambian. Se utilizan únicamente para procesar consultas DAC. Las dos tareas de este programador representan un administrador de conexiones y un trabajador inactivo.  
  
-   `active_workers_count` representa todos los trabajadores que tienen tareas asociadas y que se ejecutan en modo no preferente. Algunas tareas, como las escuchas de red, se ejecutan con programas preferentes.  
  
-   Los programadores ocultos no procesan solicitudes habituales de usuario. El programador DAC es la excepción. Este programador dispone de un subproceso para ejecutar solicitudes.  
  
### <a name="b-monitoring-nonhidden-schedulers-in-a-busy-system"></a>B. Supervisar programadores no ocultos en un sistema ocupado  
 En la consulta siguiente se muestra el estado de programadores no ocultos con mucha carga en los que existen más solicitudes de las que pueden controlar los trabajadores disponibles. En este ejemplo tienen asignadas tareas 256 trabajadores. Algunas tareas están esperando su asignación a un trabajador. El recuento bajo de ejecutables implica que varias tareas están esperando un recurso.  
  
> [!NOTE]  
>  Puede conocer el estado de los trabajadores si consulta sys.dm_os_workers. Para obtener más información, vea [sys.dm_os_workers &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-workers-transact-sql.md).  
  
 Esta es la consulta:  
  
```sql  
SELECT  
    scheduler_id,  
    cpu_id,  
    current_tasks_count,  
    runnable_tasks_count,  
    current_workers_count,  
    active_workers_count,  
    work_queue_count  
  FROM sys.dm_os_schedulers  
  WHERE scheduler_id < 255;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
scheduler_id current_tasks_count runnable_tasks_count  
------------ ------------------- --------------------  
0            144                 0                     
1            147                 1                     
  
current_workers_count active_workers_count work_queue_count  
--------------------- -------------------- --------------------  
128                   125                  16  
128                   126                  19  
```  
  
 Por comparación, el resultado siguiente muestra varias tareas ejecutables donde ninguna tarea está esperando para obtener un trabajador. El valor de `work_queue_count` es `0` en ambos programadores.  
  
```  
scheduler_id current_tasks_count runnable_tasks_count  
------------ ------------------- --------------------  
0            107                 98                    
1            110                 100                   
  
current_workers_count active_workers_count work_queue_count  
--------------------- -------------------- --------------------  
128                   104                  0  
128                   108                  0  
```  
  
## <a name="see-also"></a>Consulte también  
 [SQL Server vistas de administración dinámica relacionadas con el sistema operativo &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
