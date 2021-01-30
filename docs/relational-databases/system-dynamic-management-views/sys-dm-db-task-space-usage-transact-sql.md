---
description: sys.dm_db_task_space_usage (Transact-SQL)
title: sys.dm_db_task_space_usage (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_db_task_space_usage_TSQL
- sys.dm_db_task_space_usage_TSQL
- dm_db_task_space_usage
- sys.dm_db_task_space_usage
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_task_space_usage dynamic management view
ms.assetid: fb0c87e5-43b9-466a-a8df-11b3851dc6d0
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6a35bbdb72e5efde46e29233d6378bb2fbadd30d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204834"
---
# <a name="sysdm_db_task_space_usage-transact-sql"></a>sys.dm_db_task_space_usage (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve la actividad de asignación y desasignación de páginas por tarea de la base de datos.  
  
> [!NOTE]  
>  Esta vista solo es aplicable a la [base de datos Tempdb](../../relational-databases/databases/tempdb-database.md).  
  
> [!NOTE]  
>  Para llamar a este método desde [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , use el nombre **Sys.dm_pdw_nodes_db_task_space_usage**.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**session_id**|**smallint**|Id. de sesión.|  
|**id_de_solicitud**|**int**|Id. de solicitud en la sesión.<br /><br /> Una solicitud también se llama lote y puede contener una o más consultas. Una sesión puede tener varias solicitudes activas al mismo tiempo. Cada consulta en la solicitud puede iniciar varios subprocesos (tareas) si se utiliza un plan de ejecución paralelo.|  
|**exec_context_id**|**int**|Id. del contexto de ejecución de la tarea. Para obtener más información, vea [sys.dm_os_tasks &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md).|  
|**database_id**|**smallint**|Id. de la base de datos.|  
|**user_objects_alloc_page_count**|**bigint**|Número de páginas reservadas o asignadas para objetos de usuario por esta tarea.|  
|**user_objects_dealloc_page_count**|**bigint**|Número de páginas cuya reserva o asignación para objetos de usuario ha sido cancelada por esta tarea.|  
|**internal_objects_alloc_page_count**|**bigint**|Número de páginas reservadas o asignadas para objetos internos por esta tarea.|  
|**internal_objects_dealloc_page_count**|**bigint**|Número de páginas cuya reserva o asignación para objetos internos ha sido cancelada por esta tarea.|  
|**pdw_node_id**|**int**|**Se aplica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificador del nodo en el que se encuentra esta distribución.|  
  
## <a name="permissions"></a>Permisos

En [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiere el `VIEW SERVER STATE` permiso.   
En SQL Database objetivos de servicio Basic, S0 y S1, y para las bases de datos de grupos elásticos, `Server admin` `Azure Active Directory admin` se requiere la cuenta o. En el resto de los objetivos del servicio SQL Database, `VIEW DATABASE STATE` se requiere el permiso en la base de datos.   

## <a name="remarks"></a>Observaciones  
 Las páginas IAM no se incluyen en ninguno de los recuentos de páginas incluidos en esta vista.  
  
 Los contadores de páginas se inicializan en cero (0) al principio de la solicitud. Estos valores se agregan en el nivel de sesión cuando finaliza la solicitud. Para obtener más información, consulte [sys.dm_db_session_space_usage &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-session-space-usage-transact-sql.md).  
  
 El almacenamiento en caché de tablas de trabajo, el almacenamiento en caché de tablas temporales y las operaciones DROP diferidas afectan al número de páginas asignadas y con asignación cancelada en una tarea específica.  
  
## <a name="user-objects"></a>Objetos de usuario  
 Los objetos siguientes se incluyen en los contadores de páginas de objetos de usuario:  
  
-   Índices y tablas definidos por el usuario  
  
-   Índices y tablas del sistema  
  
-   Índices y tablas temporales globales  
  
-   Índices y tablas temporales locales  
  
-   Variables de tabla  
  
-   Tablas devueltas en las funciones con valores de tabla.  
  
## <a name="internal-objects"></a>Objetos internos  
 Los objetos internos solo están en **tempdb**. Los objetos siguientes se incluyen en los contadores de páginas de objetos internos:  
  
-   Tablas de trabajo para operaciones de cola o cursor y almacenamiento de objetos grandes (LOB) temporales  
  
-   Archivos de trabajo para operaciones como la combinación hash  
  
-   Ordenaciones  
  
## <a name="physical-joins"></a>Combinaciones físicas  
 ![Combinaciones físicas de sys.dm_db_session_task_usage](../../relational-databases/system-dynamic-management-views/media/join-dm-db-task-space-usage-1.gif "Combinaciones físicas de sys.dm_db_session_task_usage")  
  
## <a name="relationship-cardinalities"></a>Cardinalidades de relación  
  
|From|En|Relación|  
|----------|--------|------------------|  
|dm_db_task_space_usage.request_id|dm_exec_requests.request_id|Uno a uno|  
|dm_db_task_space_usage.session_id|dm_exec_requests.session_id|Uno a uno|  
  
## <a name="see-also"></a>Consulte también  
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vistas de administración dinámica relacionadas con bases de datos &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)   
 [sys.dm_exec_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md)   
 [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)   
 [sys.dm_os_tasks &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md)   
 [sys.dm_db_session_space_usage &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-session-space-usage-transact-sql.md)   
 [sys.dm_db_file_space_usage &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-file-space-usage-transact-sql.md)  
  
  


