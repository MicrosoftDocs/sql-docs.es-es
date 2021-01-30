---
description: sys.dm_db_log_stats (Transact-SQL)
title: sys.dm_db_log_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/17/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_db_log_stats_TSQL
- sys.dm_db_log_stats
- sys.dm_db_log_stats_TSQL
- dm_db_log_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_log_stats dynamic management function
ms.assetid: ''
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 88ac57a5ba41afacafe8565225ed9ce7bca55991
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191187"
---
# <a name="sysdm_db_log_stats-transact-sql"></a>sys.dm_db_log_stats (Transact-SQL)   
[!INCLUDE[tsql-appliesto-2016sp2-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-2016sp2-asdb-xxxx-xxx-md.md)]

Devuelve atributos de nivel de resumen e información sobre los archivos de registro de transacciones de bases de datos. Use esta información para supervisar y diagnosticar el estado del registro de transacciones.   
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
 sys.dm_db_log_stats ( database_id )
```  
  
## <a name="arguments"></a>Argumentos  

*database_id* | NULL | **Valor predeterminado**

Es el ID. de la base de datos. `database_id` es `int`. Las entradas válidas son el número de identificación de una base de datos, `NULL` o `DEFAULT` . De manera predeterminada, es `NULL`. `NULL` y `DEFAULT` son valores equivalentes en el contexto de la base de datos actual.  
Se puede especificar la función integrada [DB_ID](../../t-sql/functions/db-id-transact-sql.md). Al usar `DB_ID` sin especificar un nombre de base de datos, el nivel de compatibilidad de la base de datos actual debe ser 90 o superior.

  
## <a name="tables-returned"></a>Tablas devueltas  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|database_id    |**int**    |Identificador de base de datos |  
|recovery_model |**nvarchar(60)**   |   Modelo de recuperación de la base de datos. Los valores posibles son: <br /> SIMPLE<br /> BULK_LOGGED <br /> FULL |  
|log_min_lsn    |**nvarchar(24)**   |   Número de [secuencia de registro (LSN)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch) de inicio actual en el registro de transacciones.|  
|log_end_lsn    |**nvarchar(24)**   |   [número de secuencia de registro (LSN)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch) de la última entrada de registro en el registro de transacciones.|  
|current_vlf_sequence_number    |**bigint** |   Número de secuencia actual del [archivo de registro virtual (VLF)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) en el momento de la ejecución.|  
|current_vlf_size_mb    |**float**  |   Tamaño actual del [archivo de registro virtual (VLF)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) en MB.|   
|total_vlf_count    |**bigint** |   Número total de [archivos de registro virtuales (VLF)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) en el registro de transacciones. |  
|total_log_size_mb  |**float**  |   Tamaño total del registro de transacciones en MB. |  
|active_vlf_count   |**bigint** |   Número total de [archivos de registro virtuales activos (VLF)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) en el registro de transacciones.|  
|active_log_size_mb |**float**  |   Tamaño total del registro de transacciones activo en MB.|  
|log_truncation_holdup_reason   |**nvarchar(60)**   |   Motivo del parada de truncamiento del registro. El valor es el mismo que el  `log_reuse_wait_desc` de la columna de `sys.databases` .  (Para obtener explicaciones más detalladas de estos valores, consulte [el registro de transacciones](../../relational-databases/logs/the-transaction-log-sql-server.md)). <br />Los valores posibles son: <br />NOTHING<br />CHECKPOINT<br />LOG_BACKUP<br />ACTIVE_BACKUP_OR_RESTORE<br />ACTIVE_TRANSACTION<br />DATABASE_MIRRORING<br />REPLICACIÓN<br />DATABASE_SNAPSHOT_CREATION<br />LOG_SCAN<br />AVAILABILITY_REPLICA<br />OLDEST_PAGE<br />XTP_CHECKPOINT<br />OTROS TRANSITORIOS |  
|log_backup_time    |**datetime**   |   Hora de la última copia de seguridad del registro de transacciones.|   
|log_backup_lsn |**nvarchar(24)**   |   Último número de secuencia de registro de la copia de seguridad del registro de transacciones [(LSN)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch).|   
|log_since_last_log_backup_mb   |**float**  |   Tamaño del registro en MB desde el último número de secuencia de registro de la copia de seguridad del registro de transacciones [(LSN)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch).|  
|log_checkpoint_lsn |**nvarchar(24)**   |   Último [número de secuencia de registro (LSN) del](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch)punto de comprobación.|  
|log_since_last_checkpoint_mb   |**float**  |   Tamaño del registro en MB desde el último [número de secuencia de registro (LSN)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch)del punto de comprobación.|  
|log_recovery_lsn   |**nvarchar(24)**   |   [Número de secuencia de registro (LSN)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch) de recuperación de la base de datos. Si se `log_recovery_lsn` produce antes del LSN del punto de comprobación, `log_recovery_lsn` es el LSN de la transacción activa más antiguo; de lo contrario, `log_recovery_lsn` es el LSN del punto de comprobación.|  
|log_recovery_size_mb   |**float**  |   Tamaño del registro en MB desde el [número de secuencia de registro (LSN)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch)de recuperación de registro.|  
|recovery_vlf_count |**bigint** |   Número total de [archivos de registro virtuales (VLF)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) que se van a recuperar, si se ha producido una conmutación por error o un reinicio del servidor. |  


## <a name="remarks"></a>Observaciones
Cuando `sys.dm_db_log_stats` se ejecuta en una base de datos que participa en un grupo de disponibilidad como una réplica secundaria, solo se devolverá un subconjunto de los campos descritos anteriormente.  Actualmente, solo `database_id` `recovery_model` `log_backup_time` se devolverá, y cuando se ejecute en una base de datos secundaria.   

## <a name="permissions"></a>Permisos  
Requiere el `VIEW DATABASE STATE` permiso en la base de datos.   
  
## <a name="examples"></a>Ejemplos  

### <a name="a-determining-databases-in-a-ssnoversion-instance-with-high-number-of-vlfs"></a>A. Determinar las bases de datos en una [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instancia con un gran número de VLF   
La siguiente consulta devuelve las bases de datos con más de 100 VLF en los archivos de registro. Un gran número de VLF puede afectar al inicio, la restauración y el tiempo de recuperación de la base de datos.

```sql  
SELECT name AS 'Database Name', total_vlf_count AS 'VLF count' 
FROM sys.databases AS s
CROSS APPLY sys.dm_db_log_stats(s.database_id) 
WHERE total_vlf_count  > 100;
```   

### <a name="b-determining-databases-in-a-ssnoversion-instance-with-transaction-log-backups-older-than-4-hours"></a>B. Determinar las bases de datos de una [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instancia con copias de seguridad de registros de transacciones anteriores a 4 horas   
La consulta siguiente determina la hora de la última copia de seguridad de registros para las bases de datos de la instancia.

```sql  
SELECT name AS 'Database Name', log_backup_time AS 'last log backup time' 
FROM sys.databases AS s
CROSS APPLY sys.dm_db_log_stats(s.database_id); 
```

## <a name="see-also"></a>Consulte también  
[Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
[Vistas de administración dinámica relacionadas con bases de datos &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)   
[sys.dm_db_log_space_usage &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-log-space-usage-transact-sql.md)   
[sys.dm_db_log_info &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-log-info-transact-sql.md)    
  
