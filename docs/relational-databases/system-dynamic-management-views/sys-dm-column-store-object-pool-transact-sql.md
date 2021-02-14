---
description: sys.dm_column_store_object_pool (Transact-SQL)
title: sys.dm_column_store_object_pool (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: a8a58ca7-0a7d-4786-bfd9-e8894bd345dd
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: bad20aaf35fd0ddc53627648166e560cedc06ae3
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2021
ms.locfileid: "100345333"
---
# <a name="sysdm_column_store_object_pool-transact-sql"></a>sys.dm_column_store_object_pool (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

 Devuelve recuentos de diferentes tipos de uso del bloque de memoria de objetos para los objetos de índice de almacén de columnas.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**database_id**|int|Identificador de la base de datos. Esto es único dentro de una instancia de SQL Server base de datos o un servidor de Azure SQL Database. |  
|**object_id**|int|Id. del objeto. El objeto es uno de los object_types. | 
|**id_de_índice**|int|Identificador del índice de almacén de columnas.|  
|**partition_number**|bigint|Número de partición en base 1 en el índice o montón. Cada tabla o vista tiene al menos una partición.| 
|**column_id**|int|Identificador de la columna de almacén de columnas. Es NULL para DELETE_BITMAP.| 
|**row_group_id**|int|IDENTIFICADOR de filas.|
|**object_type**|SMALLINT|1 = COLUMN_SEGMENT<br /><br /> 2 = COLUMN_SEGMENT_PRIMARY_DICTIONARY<br /><br /> 3 = COLUMN_SEGMENT_SECONDARY_DICTIONARY<br /><br /> 4 = COLUMN_SEGMENT_BULKINSERT_DICTIONARY<br /><br /> 5 = COLUMN_SEGMENT_DELETE_BITMAP|  
|**object_type_desc**|nvarchar(60)|COLUMN_SEGMENT: segmento de columna. `object_id` es el identificador del segmento. Un segmento almacena todos los valores de una columna dentro de un filas. Por ejemplo, si una tabla tiene 10 columnas, hay 10 segmentos de columna por filas. <br /><br /> COLUMN_SEGMENT_PRIMARY_DICTIONARY: un diccionario global que contiene información de búsqueda para todos los segmentos de columna de la tabla.<br /><br /> COLUMN_SEGMENT_SECONDARY_DICTIONARY: Diccionario local asociado a una columna.<br /><br /> COLUMN_SEGMENT_BULKINSERT_DICTIONARY: otra representación del diccionario global. Esto proporciona una búsqueda inversa del valor que se va a dictionary_id. Se usa para crear segmentos comprimidos como parte de la carga masiva o de la tupla.<br /><br /> COLUMN_SEGMENT_DELETE_BITMAP: un mapa de bits que realiza un seguimiento de las eliminaciones de segmentos. Hay un mapa de bits de eliminación por partición.|  
|**access_count**|int|Número de accesos de lectura o escritura a este objeto.|  
|**memory_used_in_bytes**|bigint|Memoria usada por este objeto en el grupo de objetos.|  
|**object_load_time**|datetime|Hora del reloj en el que se ha introducido object_id en el grupo de objetos.|  
  
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
 [Índices de almacén de columnas: información general](../../relational-databases/indexes/columnstore-indexes-overview.md) 
  
