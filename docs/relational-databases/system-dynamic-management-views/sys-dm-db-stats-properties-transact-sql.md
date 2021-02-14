---
description: sys.dm_db_stats_properties (Transact-SQL)
title: sys.dm_db_stats_properties (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_db_stats_properties_TSQL
- sys.dm_db_stats_properties
- dm_db_stats_properties_TSQL
- dm_db_stats_properties
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_stats_properties
ms.assetid: 8a54889d-e263-4881-9fcb-b1db410a9453
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 03f712bb35f0a24ba563022ce45b01a5f1bf132c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343051"
---
# <a name="sysdm_db_stats_properties-transact-sql"></a>sys.dm_db_stats_properties (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve propiedades de estadísticas para el objeto de base de datos especificado (tabla o vista indizada) en la base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] actual. En el caso de las tablas con particiones, vea el [Sys.dm_db_incremental_stats_properties](../../relational-databases/system-dynamic-management-views/sys-dm-db-incremental-stats-properties-transact-sql.md)similar. 
 
## <a name="syntax"></a>Sintaxis  
  
```  
sys.dm_db_stats_properties (object_id, stats_id)  
```  
  
## <a name="arguments"></a>Argumentos  
 *object_id*  
 Es el identificador del objeto en la base de datos actual para el que se han solicitado propiedades de una de sus estadísticas. *object_id* es de **tipo int**.  
  
 *stats_id*  
 Es el identificador de estadísticas para el *object_id* especificado. El identificador de estadísticas se puede obtener desde la vista de administración dinámica [sys.stats](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md) . *stats_id* es **int**.  
  
## <a name="table-returned"></a>Tabla devuelta  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|object_id|**int**|Identificador del objeto (tabla o vista indizada) para el que se devuelven las propiedades del objeto de estadísticas.|  
|stats_id|**int**|Identificador del objeto de estadísticas. Es único dentro de la vista indizada o la tabla. Para obtener más información, vea [sys.stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md).|  
|last_updated|**datetime2**|Fecha y hora de la última actualización del objeto de estadísticas. Para más información, vea la sección [Comentarios](#Remarks) en esta página.|  
|rows|**bigint**|Número total de filas que tenía la tabla o la vista indizada la última vez que se actualizaron las estadísticas. Si las estadísticas se filtran o corresponden a un índice filtrado, el número de filas puede ser inferior al número de filas de la tabla.|  
|rows_sampled|**bigint**|Número total de filas muestreadas para cálculos de estadísticas.|  
|steps|**int**|Número de pasos del histograma. Para obtener más información, vea [DBCC SHOW_STATISTICS &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md).|  
|unfiltered_rows|**bigint**|Número total de filas de la tabla antes de aplicar la expresión de filtro (para estadísticas filtradas). Si las estadísticas no están filtradas, unfiltered_rows es igual al valor devuelto en la columna rows.|  
|modification_counter|**bigint**|Número total de modificaciones para la columna de estadísticas iniciales (la columna en la que se ha generado el histograma) desde la última vez que se actualizaron las estadísticas.<br /><br /> Tablas con optimización para memoria: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] el inicio y en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] esta columna contiene el número total de modificaciones de la tabla desde la última vez que se actualizaron las estadísticas o se reinició la base de datos.|  
|persisted_sample_percent|**float**|Porcentaje de ejemplo persistente empleado en las actualizaciones de estadísticas en las que no se especifica explícitamente un porcentaje de muestreo. Si el valor es cero, significa que no hay establecido ningún porcentaje de ejemplo persistente para esta estadística.<br /><br /> **Se aplica a:** [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 CU4|  
  
## <a name="remarks"></a><a name="Remarks"></a> Comentarios  
 **Sys.dm_db_stats_properties** devuelve un conjunto de filas vacío en cualquiera de las siguientes condiciones:  
  
-   **object_id** o **stats_id** es NULL.    
-   El objeto especificado no se encuentra o no corresponde a una vista indizada o a una tabla.    
-   El identificador de estadísticas especificado no se corresponde con las estadísticas existentes para el identificador de objeto especificado.    
-   El usuario actual no tiene permisos para ver el objeto de estadísticas.  
  
 Este comportamiento permite el uso seguro de **Sys.dm_db_stats_properties** cuando se aplican cruzados a filas en vistas como **Sys. Objects** y **Sys. stats**.  
 
La fecha de actualización de estadísticas se almacena en el [objeto BLOB de estadísticas](../../relational-databases/statistics/statistics.md#DefinitionQOStatistics) junto con el [histograma](../../relational-databases/statistics/statistics.md#histogram) y el [vector de densidad](../../relational-databases/statistics/statistics.md#density), pero no en los metadatos. Cuando no se lee ningún dato para generar datos de estadísticas, el BLOB de estadísticas no se crea, la fecha no está disponible y el *LAST_UPDATED* columna es NULL. Esto sucede en las estadísticas filtradas, en las que el predicado no devuelve ninguna fila, o en las tablas nuevas vacías.
  
## <a name="permissions"></a>Permisos  
 Necesita que el usuario tenga permisos de selección en columnas de estadísticas o posea la tabla, o que el usuario sea miembro del rol fijo de servidor `sysadmin`, del rol fijo de base de datos `db_owner` o del rol fijo de base de datos `db_ddladmin`.  
  
## <a name="examples"></a>Ejemplos  

### <a name="a-simple-example"></a>A. Ejemplo sencillo
En el ejemplo siguiente se devuelven las estadísticas de la `Person.Person` tabla en la base de datos AdventureWorks.

```sql
SELECT * FROM sys.dm_db_stats_properties (object_id('Person.Person'), 1);
``` 
  
### <a name="b-returning-all-statistics-properties-for-a-table"></a>B. Devolver propiedades de todas las estadísticas de una tabla  
 En el ejemplo siguiente se devuelven propiedades de todas las estadísticas que existen para la tabla TEST.  
  
```sql  
SELECT sp.stats_id, name, filter_definition, last_updated, rows, rows_sampled, steps, unfiltered_rows, modification_counter   
FROM sys.stats AS stat   
CROSS APPLY sys.dm_db_stats_properties(stat.object_id, stat.stats_id) AS sp  
WHERE stat.object_id = object_id('TEST');  
```  
  
### <a name="c-returning-statistics-properties-for-frequently-modified-objects"></a>C. Devolver propiedades de estadísticas para objetos modificados frecuentemente  
 En el ejemplo siguiente se devuelven todas las tablas, vistas indizadas y estadísticas de la base de datos actual para las que la columna inicial se modificó más de 1000 veces desde la última actualización de estadísticas.  
  
```sql  
SELECT obj.name, obj.object_id, stat.name, stat.stats_id, last_updated, modification_counter  
FROM sys.objects AS obj   
INNER JOIN sys.stats AS stat ON stat.object_id = obj.object_id  
CROSS APPLY sys.dm_db_stats_properties(stat.object_id, stat.stats_id) AS sp  
WHERE modification_counter > 1000;  
```  
  
## <a name="see-also"></a>Consulte también  
 [DBCC SHOW_STATISTICS &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)   
 [sys.stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md)   
 [Funciones y vistas de administración dinámica relacionadas con objetos &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/object-related-dynamic-management-views-and-functions-transact-sql.md)   
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)  
 [sys.dm_db_incremental_stats_properties (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-incremental-stats-properties-transact-sql.md)  
 [sys.dm_db_stats_histogram (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-histogram-transact-sql.md) 
  

