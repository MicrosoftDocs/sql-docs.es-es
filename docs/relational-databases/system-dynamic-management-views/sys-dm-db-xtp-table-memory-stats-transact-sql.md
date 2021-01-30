---
description: sys.dm_db_xtp_table_memory_stats (Transact-SQL)
title: sys.dm_db_xtp_table_memory_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_db_xtp_table_memory_stats_TSQL
- dm_db_xtp_table_memory_stats
- sys.dm_db_xtp_table_memory_stats
- dm_db_xtp_table_memory_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_xtp_table_memory_stats
- dm_db_xtp_table_memory_stats
ms.assetid: ad0efc06-3d9c-4861-9dfa-a7a87822d0c8
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6521cc342b94ba120271215b98970c05eed7fd62
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160112"
---
# <a name="sysdm_db_xtp_table_memory_stats-transact-sql"></a>sys.dm_db_xtp_table_memory_stats (Transact-SQL)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  Devuelve estadísticas del uso de memoria para cada tabla de [!INCLUDE[hek_2](../../includes/hek-2-md.md)] (usuario y sistema) de la base de datos actual. Las tablas del sistema tienen identificadores de objetos negativos y se emplean para almacenar información en tiempo de ejecución para el motor de [!INCLUDE[hek_2](../../includes/hek-2-md.md)]. A diferencia de los objetos de usuario, las tablas del sistema son internas y solo existen en memoria; por tanto, no son visible mediante vistas de catálogo. Las tablas del sistema se usan para almacenar información como metadatos para todos los archivos de datos y delta del almacenamiento, solicitudes de combinación, marcas de agua de archivos delta para filtrar filas, tablas quitadas e información pertinente para la recuperación y las copias de seguridad. Puesto que el motor de [!INCLUDE[hek_2](../../includes/hek-2-md.md)] puede tener hasta 8.192 pares de archivos de datos y delta, para las bases de datos en memoria grandes, la memoria que ocupan las tablas del sistema puede ser de algunos megabytes.  
  
 Para obtener más información, vea [OLTP en memoria &#40;optimización en memoria&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|object_id|**int**|Identificador de objeto de la tabla. Es NULL para las tablas del sistema de OLTP en memoria.|  
|memory_allocated_for_table_kb|**bigint**|La memoria asignada para esta tabla.|  
|memory_used_by_table_kb|**bigint**|Memoria utilizada por la tabla, incluidas las versiones de fila.|  
|memory_allocated_for_indexes_kb|**bigint**|La memoria asignada para los índices en esta tabla.|  
|memory_used_by_indexes_kb|**bigint**|La memoria usada por los índices en esta tabla.|  
  
## <a name="permissions"></a>Permisos  
 Se devuelven todas las filas si tiene el permiso VIEW DATABASE STATE en la base de datos actual. De lo contrario, se devuelve un conjunto de filas vacío.  
  
 Si no tiene permiso DATABASE STATE, todas las columnas se devolverán para las filas en las tablas en las que tenga el permiso SELECT.  
  
 Las tablas del sistema solo se devuelven para los usuarios con el permiso VIEW DATABASE STATE.  
  
## <a name="examples"></a>Ejemplos  
 Puede consultar la DMV siguiente para obtener la memoria asignada para las tablas y los índices en la base de datos:  
  
```  
-- finding memory for objects  
SELECT OBJECT_NAME(object_id), *   
FROM sys.dm_db_xtp_table_memory_stats;  
```  
  
 Para buscar memoria para todos los objetos dentro de la base de datos:  
  
```  
SELECT SUM( memory_allocated_for_indexes_kb + memory_allocated_for_table_kb) AS  
 memoryallocated_objects_in_kb   
FROM sys.dm_db_xtp_table_memory_stats;  
```  
  
## <a name="user-scenario"></a>Escenario de usuario  
 Cree primero las siguientes tablas en una base de datos denominada HkDb1.  
  
```  
-- set max server memory to 4 GB  
EXEC sp_configure 'max server memory (MB)', 4048  
go  
  
RECONFIGURE  
go  
  
-- create a resource pool for database with memory-optimized objects  
CREATE RESOURCE POOL PoolHkDb1 WITH (MAX_MEMORY_PERCENT = 50);  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
go  
  
--bind the pool to the database  
EXEC sp_xtp_bind_db_resource_pool 'HkDb1', 'PoolHkdb1'  
go  
  
-- take database offline/online to associate the pool  
use master  
go  
  
alter database HkDb1 set offline  
go  
alter database HkDb1 set online  
go  
  
USE HkDb1  
go  
  
CREATE TABLE dbo.t1 (  
       c1 int NOT NULL,  
       c2 char(40) NOT NULL,  
       c3 char(8000) NOT NULL,  
  
       CONSTRAINT [pk_t1_c1] PRIMARY KEY NONCLUSTERED HASH (c1) WITH (BUCKET_COUNT = 100000)  
) WITH (MEMORY_OPTIMIZED = ON, DURABILITY = SCHEMA_AND_DATA)  
go  
  
CREATE TABLE dbo.t2 (  
       c1 int NOT NULL,  
       c2 char(40) NOT NULL,  
       c3 char(8000) NOT NULL,  
  
       CONSTRAINT [pk_t2_c1] PRIMARY KEY NONCLUSTERED HASH (c1) WITH (BUCKET_COUNT = 100000)  
) WITH (MEMORY_OPTIMIZED = ON, DURABILITY = SCHEMA_AND_DATA)  
go  
  
CREATE TABLE dbo.t3 (  
       c1 int NOT NULL,  
       c2 char(40) NOT NULL,  
       c3 char(8000) NOT NULL,  
  
       CONSTRAINT [pk_t3_c1] PRIMARY KEY NONCLUSTERED HASH (c1) WITH (BUCKET_COUNT = 1000000)  
) WITH (MEMORY_OPTIMIZED = ON, DURABILITY = SCHEMA_AND_DATA)  
go  
  
-- load 150K rows  
declare @i int = 0  
while (@i <= 150000)  
begin  
       insert t1 values (@i, 'a', replicate ('b', 8000))  
       set @i += 1;  
end  
go  
```  
  
 Cuando los datos se cargan en una tabla, puede ver las tablas definidas por el usuario y cuánto almacenamiento utilizan. Por ejemplo, cada fila de una tabla puede ser de aproximadamente 8070 bytes (el tamaño de asignación es de 8 K (8192 bytes)). Puede ver los índices por tabla y cuánto almacenamiento usa el índice. Por ejemplo, 1 MB son 100K entradas redondeadas a la siguiente potencia de 2 (2**17) = 131072 de 8 bytes cada uno. Una tabla puede no tener un índice, en cuyo caso se muestra la asignación de memoria para el índice. Otras filas pueden representar las tablas del sistema  
  
```  
select convert(char(10), object_name(object_id)) as Name,*   
from sys.dm_db_xtp_table_memory_stats  
```  
  
 Estos son los resultados, en dos partes:  
  
```  
Name       object_id   memory_allocated_for_table_kb memory_used_by_table_kb  
---------- ----------- ----------------------------- -----------------------  
t3         629577281   0                             0  
t1         565577053   1372928                       1202351  
t2         597577167   0                             0  
NULL       -6          0                             0  
NULL       -5          0                             0  
NULL       -4          0                             0  
NULL       -3          0                             0  
NULL       -2          192                           25  
  
memory_allocated_for_indexes_kb memory_used_by_indexes_kb  
------------------------------- -------------------------  
8192                            8192  
1024                            1024  
8192                            8192  
2                               2  
24                              24  
2                               2  
2                               2  
16                              16  
```  
  
 El resultado de,  
  
```  
select  sum(allocated_bytes)/(1024*1024) as total_allocated_MB,   
       sum(used_bytes)/(1024*1024) as total_used_MB  
from sys.dm_db_xtp_memory_consumers  
```  
  
 es,  
  
```  
total_allocated_MB   total_used_MB  
-------------------- --------------------  
1357                 1191  
```  
  
 A continuación, veamos los resultados del grupo de recursos de servidor. Tenga en cuenta que la memoria que se usa del grupo es 1356 MB  
  
```  
select pool_id,convert(char(10), name) as Name, min_memory_percent, max_memory_percent,   
   max_memory_kb/1024 as max_memory_mb  
from sys.dm_resource_governor_resource_pools  
  
select used_memory_kb/1024 as used_memory_mb ,target_memory_kb/1024 as target_memory_mb  
from sys.dm_resource_governor_resource_pools  
```  
  
 aquí está el resultado:  
  
```  
pool_id     Name       min_memory_percent max_memory_percent max_memory_mb  
----------- ---------- ------------------ ------------------ --------------------  
1           internal   0                  100                3845  
2           default    0                  100                3845  
259         PoolHkDb1  0                  100                3845  
  
used_memory_mb       target_memory_mb  
-------------------- --------------------  
125                  3845  
32                   3845  
1356                 3845  
```  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de administración dinámica de tablas optimizadas para memoria &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
  
