---
description: sys.query_store_plan (Transact-SQL)
title: sys.query_store_plan (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/23/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- QUERY_STORE_PLAN_TSQL
- SYS.QUERY_STORE_PLAN
- SYS.QUERY_STORE_PLAN_TSQL
- QUERY_STORE_PLAN
dev_langs:
- TSQL
helpviewer_keywords:
- query_store_plan catalog view
- sys.query_store_plan catalog view
ms.assetid: b4d05439-6360-45db-b1cd-794f4a64935e
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||= azure-sqldw-latest||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 36ad9ced09332ec0b8d7659e572f4875aa49ec3d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204537"
---
# <a name="sysquery_store_plan-transact-sql"></a>sys.query_store_plan (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  Contiene información acerca de cada plan de ejecución asociado a una consulta.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|
|**plan_id**|**bigint**|Clave principal.|  
|**query_id**|**bigint**|Clave externa. Combina con [sys.query_store_query &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-query-transact-sql.md).|  
|**plan_group_id**|**bigint**|IDENTIFICADOR del grupo de planes. Las consultas de cursor suelen requerir varios planes (rellenar y capturar). Los planes de rellenado y captura que se compilan juntos se encuentran en el mismo grupo.<br /><br /> 0 significa que el plan no está en un grupo.|  
|**engine_version**|**nvarchar(32)**|Versión del motor que se usa para compilar el plan en formato **' Major. minor. Build. revision '** .|  
|**compatibility_level**|**smallint**|Nivel de compatibilidad de la base de datos a la que se hace referencia en la consulta.|  
|**query_plan_hash**|**Binary(8**|Hash MD5 del plan individual.|  
|**query_plan**|**nvarchar(max)**|SHOWPLAN XML para el plan de consulta.|  
|**is_online_index_plan**|**bit**|El plan se usó durante una generación de índice en línea. <br/>**Nota:** Azure Synapse Analytics siempre devolverá cero (0).|  
|**is_trivial_plan**|**bit**|Plan es un plan trivial (salida en la fase 0 del optimizador de consultas). <br/>**Nota:** Azure Synapse Analytics siempre devolverá cero (0).|  
|**is_parallel_plan**|**bit**|El plan es paralelo. <br/>**Nota:** Azure Synapse Analytics siempre devolverá uno (1).|  
|**is_forced_plan**|**bit**|El plan se marca como forzado cuando el usuario ejecuta el procedimiento almacenado **Sys.sp_query_store_force_plan**. Al forzar el mecanismo no se *garantiza* que se use exactamente este plan para la consulta a la que hace referencia **query_id**. Al forzar el plan, la consulta se vuelve a compilar y normalmente se produce exactamente el mismo plan o similar al plan al que hace referencia **plan_id**. Si el forzado del plan no se realiza correctamente, se incrementa **force_failure_count** y **last_force_failure_reason** se rellena con la razón del error. <br/>**Nota:** Azure Synapse Analytics siempre devolverá cero (0).|  
|**is_natively_compiled**|**bit**|El plan incluye procedimientos optimizados para memoria compilados de forma nativa. (0 = FALSE, 1 = TRUE). <br/>**Nota:** Azure Synapse Analytics siempre devolverá cero (0).|  
|**force_failure_count**|**bigint**|Número de veces que se ha producido un error al forzar este plan. Solo se puede incrementar cuando la consulta se vuelve a compilar (*no en cada ejecución*). Se restablece en 0 cada vez que se cambia **is_plan_forced** de **false** a **true**. <br/>**Nota:** Azure Synapse Analytics siempre devolverá cero (0).|  
|**last_force_failure_reason**|**int**|Motivo del error al forzar el plan.<br /><br /> 0: sin error; de lo contrario, número de error del error que ha provocado un error en la fuerza<br /><br /> 8637: ONLINE_INDEX_BUILD<br /><br /> 8683: INVALID_STARJOIN<br /><br /> 8684: TIME_OUT<br /><br /> 8689: NO_DB<br /><br /> 8690: HINT_CONFLICT<br /><br /> 8691: SETOPT_CONFLICT<br /><br /> 8694: DQ_NO_FORCING_SUPPORTED<br /><br /> 8698: NO_PLAN<br /><br /> 8712: NO_INDEX<br /><br /> 8713: VIEW_COMPILE_FAILED<br /><br /> \<other value>: GENERAL_FAILURE <br/>**Nota:** Azure Synapse Analytics siempre devolverá cero (0).|  
|**last_force_failure_reason_desc**|**nvarchar(128)**|Descripción textual de last_force_failure_reason_desc.<br /><br /> ONLINE_INDEX_BUILD: la consulta intenta modificar los datos mientras que la tabla de destino tiene un índice que se está generando en línea<br /><br /> INVALID_STARJOIN: el plan contiene una especificación StarJoin no válida<br /><br /> TIME_OUT: el optimizador superó el número de operaciones permitidas al buscar el plan especificado por el plan forzado<br /><br /> NO_DB: no existe una base de datos especificada en el plan<br /><br /> HINT_CONFLICT: no se puede compilar la consulta porque el plan entra en conflicto con una sugerencia de consulta<br /><br /> DQ_NO_FORCING_SUPPORTED: no se puede ejecutar la consulta porque el plan entra en conflicto con el uso de consultas distribuidas o operaciones de texto completo.<br /><br /> NO_PLAN: el procesador de consultas no pudo producir el plan de consulta porque no se pudo comprobar que el plan forzado no es válido para la consulta<br /><br /> NO_INDEX: el índice especificado en el plan ya no existe<br /><br /> VIEW_COMPILE_FAILED: no se pudo forzar el plan de consulta debido a un problema en una vista indizada a la que se hace referencia en el plan<br /><br /> GENERAL_FAILURE: error de fuerza general (no se trata con los motivos anteriores) <br/>**Nota:** Azure Synapse Analytics siempre devolverá *ninguno*.|  
|**count_compiles**|**bigint**|Planear estadísticas de compilación.|  
|**initial_compile_start_time**|**datetimeoffset**|Planear estadísticas de compilación.|  
|**last_compile_start_time**|**datetimeoffset**|Planear estadísticas de compilación.|  
|**last_execution_time**|**datetimeoffset**|Última hora de ejecución hace referencia a la última hora de finalización de la consulta o el plan.|  
|**avg_compile_duration**|**float**|Planear estadísticas de compilación. <br/>**Nota:** Azure Synapse Analytics siempre devolverá cero (0).|  
|**last_compile_duration**|**bigint**|Planear estadísticas de compilación. <br/>**Nota:** Azure Synapse Analytics siempre devolverá cero (0).|  
|**plan_forcing_type**|**int**|Tipo que fuerza el plan.<br /><br />0: NINGUNO<br /><br />1: MANUAL<br /><br />2: AUTO|  
|**plan_forcing_type_desc**|**nvarchar(60)**|Descripción de texto de plan_forcing_type.<br /><br />NINGUNO: no se fuerza ningún plan<br /><br />MANUAL: Plan forzado por usuario<br /><br />AUTO: Plan forzado por ajuste automático|  

## <a name="plan-forcing-limitations"></a>Limitaciones de forzar un plan
El Almacén de consultas dispone de un mecanismo para obligar al optimizador de consultas a usar un determinado plan de ejecución. Pero existen algunas limitaciones que pueden evitar la aplicación de un plan. 

En primer lugar, si el plan contiene las siguientes construcciones:
* Instrucción insert bulk.
* Referencia a una tabla externa
* Consulta distribuida u operaciones de texto completo
* Uso de consultas globales 
* Cursores dinámicos o de conjunto de claves 
* Especificación de combinación en estrella no válida 

> [!NOTE]
> Azure SQL Database y SQL Server 2019 permiten el plan de soporte técnico para los cursores de avance rápido y estáticos.

En segundo lugar, si los objetos en los que se basa el plan ya no están disponibles:
* Base de datos (si la base de datos donde se originó el plan ya no existe)
* Índice (ya no existe o está deshabilitado)

Por último, problemas con el propio plan:
* No válido para la consulta
* El optimizador de consultas ha superado el número de operaciones permitidas
* XML de plan formado incorrectamente

## <a name="permissions"></a>Permisos  
 Requiere el permiso **View Database State** .  
  
## <a name="see-also"></a>Consulte también  
 [sys.database_query_store_options &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)   
 [sys.query_context_settings &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-context-settings-transact-sql.md)   
 [sys.query_store_query &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-query-transact-sql.md)   
 [sys.query_store_query_text &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-query-text-transact-sql.md)   
 [sys.query_store_runtime_stats &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-transact-sql.md)   
 [sys.query_store_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql.md)  
 [sys.query_store_runtime_stats_interval &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md)   
 [Monitoring Performance By Using the Query Store](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)   
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Procedimientos almacenados del almacén de consultas &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)  
  
  
