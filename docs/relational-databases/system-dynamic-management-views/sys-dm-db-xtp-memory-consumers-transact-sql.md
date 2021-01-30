---
description: sys.dm_db_xtp_memory_consumers (Transact-SQL)
title: sys.dm_db_xtp_memory_consumers (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_db_xtp_memory_consumers
- dm_db_xtp_memory_consumers
- dm_db_xtp_memory_consumers_TSQL
- sys.dm_db_xtp_memory_consumers_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_xtp_memory_consumers dynamic management view
ms.assetid: f7ab2eaf-e627-464d-91fe-0e170b3f37bc
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 28968656d3efbb5a9cc658168944b75e93350d44
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193622"
---
# <a name="sysdm_db_xtp_memory_consumers-transact-sql"></a>sys.dm_db_xtp_memory_consumers (Transact-SQL)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  Notifica a los consumidores de memoria de nivel de base de datos en el motor de base de datos de [!INCLUDE[hek_2](../../includes/hek-2-md.md)]. La vista devuelve una fila para cada consumidor de memoria que el motor de base de datos utiliza. Use esta DMV para ver cómo se distribuye la memoria entre diferentes objetos internos.  
  
 Para obtener más información, vea [OLTP en memoria &#40;optimización en memoria&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|memory_consumer_id|**bigint**|Identificador (interno) del consumidor de memoria.|  
|memory_consumer_type|**int**|Tipo de consumidor de memoria:<br /><br /> 0=Aggregation. (Agrega el uso de memoria de dos o varios consumidores. No se debe mostrar).<br /><br /> 2=VARHEAP (Hace un seguimiento del consumo de memoria para un montón de longitud variable).<br /><br /> 3=HASH (Hace un seguimiento del consumo de memoria para un índice).<br /><br /> 5=DB page pool (Hace un seguimiento del consumo de memoria para un grupo de páginas de base de datos para las operaciones en tiempo de ejecución. Por ejemplo, variables de tabla y algunos recorridos serializables. Solo hay un consumidor de memoria de este tipo en cada base de datos).|  
|memory_consumer_type_desc|**nvarchar (64)**|Tipo de consumidor de memoria: VARHEAP, HASH o PGPOOL.<br /><br /> 0: (no se debe mostrar).<br /><br /> 2: VARHEAP<br /><br /> 3 - HASH<br /><br /> 5: PGPOOL|  
|memory_consumer_desc|**nvarchar (64)**|Descripción de la instancia del consumidor de memoria:<br /><br /> VARHEAP <br />Montón de base de datos. Se usa para asignar datos de usuario para una base de datos (filas).<br />Montón del sistema de base de datos. Se usa para asignar datos de base de datos que se incluirán en volcados de memoria y no incluir datos de usuario.<br />Montón de índice de intervalo. Montón privado que el índice de intervalo usa para asignar páginas BW.<br /><br /> HASH: no Description, ya que el object_id indica la tabla y el index_id el propio índice de hash.<br /><br /> PGPOOL: para la base de datos, solo hay un grupo de páginas de 64 KB de base de datos.|  
|object_id|**bigint**|Identificador del objeto al que se atribuye la memoria asignada. Un valor negativo para los objetos del sistema.|  
|xtp_object_id|**bigint**|IDENTIFICADOR de objeto de la tabla optimizada para memoria.|  
|index_id|**int**|El identificador de índice del consumidor (si existe). NULL para las tablas base.|  
|allocated_bytes|**bigint**|Número de bytes reservados para el consumidor.|  
|used_bytes|**bigint**|Bytes utilizados por el consumidor. Solo se aplica a varheap.|  
|allocation_count|**int**|Número de asignaciones.|  
|partition_count|**int**|Solo para uso interno.|  
|sizeclass_count|**int**|Solo para uso interno.|  
|min_sizeclass|**int**|Solo para uso interno.|  
|max_sizeclass|**int**|Solo para uso interno.|  
|memory_consumer_address|**varbinary**|Dirección interna del consumidor. Solo para uso interno.|  
|xtp_object_id|**bigint**|IDENTIFICADOR de objeto de OLTP en memoria que corresponde a la tabla optimizada para memoria.|  
  
## <a name="remarks"></a>Observaciones  
 En la salida, los asignadores en los niveles de base de datos hacen referencia a las tablas de usuario, los índices y las tablas del sistema. VARHEAP con object_id = NULL hace referencia en la memoria asignada a tablas con columnas de longitud variable.  
  
## <a name="permissions"></a>Permisos  
 Se devuelven todas las filas si tiene el permiso VIEW DATABASE STATE en la base de datos actual. De lo contrario, se devuelve un conjunto de filas vacío.  
  
 Si no tiene permiso DATABASE STATE, todas las columnas se devolverán para las filas en las tablas en las que tenga el permiso SELECT.  
  
 Las tablas del sistema solo se devuelven para los usuarios con el permiso VIEW DATABASE STATE.  
  
## <a name="general-remarks"></a>Notas generales  
 Cuando una tabla optimizada para memoria tiene un índice de almacén de columnas, el sistema usa algunas tablas internas, que consumen memoria, para realizar el seguimiento de los datos del índice de almacén de columnas. Para obtener más información sobre estas tablas internas y consultas de ejemplo que muestran su consumo de memoria [, vea sys.memory_optimized_tables_internal_attributes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-memory-optimized-tables-internal-attributes-transact-sql.md).
 
  
## <a name="examples"></a>Ejemplos  
  
```  
-- memory consumers (database level)  
SELECT OBJECT_NAME(object_id), *   
FROM sys.dm_db_xtp_memory_consumers;  
```  
  
## <a name="user-scenario"></a>Escenario de usuario  
  
```  
-- memory consumers (database level)  
  
select  convert(char(10), object_name(object_id)) as Name,   
convert(char(10),memory_consumer_type_desc ) as memory_consumer_type_desc, object_id,index_id, allocated_bytes,  used_bytes   
from sys.dm_db_xtp_memory_consumers  
```  
  
 Este es el resultado con un subconjunto de columnas. Los asignadores en los niveles de base de datos hacen referencia a las tablas de usuario, los índices y las tablas del sistema. VARHEAP con object_id = NULL (última fila) hace referencia a la memoria asignada a las filas de datos de las tablas (en este ejemplo, es t1). Bytes asignados, cuando se convierten a MB, es 1340 MB.  
  
```  
Name       memory_consumer_type_desc object_id   index_id    allocated_bytes      used_bytes  
---------- ------------------------- ----------- ----------- -------------------- --------------------  
t3         HASH                      629577281   2           8388608              8388608  
t2         HASH                      597577167   2           8388608              8388608  
t1         HASH                      565577053   2           1048576              1048576  
NULL       HASH                      -6          1           2048                 2048  
NULL       VARHEAP                   -6          NULL        0                    0  
NULL       HASH                      -5          3           8192                 8192  
NULL       HASH                      -5          2           8192                 8192  
NULL       HASH                      -5          1           8192                 8192  
NULL       HASH                      -4          1           2048                 2048  
NULL       VARHEAP                   -4          NULL        0                    0  
NULL       HASH                      -3          1           2048                 2048  
NULL       HASH                      -2          2           8192                 8192  
NULL       HASH                      -2          1           8192                 8192  
NULL       VARHEAP                   -2          NULL        196608               26496  
NULL       HASH                      0           1           2048                 2048  
NULL       PGPOOL                    0           NULL        0                    0  
NULL       VARHEAP                   NULL        NULL        1405943808           1231220560  
  
(17 row(s) affected)  
```  
  
 La memoria total asignada y utilizada desde esta DMV es la misma que el nivel de objeto de [sys.dm_db_xtp_table_memory_stats &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-table-memory-stats-transact-sql.md).  
  
```  
select  sum(allocated_bytes)/(1024*1024) as total_allocated_MB,   
        sum(used_bytes)/(1024*1024) as total_used_MB  
from sys.dm_db_xtp_memory_consumers  
  
total_allocated_MB   total_used_MB  
-------------------- --------------------  
1358                 1191  
```  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de administración dinámica de tablas optimizadas para memoria &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
  
