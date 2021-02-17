---
description: sys.dm_db_index_usage_stats (Transact-SQL)
title: sys.dm_db_index_usage_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_db_index_usage_stats_TSQL
- sys.dm_db_index_usage_stats
- sys.dm_db_index_usage_stats_TSQL
- dm_db_index_usage_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_index_usage_stats dynamic management view
ms.assetid: d06a001f-0f72-4679-bc2f-66fff7958b86
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3d77f847862bb1a551508f26ac353f6f58809b02
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2021
ms.locfileid: "100343116"
---
# <a name="sysdm_db_index_usage_stats-transact-sql"></a>sys.dm_db_index_usage_stats (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve recuentos de diferentes tipos de operaciones de índice y la hora en que se realizó por última vez cada uno de los tipos de operación.  
  
 En [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], las vistas de administración dinámica no pueden exponer información que impactaría a la contención de la base de datos ni acerca de otras bases de datos a las que el usuario tenga acceso. Para evitar exponer esta información, se filtran todas las filas que contienen datos que no pertenecen al inquilino conectado.  
  
> [!NOTE]  
>  **Sys.dm_db_index_usage_stats** no devuelve información acerca de los índices optimizados para memoria o los índices espaciales. Para obtener información sobre el uso de índices optimizados para memoria, vea [sys.dm_db_xtp_index_stats &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-index-stats-transact-sql.md).  
  
> [!NOTE]  
>  Para llamar a esta vista desde [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , use **Sys.dm_pdw_nodes_db_index_usage_stats**.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**database_id**|**smallint**|Id. de la base de datos en la que se define la tabla o vista.|  
|**object_id**|**int**|Id. de la tabla o vista en la que se define el índice.|  
|**id_de_índice**|**int**|Id. del índice.|  
|**user_seeks**|**bigint**|Número de consultas de búsqueda realizadas por el usuario.|  
|**user_scans**|**bigint**|Número de recorridos por consultas de usuario que no usaron el predicado "Buscar".|  
|**user_lookups**|**bigint**|Número de búsquedas de marcadores realizadas por consultas de usuario.|  
|**user_updates**|**bigint**|Número de consultas de actualización realizadas por el usuario. Esto incluye las operaciones de inserción, eliminación y actualización que representan el número de operaciones realizadas no las filas reales afectadas. Por ejemplo, si elimina 1000 filas en una instrucción, el recuento se incrementa en 1.|  
|**last_user_seek**|**datetime**|Hora en que el usuario realizó la última búsqueda.|  
|**last_user_scan**|**datetime**|Hora en que el usuario realizó el último recorrido.|  
|**last_user_lookup**|**datetime**|Hora de la última búsqueda del usuario.|  
|**last_user_update**|**datetime**|Hora en que el usuario realizó la última actualización.|  
|**system_seeks**|**bigint**|Número de consultas de búsqueda realizadas por el sistema.|  
|**system_scans**|**bigint**|Número de consultas de recorrido realizadas por el sistema.|  
|**system_lookups**|**bigint**|Número de búsquedas realizadas por consultas del sistema.|  
|**system_updates**|**bigint**|Número de consultas de actualización realizadas por el sistema.|  
|**last_system_seek**|**datetime**|Hora en que el sistema realizó la última búsqueda.|  
|**last_system_scan**|**datetime**|Hora en que el sistema realizó el último recorrido.|  
|**last_system_lookup**|**datetime**|Hora en que el sistema realizó la última búsqueda.|  
|**last_system_update**|**datetime**|Hora en que el sistema realizó la última actualización.|  
|pdw_node_id|**int**|**Se aplica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificador del nodo en el que se encuentra esta distribución.|  
  
## <a name="remarks"></a>Observaciones  
 Cada búsqueda, recorrido o actualización en el índice especificado realizado por una ejecución de la consulta se cuenta como un uso de ese índice e incrementa el contador correspondiente en esa vista. Se ofrece información tanto de las operaciones causadas por las consultas emitidas por el usuario, como de las consultas generadas internamente, tales como los recorridos realizados para recopilar estadísticas.  
  
 El contador de **user_updates** indica el nivel de mantenimiento en el índice causado por operaciones de inserción, actualización o eliminación en la vista o tabla subyacente. Puede utilizar esta vista para determinar los índices que las aplicaciones apenas utilizan. También puede utilizar esta vista para determinar los índices que producen una sobrecarga de mantenimiento. Puede considerar la opción de quitar los índices que produzcan esta sobrecarga, pero que no se utilicen para consultas o se usen con poca frecuencia.  
  
 Los contadores se inicializan en un valor vacío cada vez que se inicia el servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (MSSQLSERVER). Además, cada vez que una base de datos se separa o se apaga (por ejemplo, porque se establece AUTO_CLOSE en ON), se quitan todas las filas asociadas con la base de datos.  
  
 Cuando se utiliza un índice, se agrega una fila a **sys.dm_db_index_usage_stats** si no existe todavía una fila para el índice. Cuando se agrega la fila, sus contadores se establecen inicialmente en cero.  
  
 Durante la actualización a [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] , [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] o [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] , se quitan las entradas de sys.dm_db_index_usage_stats. A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] , las entradas se conservan tal y como estaban antes de [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] .  
  
## <a name="permissions"></a>Permisos  
En [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiere el `VIEW SERVER STATE` permiso.   
En SQL Database objetivos de servicio Basic, S0 y S1, y para las bases de datos de grupos elásticos, se requiere la cuenta de [Administrador del servidor](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) o la cuenta de [Administrador de Azure Active Directory](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . En el resto de los objetivos del servicio SQL Database, `VIEW DATABASE STATE` se requiere el permiso en la base de datos.  
  
## <a name="see-also"></a>Consulte también  

 [Funciones y vistas de administración dinámica relacionadas con índices &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/index-related-dynamic-management-views-and-functions-transact-sql.md)   
 [sys.dm_db_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)   
 [sys.dm_db_index_operational_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-operational-stats-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [Supervisión y optimización del rendimiento](../../relational-databases/performance/monitor-and-tune-for-performance.md)  
  
  


