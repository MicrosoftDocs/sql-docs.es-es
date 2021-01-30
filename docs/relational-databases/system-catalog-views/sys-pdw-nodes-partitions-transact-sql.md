---
description: sys.pdw_nodes_partitions (Transact-SQL)
title: sys.pdw_nodes_partitions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: b4216752-4813-4b2c-b259-7d8ffc6cc190
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: c4c6e3ce34f1fbd73bf046b9fdeb178d82878f6f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207837"
---
# <a name="syspdw_nodes_partitions-transact-sql"></a>sys.pdw_nodes_partitions (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Contiene una fila para cada partición de todas las tablas y la mayoría de los tipos de índices de una [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] base de datos. Todas las tablas e índices contienen al menos una partición, tanto si se crean particiones explícitamente como si no.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|partition_id|**bigint**|Id. de la partición. Es único en una base de datos.|  
|object_id|**int**|Id. del objeto al que pertenece esta partición. Todas las tablas o vistas se componen al menos de una partición.|  
|index_id|**int**|Id. del índice dentro del objeto al que pertenece esta partición.|  
|partition_number|**int**|Número de partición basado en uno en el índice o el montón propietario. Para [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] , el valor de esta columna es 1.|  
|hobt_id|**bigint**|IDENTIFICADOR de la montículo o árbol B de datos (HoBT) que contiene las filas de esta partición.|  
|rows|**bigint**|Número aproximado de filas de esta partición. |  
|data_compression|**int**|Indica el estado de compresión para cada partición:<br /><br /> 0 = NONE<br /><br /> 1 = ROW<br /><br /> 2 = PAGE<br /><br /> 3 = COLUMNSTORE|  
|data_compression_desc|**nvarchar(60)**|Indica el estado de compresión para cada partición. Los valores posibles son NONE, ROW y PAGE.|  
|pdw_node_id|**int**|Identificador único de un [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] nodo.|  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso `CONTROL SERVER`.  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  

### <a name="example-a-display-rows-in-each-partition-within-each-distribution"></a>Ejemplo A: mostrar filas en cada partición de cada distribución 

**Se aplica a:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]
 
Para mostrar el número de filas de cada partición dentro de cada distribución, use [DBCC PDW_SHOWPARTITIONSTATS (PDW de SQL Server)](../../t-sql/database-console-commands/dbcc-pdw-showpartitionstats-transact-sql.md) .

### <a name="example-b-uses-system-views-to-view-rows-in-each-partition-of-each-distribution-of-a-table"></a>Ejemplo B: usa las vistas del sistema para ver las filas de cada una de las distribuciones de una tabla

**Se aplica a:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].
 
Esta consulta devuelve el número de filas de cada partición de cada distribución de la tabla `myTable` .  
 
```sql  
SELECT o.name, pnp.index_id, pnp.partition_id, pnp.rows,   
    pnp.data_compression_desc, pnp.pdw_node_id  
FROM sys.pdw_nodes_partitions AS pnp  
JOIN sys.pdw_nodes_tables AS NTables  
    ON pnp.object_id = NTables.object_id  
AND pnp.pdw_node_id = NTables.pdw_node_id  
JOIN sys.pdw_table_mappings AS TMap  
    ON NTables.name = TMap.physical_name 
    AND substring(TMap.physical_name,40, 10) = pnp.distribution_id 
JOIN sys.objects AS o  
    ON TMap.object_id = o.object_id  
WHERE o.name = 'myTable'  
ORDER BY o.name, pnp.index_id, pnp.partition_id;  
```    
  
## <a name="see-also"></a>Consulte también  
 [Vistas de catálogo de Azure Synapse Analytics y Almacenamiento de datos paralelos](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)  
  
  

