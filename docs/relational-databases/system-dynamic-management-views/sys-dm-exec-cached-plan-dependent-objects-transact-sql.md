---
description: sys.dm_exec_cached_plan_dependent_objects (Transact-SQL)
title: sys.dm_exec_cached_plan_dependent_objects (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_exec_cached_plan_dependent_objects
- dm_exec_cached_plan_dependent_objects_TSQL
- sys.dm_exec_cached_plan_dependent_objects_TSQL
- dm_exec_cached_plan_dependent_objects
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_cached_plan_dependent_objects dynamic management function
ms.assetid: 9b6cf5f7-b267-44fb-aac8-f49c9aa10cc1
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f26611fba5e00acd67a865e713807724b61bc64c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160132"
---
# <a name="sysdm_exec_cached_plan_dependent_objects-transact-sql"></a>sys.dm_exec_cached_plan_dependent_objects (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve una fila para cada plan de ejecución de [!INCLUDE[tsql](../../includes/tsql-md.md)], plan de ejecución de Common Language Runtime (CLR) y cursor asociado a un plan.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
sys.dm_exec_cached_plan_dependent_objects(plan_handle)  
```  
  
## <a name="arguments"></a>Argumentos  
*plan_handle*  
Es un token que identifica de forma única un plan de ejecución de consulta para un lote que se ha ejecutado y su plan reside en la caché del plan. *plan_handle* es **varbinary (64)**.   

El *plan_handle* se puede obtener de los siguientes objetos de administración dinámica:  
  
-   [sys.dm_exec_cached_plans &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)  
  
-   [sys.dm_exec_query_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)  
  
-   [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  

-   [sys.dm_exec_procedure_stats &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md)  

-   [sys.dm_exec_trigger_stats &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-trigger-stats-transact-sql.md)  
  
## <a name="table-returned"></a>Tabla devuelta  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**usecounts**|**int**|Número de veces que se ha usado un contexto de ejecución o un cursor.<br /><br /> La columna no acepta valores NULL.|  
|**memory_object_address**|**varbinary(8**|Dirección de memoria del contexto de ejecución o el cursor.<br /><br /> La columna no acepta valores NULL.|  
|**cacheobjtype**|**nvarchar(50)**|El tipo de objeto de caché del plan. La columna no acepta valores NULL. Los valores posibles son<br /><br /> Plan ejecutable<br /><br /> Función CLR compilada<br /><br /> Procedimiento CLR compilado<br /><br /> Cursor|  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso `VIEW SERVER STATE` en el servidor.  
  
## <a name="physical-joins"></a>Combinaciones físicas  
 ![Diagrama de relaciones](../../relational-databases/system-dynamic-management-views/media/dm-dependent-objects.gif "Diagrama de relaciones")  
  
## <a name="relationship-cardinalities"></a>Cardinalidades de relación  
  
|From|En|Por|Relación|  
|----------|--------|--------|------------------|  
|**dm_exec_cached_plan_dependent_objects**|**dm_os_memory_objects**|**memory_object_address**|Uno a uno|  
  
## <a name="see-also"></a>Consulte también  
 [Funciones y vistas de administración dinámica relacionadas con la ejecución &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [sys.syscacheobjects &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-syscacheobjects-transact-sql.md)  
  
  
