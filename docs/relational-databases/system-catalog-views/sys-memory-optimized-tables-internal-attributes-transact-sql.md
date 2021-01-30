---
description: sys.memory_optimized_tables_internal_attributes (Transact-SQL)
title: sys.memory_optimized_tables_internal_attributes (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.memory_optimized_tables_internal_attributes
- sys.memory_optimized_tables_internal_attributes_TSQL
- memory_optimized_tables_internal_attributes
- memory_optimized_tables_internal_attributes_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.memory_optimized_tables_internal_attributes catalog view
ms.assetid: 78ef5807-0504-4de8-9a01-ede6c03c7ff1
author: kevin-farlee
ms.author: kfarlee
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 29d087c2501bd4296c3c60b9cefb4ca28be93fde
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191327"
---
# <a name="sysmemory_optimized_tables_internal_attributes-transact-sql"></a>sys.memory_optimized_tables_internal_attributes (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

Contiene una fila para cada tabla interna optimizada para memoria que se usa para almacenar tablas de usuario optimizadas para memoria. Cada tabla de usuario corresponde a una o varias tablas internas. Se usa solo una tabla para el almacén de datos central. Las tablas internas adicionales se usan para admitir características como almacenamiento temporal, almacenamiento de índice de almacén de columnas y almacenamiento no consecutivo (LOB) para tablas optimizadas para memoria.
 
| Nombre de la columna  | Tipo de datos  | Descripción |
| :------ |:----------| :-----|
|object_id  |**int**|       Identificador de la tabla de usuario. Las tablas internas optimizadas para memoria que existen para admitir una tabla de usuario (como almacenamiento no consecutivo o filas eliminadas, en el caso de las combinaciones de Hk/almacén de columnas) tienen el mismo valor object_id como principal. |
|xtp_object_id  |**bigint**|    Identificador de objeto de OLTP en memoria que corresponde a la tabla interna optimizada para memoria que se usa para admitir la tabla de usuario. Es un identificador único dentro de la base de datos y puede cambiar a lo largo de la duración del objeto. 
|tipo|  **int** |   Tipo de tabla interna.<br/><br/> 0 => DELETED_ROWS_TABLE <br/> 1 => USER_TABLE <br/> 2 => DICTIONARIES_TABLE<br/>3 => SEGMENTS_TABLE<br/>4 => ROW_GROUPS_INFO_TABLE<br/>5 => INTERNAL OFF-ROW DATA TABLE<br/>252 => INTERNAL_TEMPORAL_HISTORY_TABLE | 
|type_desc| **nvarchar(60)**|   Descripción del tipo<br/><br/>DELETED_ROWS_TABLE -> Tabla interna que hace seguimiento de las filas eliminadas en un índice de almacén de columnas.<br/>USER_TABLE -> Tabla que contiene los datos de usuario de manera consecutiva.<br/>DICTIONARIES_TABLE -> Diccionarios correspondiente a un índice de almacén de columnas.<br/>SEGMENTS_TABLE -> Segmentos comprimidos de un índice de almacén de columnas.<br/>ROW_GROUPS_INFO_TABLE -> Metadatos sobre los grupos de filas comprimidas de un índice de almacén de columnas.<br/>INTERNAL OFF-ROW DATA TABLE -> Tabla interna que se usa para almacenar una columna de manera no consecutiva. En este caso, minor_id refleja el valor de column_id.<br/>INTERNAL_TEMPORAL_HISTORY_TABLE -> Final activo de la tabla de historial basada en disco. Las filas insertadas en el historial se insertan primero en esta tabla interna optimizada para memoria. Existe una tarea en segundo plano que mueve de forma asincrónica las filas desde esta tabla interna a la tabla de historial basada en disco. |
|minor_id|  **int**|    El valor 0 indica un usuario o una tabla interna.<br/><br/>Un valor distinto de 0 indica el identificador de una columna almacenada no de manera consecutiva. Se combina con column_id en sys.columns.<br/><br/>Cada columna que se almacena de manera no consecutiva tiene una fila correspondiente en esta vista del sistema.|

## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-returning-all-columns-that-are-stored-off-row"></a>A. Devolver todas las columnas que se almacenan de manera no consecutiva

El siguiente script T-SQL muestra una tabla con varias columnas que no son de tipo LOB y una sola columna de tipo LOB:

```Transact-SQL
CREATE TABLE dbo.LargeTableSample
(
      Id   int IDENTITY PRIMARY KEY NONCLUSTERED,
      C1   nvarchar(4000),
      C2   nvarchar(4000),
      C3   nvarchar(4000),
      C4   nvarchar(4000),
      Misc nvarchar(max)
) WITH (MEMORY_OPTIMIZED = ON);
GO
```

La consulta siguiente muestra todas las columnas almacenadas en filas no consecutivas, además de sus tamaños. Un tamaño de -1 indica una columna de LOB. Todas las columnas de LOB se almacenan en filas no consecutivas.

```Transact-SQL
SELECT 
  QUOTENAME(SCHEMA_NAME(o.schema_id)) + N'.' + QUOTENAME(OBJECT_NAME(moa.object_id)) AS 'table', 
  c.name AS 'column', 
  c.max_length
FROM sys.memory_optimized_tables_internal_attributes moa
     JOIN sys.columns c ON moa.object_id = c.object_id AND moa.minor_id=c.column_id
     JOIN sys.objects o on moa.object_id=o.object_id 
WHERE moa.type=5;
```

### <a name="b-returning-memory-consumption-of-all-columns-that-are-stored-off-row"></a>B. Devolver el consumo de memoria de todas las columnas que se almacenan de manera no consecutiva

Para más detalles sobre el consumo de memoria de las columnas que se almacenan de manera no consecutiva, puede usar la consulta siguiente que muestra el consumo de memoria de todas las tablas internas y los índices que se usan para almacenar las columnas no consecutivas:

```Transact-SQL
SELECT
  QUOTENAME(SCHEMA_NAME(o.schema_id)) + N'.' + QUOTENAME(OBJECT_NAME(moa.object_id)) AS 'table',
  c.name AS 'column',
  c.max_length,
  mc.memory_consumer_desc,
  mc.index_id,
  mc.allocated_bytes,
  mc.used_bytes
FROM sys.memory_optimized_tables_internal_attributes moa
   JOIN sys.columns c ON moa.object_id = c.object_id AND moa.minor_id=c.column_id
   JOIN sys.dm_db_xtp_memory_consumers mc ON moa.xtp_object_id=mc.xtp_object_id
   JOIN sys.objects o on moa.object_id=o.object_id 
WHERE moa.type=5;
```

### <a name="c-returning-memory-consumption-of-columnstore-indexes-on-memory-optimized-tables"></a>C. Devolver el consumo de memoria de los índices de almacén de columnas en las tablas optimizadas para memoria

Use la siguiente consulta para mostrar el consumo de memoria de los índices de almacén de columnas en las tablas optimizadas para memoria:

```Transact-SQL
SELECT
  QUOTENAME(SCHEMA_NAME(o.schema_id)) + N'.' + QUOTENAME(OBJECT_NAME(moa.object_id)) AS 'table',
  i.name AS 'columnstore index',
  SUM(mc.allocated_bytes) / 1024 as [allocated_kb],
  SUM(mc.used_bytes) / 1024 as [used_kb]
FROM sys.memory_optimized_tables_internal_attributes moa
   JOIN sys.indexes i ON moa.object_id = i.object_id AND i.type in (5,6)
   JOIN sys.dm_db_xtp_memory_consumers mc ON moa.xtp_object_id=mc.xtp_object_id
   JOIN sys.objects o on moa.object_id=o.object_id
WHERE moa.type IN (0, 2, 3, 4)
GROUP BY o.schema_id, moa.object_id, i.name;
```

Use la siguiente consulta para desglosar el consumo de memoria en las estructuras internas usadas para los índices de almacén de columnas en las tablas optimizadas para memoria:

```Transact-SQL
SELECT
  QUOTENAME(SCHEMA_NAME(o.schema_id)) + N'.' + QUOTENAME(OBJECT_NAME(moa.object_id)) AS 'table',
  i.name AS 'columnstore index',
  moa.type_desc AS 'internal table',
  mc.index_id AS 'index',
  mc.memory_consumer_desc,
  mc.allocated_bytes / 1024 as [allocated_kb],
  mc.used_bytes / 1024 as [used_kb]
FROM sys.memory_optimized_tables_internal_attributes moa
   JOIN sys.indexes i ON moa.object_id = i.object_id AND i.type in (5,6)
   JOIN sys.dm_db_xtp_memory_consumers mc ON moa.xtp_object_id=mc.xtp_object_id
   JOIN sys.objects o on moa.object_id=o.object_id
WHERE moa.type IN (0, 2, 3, 4)
```


