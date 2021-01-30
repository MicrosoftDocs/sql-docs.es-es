---
description: sys.dm_io_virtual_file_stats (Transact-SQL)
title: sys.dm_io_virtual_file_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_io_virtual_file_stats
- sys.dm_io_virtual_file_stats_TSQL
- sys.dm_io_virtual_file_stats
- dm_io_virtual_file_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_io_virtual_file_stats dynamic management function
ms.assetid: fa3e321f-6fe5-45ff-b397-02a0dd3d6b7d
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d967f94947338c00dd5d7fa1150f6f6c414d0bb6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185019"
---
# <a name="sysdm_io_virtual_file_stats-transact-sql"></a>sys.dm_io_virtual_file_stats (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Devuelve las estadísticas de E/S de los archivos de registro y datos. Esta función de administración dinámica reemplaza a la función [fn_virtualfilestats](../../relational-databases/system-functions/sys-fn-virtualfilestats-transact-sql.md) .  
  
> [!NOTE]  
>  Para llamarlo desde [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , utilice el nombre **Sys.dm_pdw_nodes_io_virtual_file_stats**. 

## <a name="syntax"></a>Sintaxis  
  
```  
-- Syntax for SQL Server and Azure SQL Database

sys.dm_io_virtual_file_stats (   
    { database_id | NULL },  
    { file_id | NULL }  
)  
```  

```  
-- Syntax for Azure Synapse Analytics

sys.dm_pdw_nodes_io_virtual_file_stats
```
  
## <a name="arguments"></a>Argumentos  


 *database_id* | ACEPTA

 **SE APLICA A:** SQL Server (a partir de 2008), Azure SQL Database

 Identificador de la base de datos. *database_id* es de tipo int y no tiene ningún valor predeterminado. Las entradas válidas son el número de identificación de una base de datos o NULL. Cuando se especifica NULL, se devuelven todas las bases de datos de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Se puede especificar la función integrada [DB_ID](../../t-sql/functions/db-id-transact-sql.md).  
  
*file_id* | ACEPTA

**SE APLICA A:** SQL Server (a partir de 2008), Azure SQL Database
 
Id. del archivo. *file_id* es de tipo int y no tiene ningún valor predeterminado. Las entradas válidas son el número de identificación de un archivo o NULL. Cuando se especifica NULL, se devuelven todos los archivos de la base de datos.  
  
 Se puede especificar la función integrada [FILE_IDEX](../../t-sql/functions/file-idex-transact-sql.md) y hace referencia a un archivo de la base de datos actual.  
  
## <a name="table-returned"></a>Tabla devuelta  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**database_name**|**sysname**|No **se aplica a:**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .<br /><br /> nombre de base de datos.</br></br>En el caso de Azure Synapse Analytics, es el nombre de la base de datos almacenada en el nodo que se identifica mediante pdw_node_id. Cada nodo tiene una base de datos Tempdb con 13 archivos. Cada nodo también tiene una base de datos por distribución, y cada base de datos de distribución tiene 5 archivos. Por ejemplo, si cada nodo contiene 4 distribuciones, los resultados muestran 20 archivos de base de datos de distribución por pdw_node_id. 
|**database_id**|**smallint**|Identificador de la base de datos.|  
|**file_id**|**smallint**|IDENTIFICADOR del archivo.|  
|**sample_ms**|**bigint**|Número de milisegundos transcurridos desde que se inició el equipo. Esta columna se puede utilizar para comparar diferentes resultados de esta función.</br></br>El tipo de datos es **int** para [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] a través de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]|  
|**num_of_reads**|**bigint**|Número de operaciones de lectura realizadas en el archivo.|  
|**num_of_bytes_read**|**bigint**|Número total de bytes leídos en el archivo.|  
|**io_stall_read_ms**|**bigint**|Tiempo total, en milisegundos, que los usuarios han esperado para que se realicen las lecturas en el archivo.|  
|**num_of_writes**|**bigint**|Número de operaciones de escritura realizadas en este archivo.|  
|**num_of_bytes_written**|**bigint**|Número total de bytes escritos en el archivo.|  
|**io_stall_write_ms**|**bigint**|Tiempo total, en milisegundos, que los usuarios han esperado para que se completen las escrituras en el archivo.|  
|**io_stall**|**bigint**|Tiempo total, en milisegundos, que los usuarios han esperado para que se completen las operaciones de E/S en el archivo.|  
|**size_on_disk_bytes**|**bigint**|Número de bytes utilizados en el disco para este archivo. En el caso de archivos dispersos, este número es el número real de bytes en el disco utilizados para las instantáneas de base de datos.|  
|**file_handle**|**varbinary**|Identificador de archivo de Windows para este archivo.|  
|**io_stall_queued_read_ms**|**bigint**|No **se aplica a:**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] hasta [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] .<br /><br /> Latencia total de E/S introducida por la gobernanza de recursos de E/S para las lecturas. No admite valores NULL. Para obtener más información, vea [sys.dm_resource_governor_resource_pools &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-transact-sql.md).|  
|**io_stall_queued_write_ms**|**bigint**|No **se aplica a:**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] hasta [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] .<br /><br />  Latencia total de E/S introducida por la gobernanza de recursos de E/S para las escrituras. No admite valores NULL.|
|**pdw_node_id**|**int**|**Se aplica a:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].</br></br>Identificador del nodo de la distribución.
 
## <a name="remarks"></a>Observaciones
Los contadores se inicializan en vacío cada vez que se inicia el servicio SQL Server (MSSQLSERVER).
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso VIEW SERVER STATE. Para obtener más información, vea [funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md).  
  
## <a name="examples"></a>Ejemplos  

### <a name="a-return-statistics-for-a-log-file"></a>A. Devolver estadísticas para un archivo de registro

**Se aplica a:** SQL Server (a partir de 2008), Azure SQL Database

 El ejemplo siguiente devuelve todas las estadísticas para el archivo de registro en la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
SELECT * FROM sys.dm_io_virtual_file_stats(DB_ID(N'AdventureWorks2012'), 2);  
GO  
```  
  
### <a name="b-return-statistics-for-file-in-tempdb"></a>B. Devolver estadísticas del archivo en tempdb

**Se aplica a:** Azure Synapse Analytics

```sql
SELECT * FROM sys.dm_pdw_nodes_io_virtual_file_stats 
WHERE database_name = 'tempdb' AND file_id = 2;

```

## <a name="see-also"></a>Consulte también  
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [I O funciones y vistas de administración dinámica relacionadas &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/i-o-related-dynamic-management-views-and-functions-transact-sql.md)   
 [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [sys.master_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)  
  
  

