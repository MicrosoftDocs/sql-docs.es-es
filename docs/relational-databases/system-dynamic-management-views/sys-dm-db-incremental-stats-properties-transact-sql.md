---
description: sys.dm_db_incremental_stats_properties (Transact-SQL)
title: sys.dm_db_incremental_stats_properties (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/18/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_db_incremental_stats_properties
- sys.dm_db_incremental_stats_properties_TSQL
- dm_db_incremental_stats_properties
- dm_db_incremental_stats_properties_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_incremental_stats_properties
ms.assetid: aa0db893-34d1-419c-b008-224852e71307
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 578d55bc9f80a4586993b2b5a2011934f66c0cee
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180608"
---
# <a name="sysdm_db_incremental_stats_properties-transact-sql"></a>sys.dm_db_incremental_stats_properties (Transact-SQL)
[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

  Devuelve propiedades de estadísticas incrementales para el objeto de base de datos especificado (tabla) en la base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . El uso de `sys.dm_db_incremental_stats_properties` (que contiene un número de partición) es similar a `sys.dm_db_stats_properties` , que se usa para estadísticas no incrementales. 
  
  Esta función se presentó en el [!INCLUDE[ssSQL14_md](../../includes/sssql14-md.md)] Service Pack 2 y en el [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] Service Pack 1.
  
## <a name="syntax"></a>Sintaxis  
  
```  
sys.dm_db_incremental_stats_properties (object_id, stats_id)  
```  
  
## <a name="arguments"></a>Argumentos  
 *object_id*  
 Es el identificador del objeto en la base de datos actual para el que se solicitaron las propiedades de una de sus estadísticas incrementales. *object_id* es de **tipo int**.  
  
 *stats_id*  
 Es el identificador de estadísticas para el *object_id* especificado. El identificador de estadísticas se puede obtener desde la vista de administración dinámica [sys.stats](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md) . *stats_id* es **int**.  
  
## <a name="table-returned"></a>Tabla devuelta  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|object_id|**int**|Identificador del objeto (tabla) para el que se devuelven las propiedades del objeto de estadísticas.|  
|stats_id|**int**|Identificador del objeto de estadísticas. Es único en la tabla. Para obtener más información, vea [sys.stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md).|
|partition_number|**int**|El número de la partición que contiene la parte de la tabla.|  
|last_updated|**datetime2**|Fecha y hora de la última actualización del objeto de estadísticas. Para más información, vea la sección [Comentarios](#Remarks) en esta página.|  
|rows|**bigint**|El número total de filas que tenía la tabla la última vez que se actualizaron las estadísticas. Si las estadísticas se filtran o corresponden a un índice filtrado, el número de filas puede ser inferior al número de filas de la tabla.|  
|rows_sampled|**bigint**|Número total de filas muestreadas para cálculos de estadísticas.|  
|steps|**int**|Número de pasos del histograma. Para obtener más información, vea [DBCC SHOW_STATISTICS &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md).|  
|unfiltered_rows|**bigint**|Número total de filas de la tabla antes de aplicar la expresión de filtro (para estadísticas filtradas). Si las estadísticas no están filtradas, unfiltered_rows es igual al valor devuelto en la columna rows.|  
|modification_counter|**bigint**|Número total de modificaciones para la columna de estadísticas iniciales (la columna en la que se ha generado el histograma) desde la última vez que se actualizaron las estadísticas.<br /><br /> Esta columna no contiene información sobre tablas optimizadas para memoria.|  
  
## <a name="remarks"></a><a name="Remarks"></a> Comentarios  
 `sys.dm_db_incremental_stats_properties` devuelve un conjunto de filas vacío si se da alguna de las condiciones siguientes:  
  
-   `object_id` o `stats_id` es NULL.   
-   El objeto especificado no se encuentra o no corresponde a una tabla con estadísticas incrementales.  
-   El identificador de estadísticas especificado no se corresponde con las estadísticas existentes para el identificador de objeto especificado.  
-   El usuario actual no tiene permisos para ver el objeto de estadísticas.
 
 Este comportamiento permite el uso seguro de `sys.dm_db_incremental_stats_properties` cuando se aplica de manera cruzada a filas en vistas como `sys.objects` y `sys.stats`. Este método puede devolver propiedades de las estadísticas que corresponden a cada partición. Para ver las propiedades de las estadísticas combinadas en todas las particiones, use sys.dm_db_stats_properties en su lugar. 

La fecha de actualización de estadísticas se almacena en el [objeto BLOB de estadísticas](../../relational-databases/statistics/statistics.md#DefinitionQOStatistics) junto con el [histograma](../../relational-databases/statistics/statistics.md#histogram) y el [vector de densidad](../../relational-databases/statistics/statistics.md#density), pero no en los metadatos. Cuando no se lee ningún dato para generar datos de estadísticas, el BLOB de estadísticas no se crea, la fecha no está disponible y el *LAST_UPDATED* columna es NULL. Esto sucede en las estadísticas filtradas, en las que el predicado no devuelve ninguna fila, o en las tablas nuevas vacías.

## <a name="permissions"></a>Permisos  
 Necesita que el usuario tenga permisos de selección en columnas de estadísticas o posea la tabla, o que el usuario sea miembro del rol fijo de servidor `sysadmin`, del rol fijo de base de datos `db_owner` o del rol fijo de base de datos `db_ddladmin`.  
  
## <a name="examples"></a>Ejemplos  

### <a name="a-simple-example"></a>A. Ejemplo sencillo
El ejemplo siguiente devuelve las estadísticas de la tabla `PartitionTable` que se describe en el tema [Crear tablas e índices con particiones](../../relational-databases/partitions/create-partitioned-tables-and-indexes.md).

```sql
SELECT * FROM sys.dm_db_incremental_stats_properties (object_id('PartitionTable'), 1);
``` 

Para sugerencias adicionales de uso, consulte  [sys.dm_db_stats_properties](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-properties-transact-sql.md).
  
## <a name="see-also"></a>Consulte también  
 [DBCC SHOW_STATISTICS &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)   
 [sys.stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md)   
 [Funciones y vistas de administración dinámica relacionadas con objetos &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/object-related-dynamic-management-views-and-functions-transact-sql.md)   
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)  
 [sys.dm_db_stats_properties](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-properties-transact-sql.md)   
 [sys.dm_db_stats_histogram (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-histogram-transact-sql.md) 
