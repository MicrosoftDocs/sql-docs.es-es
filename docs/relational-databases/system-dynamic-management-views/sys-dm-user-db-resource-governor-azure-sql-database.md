---
description: sys.dm_user_db_resource_governance (Transact-SQL)
title: sys.dm_user_db_resource_governance (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/17/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.resource_governance
- sys.resource_governance_TSQL
- resource_governance
- resource_governance_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.resource_governance catalog view
ms.assetid: ''
author: joesackmsft
ms.author: josack
monikerRange: =azuresqldb-current
ms.openlocfilehash: 1044577add08fe1c9d6337f67244a185d98db8e4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065570"
---
# <a name="sysdm_user_db_resource_governance-transact-sql"></a>sys.dm_user_db_resource_governance (Transact-SQL)

[!INCLUDE[appliesto-xx-asdb-xxxx-xxx-md](../../includes/appliesto-xx-asdb-xxxx-xxx-md.md)]

Devuelve la configuración y los valores de capacidad reales que usan los mecanismos de regulación de recursos en la base de datos o el grupo elástico actuales.
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**database_id**|int|IDENTIFICADOR de la base de datos, único dentro de un servidor de Azure SQL Database.|
|**logical_database_guid**|UNIQUEIDENTIFIER|GUID lógico para la base de datos de usuario que permanece a lo largo de la vida de una base de datos de usuario.  Cambiar el nombre de la base de datos o cambiar su objetivo de nivel de servicio no cambiará este valor.|
|**physical_database_guid**|UNIQUEIDENTIFIER|GUID físico para una base de datos de usuario que permanece a lo largo de la vida de la instancia física de la base de datos de usuario. Cambiar el objetivo de nivel de servicio de base de datos hará que este valor cambie.|
|**server_name**|NVARCHAR|Nombre del servidor lógico.|
|**database_name**|NVARCHAR|Nombre de la base de datos lógica.|
|**slo_name**|NVARCHAR|Objetivo de nivel de servicio, incluida la generación de hardware.|
|**dtu_limit**|int|Límite de DTU de la base de datos (NULL para núcleo virtual).|
|**cpu_limit**|int|Núcleo virtual límite de base de datos (NULL para las bases de datos de DTU).|
|**min_cpu**|TINYINT|El valor MIN_CPU_PERCENT del grupo de recursos de la carga de trabajo del usuario. Vea [conceptos de grupos de recursos](../resource-governor/resource-governor-resource-pool.md#resource-pool-concepts).|
|**max_cpu**|TINYINT|El valor MAX_CPU_PERCENT del grupo de recursos de la carga de trabajo del usuario. Vea [conceptos de grupos de recursos](../resource-governor/resource-governor-resource-pool.md#resource-pool-concepts).|
|**cap_cpu**|TINYINT|El valor CAP_CPU_PERCENT del grupo de recursos de la carga de trabajo del usuario. Vea [conceptos de grupos de recursos](../resource-governor/resource-governor-resource-pool.md#resource-pool-concepts).|
|**min_cores**|SMALLINT|Solo para uso interno.|
|**max_dop**|SMALLINT|El valor de MAX_DOP del grupo de cargas de trabajo del usuario. Consulte [creación](../../t-sql/statements/create-workload-group-transact-sql.md)de un grupo de cargas de trabajo.|
|**min_memory**|int|El valor MIN_MEMORY_PERCENT del grupo de recursos de la carga de trabajo del usuario. Vea [conceptos de grupos de recursos](../resource-governor/resource-governor-resource-pool.md#resource-pool-concepts).|
|**max_memory**|int|El valor MAX_MEMORY_PERCENT del grupo de recursos de la carga de trabajo del usuario. Vea [conceptos de grupos de recursos](../resource-governor/resource-governor-resource-pool.md#resource-pool-concepts).|
|**max_sessions**|int|El número máximo de sesiones permitidas en el grupo de cargas de trabajo de usuario.|
|**max_memory_grant**|int|El valor de REQUEST_MAX_MEMORY_GRANT_PERCENT del grupo de cargas de trabajo del usuario. Consulte [creación](../../t-sql/statements/create-workload-group-transact-sql.md)de un grupo de cargas de trabajo.|
|**max_db_memory**|int|Solo para uso interno.|
|**govern_background_io**|bit|Solo para uso interno.|
|**min_db_max_size_in_mb**|bigint|El valor mínimo max_size para un archivo de datos, en MB. Vea [Sys.database_files](../system-catalog-views/sys-database-files-transact-sql.md).|
|**max_db_max_size_in_mb**|bigint|El valor máximo max_size para un archivo de datos, en MB. Vea [Sys.database_files](../system-catalog-views/sys-database-files-transact-sql.md).|
|**default_db_max_size_in_mb**|bigint|El valor predeterminado max_size para un archivo de datos, en MB. Vea [Sys.database_files](../system-catalog-views/sys-database-files-transact-sql.md).|
|**db_file_growth_in_mb**|bigint|Incremento de crecimiento predeterminado para un archivo de datos, en MB. Vea [Sys.database_files](../system-catalog-views/sys-database-files-transact-sql.md).|
|**initial_db_file_size_in_mb**|bigint|Tamaño predeterminado del nuevo archivo de datos, en MB. Vea [Sys.database_files](../system-catalog-views/sys-database-files-transact-sql.md).|
|**log_size_in_mb**|bigint|Tamaño predeterminado del nuevo archivo de registro, en MB. Vea [Sys.database_files](../system-catalog-views/sys-database-files-transact-sql.md).|
|**instance_cap_cpu**|int|Solo para uso interno.|
|**instance_max_log_rate**|bigint|Límite de velocidad de generación de registros para la instancia de SQL Server, en bytes por segundo. Se aplica a todos los registros generados por la instancia de, incluidas `tempdb` y otras bases de datos del sistema. En un grupo elástico, se aplica al registro generado por todas las bases de datos del grupo.|
|**instance_max_worker_threads**|int|Límite de subprocesos de trabajo para la instancia de SQL Server.|
|**replica_type**|int|Tipo de réplica, donde 0 es principal y 1 es secundario.|
|**max_transaction_size**|bigint|Espacio de registro máximo usado por cualquier transacción, en KB.|
|**checkpoint_rate_mbps**|int|Solo para uso interno.|
|**checkpoint_rate_io**|int|Solo para uso interno.|
|**last_updated_date_utc**|datetime|Fecha y hora del último cambio o reconfiguración de la configuración, en UTC.|
|**primary_group_id**|int|IDENTIFICADOR de grupo de cargas de trabajo para la carga de trabajo de usuario en la réplica principal y en las réplicas secundarias.|
|**primary_group_max_workers**|int|Límite de subprocesos de trabajo para el grupo de cargas de trabajo de usuario.|
|**primary_min_log_rate**|bigint|Velocidad de registro mínima en bytes por segundo en el nivel de grupo de cargas de trabajo de usuario. La regulación de recursos no intentará reducir la velocidad de registro por debajo de este valor.|
|**primary_max_log_rate**|bigint|Velocidad de registro máxima en bytes por segundo en el nivel de grupo de cargas de trabajo de usuario. La regulación de recursos no permitirá la velocidad de registro por encima de este valor.|
|**primary_group_min_io**|int|IOPS mínimas para el grupo de cargas de trabajo de usuario. La regulación de recursos no intentará reducir las IOPS por debajo de este valor.|
|**primary_group_max_io**|int|Número máximo de IOPS para el grupo de cargas de trabajo de usuario. La regulación de recursos no permitirá IOPS por encima de este valor.|
|**primary_group_min_cpu**|FLOAT|Porcentaje mínimo de CPU para el nivel de grupo de cargas de trabajo de usuario. La regulación de recursos no intentará reducir el uso de CPU por debajo de este valor.|
|**primary_group_max_cpu**|FLOAT|Porcentaje máximo de CPU para el nivel de grupo de cargas de trabajo de usuario. La regulación de recursos no permitirá el uso de CPU por encima de este valor.|
|**primary_log_commit_fee**|int|Cuota de confirmación del gobierno de velocidad de registro para el grupo de cargas de trabajo de usuario, en bytes. Una cuota de confirmación aumenta el tamaño de cada registro e/s por un valor fijo solo con fines de contabilidad de tasas de registro. No se aumenta la e/s de registro real al almacenamiento.|
|**primary_pool_max_workers**|int|Límite de subprocesos de trabajo para el grupo de recursos de carga de usuario.|
|**pool_max_io**|int|Límite máximo de IOPS para el grupo de recursos de la carga de trabajo del usuario.|
|**govern_db_memory_in_resource_pool**|bit|Solo para uso interno.|
|**volume_local_iops**|int|Solo para uso interno.|
|**volume_managed_xstore_iops**|int|Solo para uso interno.|
|**volume_external_xstore_iops**|int|Solo para uso interno.|
|**volume_type_local_iops**|int|Solo para uso interno.|
|**volume_type_managed_xstore_iops**|int|Solo para uso interno.|
|**volume_type_external_xstore_iops**|int|Solo para uso interno.|
|**volume_pfs_iops**|int|Solo para uso interno.|
|**volume_type_pfs_iops**|int|Solo para uso interno.|
|||

## <a name="permissions"></a>Permisos

Esta vista necesita el permiso VIEW DATABASE STATE.

## <a name="remarks"></a>Observaciones

Para obtener una descripción de la regulación de recursos en Azure SQL Database, consulte [SQL Database límites de recursos](/azure/sql-database/sql-database-resource-limits-database-server).

> [!IMPORTANT]
> La mayoría de los datos devueltos por esta DMV están destinados al consumo interno y están sujetos a cambios en cualquier momento.

## <a name="examples"></a>Ejemplos

La siguiente consulta, que se ejecuta en el contexto de una base de datos de usuario, devuelve la velocidad de registro máxima y la IOPS máxima en el nivel de grupo de cargas de trabajo de usuario y grupo de recursos. Para una sola base de datos, se devuelve una fila. En el caso de una base de datos en un grupo elástico, se devuelve una fila para cada base de datos del grupo.

```
SELECT database_name,
       primary_group_id,
       primary_max_log_rate,
       primary_group_max_io,
       pool_max_io
FROM sys.dm_user_db_resource_governance
ORDER BY database_name;  
```

## <a name="see-also"></a>Consulte también

- [Regulador de recursos](../resource-governor/resource-governor.md)
- [sys.dm_resource_governor_resource_pools (Transact-SQL)](./sys-dm-resource-governor-resource-pools-transact-sql.md)
- [sys.dm_resource_governor_workload_groups (Transact-SQL)](./sys-dm-resource-governor-workload-groups-transact-sql.md)
- [sys.dm_resource_governor_resource_pools_history_ex (Transact-SQL)](./sys-dm-resource-governor-resource-pools-history-ex-azure-sql-database.md)
- [sys.dm_resource_governor_workload_groups_history_ex (Azure SQL Database)](./sys-dm-resource-governor-workload-groups-history-ex-azure-sql-database.md)
- [Regulación de la tasa de registro de transacciones](/azure/sql-database/sql-database-resource-limits-database-server#transaction-log-rate-governance)
- [Límites de recursos de DTU de bases de datos única](/azure/sql-database/sql-database-dtu-resource-limits-single-databases)
- [Límites de recursos de núcleo virtual de bases de datos únicas](/azure/sql-database/sql-database-vcore-resource-limits-single-databases)