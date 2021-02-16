---
description: sys.dm_db_missing_index_groups (Transact-SQL)
title: sys.dm_db_missing_index_groups (Transact-SQL)
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_db_missing_index_groups
- sys.dm_db_missing_index_groups_TSQL
- dm_db_missing_index_groups
- dm_db_missing_index_groups_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_missing_index_groups dynamic management view
- missing indexes feature [SQL Server], sys.dm_db_missing_index_groups dynamic management view
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 203e3f190a76299899098cc0cc5a185947ce0428
ms.sourcegitcommit: c6cc0b669b175ae290cf5b08952010661ebd03c3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/16/2021
ms.locfileid: "100530840"
---
# <a name="sysdm_db_missing_index_groups-transact-sql"></a>sys.dm_db_missing_index_groups (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Esta DMV devuelve información acerca de los índices que faltan en un grupo de índices específico, excepto los índices espaciales. 
  
 En [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], las vistas de administración dinámica no pueden exponer información que impactaría a la contención de la base de datos ni acerca de otras bases de datos a las que el usuario tenga acceso. Para evitar exponer esta información, se filtran todas las filas que contienen datos que no pertenecen al inquilino conectado.  
   
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**index_group_handle**|**int**|Identifica un grupo de índices que faltan.|  
|**index_handle**|**int**|Identifica un índice que falta que pertenece al grupo especificado por **index_group_handle**.<br /><br /> Un grupo de índices solo contiene un índice.|  
  
## <a name="remarks"></a>Observaciones  
 La información devuelta por **sys.dm_db_missing_index_groups** se actualiza cuando se optimiza una consulta mediante el optimizador de consultas y no se guarda. La información sobre índices que faltan solo se conserva hasta que se reinicia [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Los administradores de bases de datos deben realizar copias de seguridad de forma periódica de la información de índices que faltan si desean conservarla después de reciclar el servidor.  
  
 Ninguna columna del conjunto de resultados de salida es una clave, pero juntas forman una clave de índice.  

  >[!NOTE]
  >El conjunto de resultados de esta DMV está limitado a 600 filas. Cada fila contiene un índice que falta. Si tiene más de 600 índices que faltan, debe tratar los índices que faltan existentes para que pueda ver los más recientes.
  
## <a name="permissions"></a>Permisos  
 Para consultar esta vista de administración dinámica, se debe conceder a los usuarios el permiso VIEW SERVER STATE o cualquier permiso que implique el permiso VIEW SERVER STATE.  
  
## <a name="see-also"></a>Consulte también  
 [sys.dm_db_missing_index_columns &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-columns-transact-sql.md)   
 [sys.dm_db_missing_index_details &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-details-transact-sql.md)   
 [sys.dm_db_missing_index_group_stats &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-group-stats-transact-sql.md)  
 [sys.dm_db_missing_index_group_stats_query &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-group-stats-query-transact-sql.md)    