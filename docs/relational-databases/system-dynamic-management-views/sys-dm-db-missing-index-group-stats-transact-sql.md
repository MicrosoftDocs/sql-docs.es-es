---
description: sys.dm_db_missing_index_group_stats (Transact-SQL)
title: sys.dm_db_missing_index_group_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_db_missing_index_group_stats_TSQL
- sys.dm_db_missing_index_group_stats
- dm_db_missing_index_group_stats_TSQL
- dm_db_missing_index_group_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_missing_index_group_stats dynamic management view
- missing indexes feature [SQL Server], sys.dm_db_missing_index_group_stats dynamic management view
ms.assetid: c2886986-9e07-44ea-a350-feeac05ee4f4
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1cb6680d80df5b54c5ea4d74b07256442ccf3e28
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209422"
---
# <a name="sysdm_db_missing_index_group_stats-transact-sql"></a>sys.dm_db_missing_index_group_stats (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve información de resumen sobre los grupos de índices que faltan, excluidos los índices espaciales.  
  
 En [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], las vistas de administración dinámica no pueden exponer información que impactaría a la contención de la base de datos ni acerca de otras bases de datos a las que el usuario tenga acceso. Para evitar exponer esta información, se filtran todas las filas que contienen datos que no pertenecen al inquilino conectado.  
    
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**group_handle**|**int**|Identifica un grupo de índices que faltan. Este identificador es único en todo el servidor.<br /><br /> Las otras columnas proporcionan información sobre todas las consultas para las que se considera que falta el índice del grupo.<br /><br /> Un grupo de índices solo contiene un índice.|  
|**unique_compiles**|**bigint**|Número de compilaciones y recompilaciones que se beneficiarían de este grupo de índices que faltan. Las compilaciones y recompilaciones de muchas consultas distintas puede contribuir a este valor de columna.|  
|**user_seeks**|**bigint**|Número de búsquedas iniciadas por consultas de usuario para las que se podría haber utilizado el índice recomendado del grupo.|  
|**user_scans**|**bigint**|Número de recorridos iniciados por consultas de usuario para los que se podría haber utilizado el índice recomendado del grupo.|  
|**last_user_seek**|**datetime**|Fecha y hora de la última búsqueda iniciada por consultas de usuario para la que se podría haber utilizado el índice recomendado del grupo.|  
|**last_user_scan**|**datetime**|Fecha y hora del último recorrido iniciado por consultas de usuario para el que se podría haber utilizado el índice recomendado del grupo.|  
|**avg_total_user_cost**|**float**|Costo medio de las consultas de usuario que podría reducirse mediante el índice del grupo.|  
|**avg_user_impact**|**float**|Beneficio porcentual medio que podrían obtener las consultas de usuario si se implementara este grupo de índices que faltan. El valor significa que el costo de las consultas se reduciría este porcentaje como promedio si se implementara este grupo de índices que faltan.|  
|**system_seeks**|**bigint**|Número de búsquedas iniciadas por consultas del sistema, como consultas de estadísticas automáticas, para las que se podría haber utilizado el índice recomendado del grupo. Para obtener más información, consulte [auto stats (clase de eventos](../../relational-databases/event-classes/auto-stats-event-class.md)).|  
|**system_scans**|**bigint**|Número de recorridos iniciados por consultas del sistema para los que se podría haber utilizado el índice recomendado del grupo.|  
|**last_system_seek**|**datetime**|Fecha y hora de la última búsqueda en el sistema iniciada por consultas del sistema para la que se podría haber utilizado el índice recomendado del grupo.|  
|**last_system_scan**|**datetime**|Fecha y hora del último recorrido en el sistema iniciado por consultas del sistema para el que se podría haber utilizado el índice recomendado del grupo.|  
|**avg_total_system_cost**|**float**|Costo medio de las consultas del sistema que podría reducirse mediante el índice del grupo.|  
|**avg_system_impact**|**float**|Beneficio porcentual medio que podrían obtener las consultas del sistema si se implementara este grupo de índices que faltan. El valor significa que el costo de las consultas se reduciría este porcentaje como promedio si se implementara este grupo de índices que faltan.|  
  
## <a name="remarks"></a>Observaciones  
 La información devuelta por **sys.dm_db_missing_index_group_stats** se actualiza en cada ejecución de la consulta, no en cada compilación o recompilación de la consulta. Las estadísticas de uso no se guardan; solo se conservan hasta que se reinicia [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Los administradores de bases de datos deben realizar periódicamente una copia de seguridad de la información de los índices que faltan si desean conservar las estadísticas de uso después del reciclaje del servidor.  

  >[!NOTE]
  >El conjunto de resultados de esta DMV está limitado a 600 filas. Cada fila contiene un índice que falta. Si tiene más de 600 índices que faltan, debe tratar los índices que faltan existentes para que pueda ver los más recientes.
  
## <a name="permissions"></a>Permisos  
 Para consultar esta vista de administración dinámica, se debe conceder a los usuarios el permiso VIEW SERVER STATE o cualquier permiso que implique el permiso VIEW SERVER STATE.  
  
## <a name="examples"></a>Ejemplos  
 En los siguientes ejemplos se ilustra la forma de usar la vista de administración dinámica **sys.dm_db_missing_index_group_stats**.  
  
### <a name="a-find-the-10-missing-indexes-with-the-highest-anticipated-improvement-for-user-queries"></a>A. Buscar los 10 índices que faltan para los que se prevé mayor aumento de rendimiento en las consultas de usuario  
 La siguiente consulta determina cuáles de los 10 índices que faltan producirían el mayor aumento acumulado previsto, en orden descendente, para consultas de usuario.  
  
```  
SELECT TOP 10 *  
FROM sys.dm_db_missing_index_group_stats  
ORDER BY avg_total_user_cost * avg_user_impact * (user_seeks + user_scans)DESC;  
```  
  
### <a name="b-find-the-individual-missing-indexes-and-their-column-details-for-a-particular-missing-index-group"></a>B. Buscar los índices que faltan individuales y sus detalles de columna para un grupo específico de índices que faltan  
 La siguiente consulta determina cuáles de los índices que faltan forman un grupo específico de índices que faltan y muestra sus detalles de columna. Para este ejemplo, el identificador del grupo de índices que faltan es 24.  
  
```  
SELECT migs.group_handle, mid.*  
FROM sys.dm_db_missing_index_group_stats AS migs  
INNER JOIN sys.dm_db_missing_index_groups AS mig  
    ON (migs.group_handle = mig.index_group_handle)  
INNER JOIN sys.dm_db_missing_index_details AS mid  
    ON (mig.index_handle = mid.index_handle)  
WHERE migs.group_handle = 24;  
```  
  
 Esta consulta proporciona el nombre de la base de datos, el esquema y la tabla en que falta un índice. También proporciona los nombres de las columnas que deben usarse para la clave de índice. Al escribir la instrucción CREATE INDEX DDL para implementar índices que faltan, enumere primero las columnas de igualdad y después las columnas de desigualdad en la \<*table_name*> cláusula on de la instrucción CREATE index. Las columnas incluidas deben mostrarse en la cláusula INCLUDE de la instrucción CREATE INDEX. Para determinar un orden efectivo para las columnas de igualdad, ordénelas en función de su selectividad, mostrando primero las columnas más selectivas (en la parte izquierda de la lista de columnas).  
  
## <a name="see-also"></a>Consulte también  
 [sys.dm_db_missing_index_columns &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-columns-transact-sql.md)   
 [sys.dm_db_missing_index_details &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-details-transact-sql.md)   
 [sys.dm_db_missing_index_groups &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-groups-transact-sql.md)   
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)  
  
  
