---
description: sys.dm_exec_background_job_queue (Transact-SQL)
title: sys.dm_exec_background_job_queue (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_exec_background_job_queue
- sys.dm_exec_background_job_queue_TSQL
- dm_exec_background_job_queue_TSQL
- sys.dm_exec_background_job_queue
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_background_job_queue dynamic management function
ms.assetid: 05d9884f-b74c-4e3c-a23b-c90c1ea5ef02
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: cd67ec5a14684435e506f97bdae59bb5a3a46ff2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99107641"
---
# <a name="sysdm_exec_background_job_queue-transact-sql"></a>sys.dm_exec_background_job_queue (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve una fila por cada trabajo del procesador de consultas que está programado para ejecución asincrónica (en segundo plano).  
  
> **NOTA** Para llamar a este método desde **[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)]** o **[!INCLUDE[ssPDW](../../includes/sspdw-md.md)]** , use el nombre **Sys.dm_pdw_nodes_exec_background_job_queue**.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**time_queued**|**datetime**|Hora en que se agregó el trabajo a la cola.|  
|**job_id**|**int**|Identificador del trabajo.|  
|**database_id**|**int**|Base de datos en que se va a ejecutar el trabajo.|  
|**object_id1**|**int**|El valor depende del tipo de trabajo. Para obtener más información, vea la sección Comentarios.|  
|**object_id2**|**int**|El valor depende del tipo de trabajo. Para obtener más información, vea la sección Comentarios.|  
|**object_id3**|**int**|El valor depende del tipo de trabajo. Para obtener más información, vea la sección Comentarios.|  
|**object_id4**|**int**|El valor depende del tipo de trabajo. Para obtener más información, vea la sección Comentarios.|  
|**error_code**|**int**|Código de error si el trabajo se ha vuelto a insertar debido a un error. NULL si se ha suspendido, no se ha seleccionado o se ha completado.|  
|**request_type**|**smallint**|Tipo de trabajo solicitado.|  
|**retry_count**|**smallint**|Número de veces que el trabajo se ha seleccionado de la cola y se ha vuelto a insertar porque faltaban recursos u otro motivo.|  
|**in_progress**|**smallint**|Indica si el trabajo ha empezado a ejecutarse.<br /><br /> 1 = Iniciado<br /><br /> 0 = En espera|  
|**session_id**|**smallint**|Identificador de la sesión.|  
|**pdw_node_id**|**int**|**Se aplica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificador del nodo en el que se encuentra esta distribución.|  
  
## <a name="permissions"></a>Permisos

En [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiere el `VIEW SERVER STATE` permiso.   
En SQL Database objetivos de servicio Basic, S0 y S1, y para las bases de datos de grupos elásticos, `Server admin` `Azure Active Directory admin` se requiere la cuenta o. En el resto de los objetivos del servicio SQL Database, `VIEW DATABASE STATE` se requiere el permiso en la base de datos.   
  
## <a name="remarks"></a>Observaciones  
 Esta vista devuelve información solo para los trabajos de estadísticas de actualización asincrónica. Para obtener más información sobre las estadísticas de actualización asincrónica, vea [estadísticas](../../relational-databases/statistics/statistics.md).  
  
 Los valores de **object_id1** a través de **object_id4** dependen del tipo de la solicitud de trabajo. En la tabla siguiente se resume el significado de estas columnas para los diferentes tipos de trabajos.  
  
|Tipo de solicitud|object_id1|object_id2|object_id3|object_id4|  
|------------------|-----------------|-----------------|-----------------|-----------------|  
|Estadísticas de actualización asincrónicas|Id. de tabla o vista|Id. de estadística|No se usa|No se usa|  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se devuelve el número de trabajos asincrónicos activos en la cola en segundo plano para cada base de datos de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```  
SELECT DB_NAME(database_id) AS [Database], COUNT(*) AS [Active Async Jobs]  
FROM sys.dm_exec_background_job_queue  
WHERE in_progress = 1  
GROUP BY database_id;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Funciones y vistas de administración dinámica relacionadas con la ejecución &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
 [Utilizar las estadísticas para mejorar el rendimiento de las consultas](../../relational-databases/statistics/statistics.md)   
 [KILL STATS JOB &#40;Transact-SQL&#41;](../../t-sql/language-elements/kill-stats-job-transact-sql.md)  
  
  



