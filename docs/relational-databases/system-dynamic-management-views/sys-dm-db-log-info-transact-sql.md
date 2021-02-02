---
description: sys.dm_db_log_info (Transact-SQL)
title: sys.dm_db_log_info (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/24/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
f1_keywords:
- sys.dm_db_log_info
- sys.dm_db_log_info_TSQL
- dm_db_log_info
- dm_db_log_info_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_log_info dynamic management view
ms.assetid: f6b40060-c17d-472f-b0a3-3b350275d487
author: savjani
ms.author: pariks
manager: ajayj
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 171cada53538f8e5b615e6096a169b0976b57532
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236044"
---
# <a name="sysdm_db_log_info-transact-sql"></a>sys.dm_db_log_info (Transact-SQL)
[!INCLUDE[tsql-appliesto-2016sp2-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-2016sp2-asdb-xxxx-xxx-md.md)]

Devuelve información del [archivo de registro virtual (VLF)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) del registro de transacciones. Nota: todos los archivos de registro de transacciones se combinan en la salida de la tabla. Cada fila de la salida representa un VLF en el registro de transacciones y proporciona información relevante para ese VLF en el registro.

## <a name="syntax"></a>Sintaxis  
  
```  
sys.dm_db_log_info ( database_id )  
``` 

## <a name="arguments"></a>Argumentos  
 *database_id* | NULL | PREDETERMINADA  
 Es el ID. de la base de datos. *database_id* es de **tipo int**. Las entradas válidas son el número de identificación de una base de datos, NULL o DEFAULT. El valor predeterminado es NULL. NULL y DEFAULT son valores equivalentes en el contexto de la base de datos actual.
 
 Especifique NULL para devolver información de VLF de la base de datos actual.

 Se puede especificar la función integrada [DB_ID](../../t-sql/functions/db-id-transact-sql.md). Al usar `DB_ID` sin especificar un nombre de base de datos, el nivel de compatibilidad de la base de datos actual debe ser 90 o superior.  

## <a name="table-returned"></a>Tabla devuelta  

|Nombre de columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|database_id|**int**|Id. de la base de datos.|
|file_id|**smallint**|Identificador de archivo del registro de transacciones.|  
|vlf_begin_offset|**bigint** |Ubicación de desplazamiento del [archivo de registro virtual (VLF)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) desde el principio del archivo de registro de transacciones.|
|vlf_size_mb |**float** |tamaño del [archivo de registro virtual (VLF)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) en MB, redondeado a 2 posiciones decimales.|     
|vlf_sequence_number|**bigint** |número de secuencia del [archivo de registro virtual (VLF)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) en el orden creado. Se usa para identificar de forma única VLF en el archivo de registro.|
|vlf_active|**bit** |Indica si el [archivo de registro virtual (VLF)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) está en uso o no. <br />0-VLF no está en uso.<br />1-VLF está activo.|
|vlf_status|**int** |Estado del [archivo de registro virtual (VLF)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch). Los valores posibles son: <br />0-VLF está inactivo <br />1-VLF se inicializa pero no se usa <br /> 2-VLF está activo.|
|vlf_parity|**tinyint** |Paridad del [archivo de registro virtual (VLF)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch). Se usa internamente para determinar el final del registro dentro de un VLF.|
|vlf_first_lsn|**nvarchar (48)** |[Número de secuencia de registro (LSN)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch) de la primera entrada de registro del [archivo de registro virtual (VLF)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch).|
|vlf_create_lsn|**nvarchar (48)** |[Número de secuencia de registro (LSN)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch) de la entrada de registro que creó el [archivo de registro virtual (VLF)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch).|
|vlf_encryptor_thumbprint|**varbinary(20)**| **Se aplica a:** [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]. <br><br> Muestra la huella digital del sistema de cifrado de VLF si el VLF se cifra mediante [cifrado de datos transparente](../../relational-databases/security/encryption/transparent-data-encryption.md), de lo contrario, es NULL. |

## <a name="remarks"></a>Notas
La `sys.dm_db_log_info` función de administración dinámica sustituye a la `DBCC LOGINFO` instrucción.    
 
## <a name="permissions"></a>Permisos  
Requiere el `VIEW DATABASE STATE` permiso en la base de datos.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-determing-databases-in-a-sql-server-instance-with-high-number-of-vlfs"></a>A. Desplazo de bases de datos en una instancia de SQL Server con un número elevado de VLF
La consulta siguiente determina las bases de datos con más de 100 VLF en los archivos de registro, lo que puede afectar al inicio, la restauración y el tiempo de recuperación de la base de datos.

```sql
SELECT [name], COUNT(l.database_id) AS 'vlf_count' 
FROM sys.databases s
CROSS APPLY sys.dm_db_log_info(s.database_id) l
GROUP BY [name]
HAVING COUNT(l.database_id) > 100
```

### <a name="b-determing-the-position-of-the-last-vlf-in-transaction-log-before-shrinking-the-log-file"></a>B. Determinar la posición de la última en el `VLF` registro de transacciones antes de reducir el archivo de registro

La siguiente consulta se puede utilizar para determinar la posición de la última VLF activa antes de ejecutar SHRINKFILE en el registro de transacciones para determinar si se puede reducir el registro de transacciones.

```sql
USE AdventureWorks2016
GO

;WITH cte_vlf AS (
SELECT ROW_NUMBER() OVER(ORDER BY vlf_begin_offset) AS vlfid, DB_NAME(database_id) AS [Database Name], vlf_sequence_number, vlf_active, vlf_begin_offset, vlf_size_mb
    FROM sys.dm_db_log_info(DEFAULT)),
cte_vlf_cnt AS (SELECT [Database Name], COUNT(vlf_sequence_number) AS vlf_count,
    (SELECT COUNT(vlf_sequence_number) FROM cte_vlf WHERE vlf_active = 0) AS vlf_count_inactive,
    (SELECT COUNT(vlf_sequence_number) FROM cte_vlf WHERE vlf_active = 1) AS vlf_count_active,
    (SELECT MIN(vlfid) FROM cte_vlf WHERE vlf_active = 1) AS ordinal_min_vlf_active,
    (SELECT MIN(vlf_sequence_number) FROM cte_vlf WHERE vlf_active = 1) AS min_vlf_active,
    (SELECT MAX(vlfid) FROM cte_vlf WHERE vlf_active = 1) AS ordinal_max_vlf_active,
    (SELECT MAX(vlf_sequence_number) FROM cte_vlf WHERE vlf_active = 1) AS max_vlf_active
    FROM cte_vlf
    GROUP BY [Database Name])
SELECT [Database Name], vlf_count, min_vlf_active, ordinal_min_vlf_active, max_vlf_active, ordinal_max_vlf_active,
((ordinal_min_vlf_active-1)*100.00/vlf_count) AS free_log_pct_before_active_log,
((ordinal_max_vlf_active-(ordinal_min_vlf_active-1))*100.00/vlf_count) AS active_log_pct,
((vlf_count-ordinal_max_vlf_active)*100.00/vlf_count) AS free_log_pct_after_active_log
FROM cte_vlf_cnt
GO
```

## <a name="see-also"></a>Consulte también  
[Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
[Vistas de administración dinámica relacionadas con bases de datos &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)   
[sys.dm_db_log_space_usage &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-log-space-usage-transact-sql.md)   
[sys.dm_db_log_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-log-stats-transact-sql.md)

