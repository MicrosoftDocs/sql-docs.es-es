---
description: Asignar tablas del sistema a vistas del sistema (Transact-SQL)
title: Asignar tablas del sistema a vistas del sistema (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- catalog views [SQL Server], mapping system tables to
- mapping system tables to system views [SQL Server]
- system tables [SQL Server], mapping to catalog views
ms.assetid: a616fce9-b4c1-49da-87a7-9d6f74911d8f
author: cawrites
ms.author: chadam
ms.openlocfilehash: 043e31e13dfddb7044d2a57796f69e10d56b340f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160545"
---
# <a name="mapping-system-tables-to-system-views-transact-sql"></a>Asignar tablas del sistema a vistas del sistema (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  En este tema se muestra la asignación entre las tablas y funciones del sistema y las vistas y funciones del sistema.  
  
 La tabla siguiente muestra la asignación de las tablas del sistema a sus correspondientes vistas o funciones del sistema en [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
|Tabla del sistema|Vistas o funciones del sistema|Tipo de vista o función|  
|------------------|-------------------------------|------------------------------|  
|sysaltfiles|[sys.master_files](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)|Vista de catálogo|  
|syscacheobjects|[sys.dm_exec_cached_plans](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)<br /><br /> [sys.dm_exec_plan_attributes &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-plan-attributes-transact-sql.md)<br /><br /> [sys.dm_exec_sql_text](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)<br /><br /> [sys.dm_exec_cached_plan_dependent_objects](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plan-dependent-objects-transact-sql.md)|Vista de administración dinámica<br /><br /> Vista de administración dinámica<br /><br /> Vista de administración dinámica<br /><br /> Vista de administración dinámica|  
|syscharsets|[sys.syscharsets](../../relational-databases/system-compatibility-views/sys-syscharsets-transact-sql.md)|Vista de compatibilidad|  
|sysconfigures|[sys.configurations](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md)|Vista de catálogo|  
|syscurconfigs|[sys.configurations](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md)|Vista de catálogo|  
|sysdatabases|[sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)|Vista de catálogo|  
|sysdevices|[sys.backup_devices](../../relational-databases/system-catalog-views/sys-backup-devices-transact-sql.md)|Vista de catálogo|  
|syslanguages|[sys.syslanguages](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql.md)|Vista de compatibilidad|  
|syslockinfo|[sys.dm_tran_locks](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)|Vista de administración dinámica|  
|syslocks|[sys.dm_tran_locks](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)|Vista de administración dinámica|  
|syslogins|[sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)<br /><br /> [sys.sql_logins](../../relational-databases/system-catalog-views/sys-sql-logins-transact-sql.md)|Vista de catálogo|  
|sysmessages|[sys.messages](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md)|Vista de catálogo|  
|sysoledbusers|[sys.linked_logins](../../relational-databases/system-catalog-views/sys-linked-logins-transact-sql.md)|Vista de catálogo|  
|sysopentapes|[sys.dm_io_backup_tapes](../../relational-databases/system-dynamic-management-views/sys-dm-io-backup-tapes-transact-sql.md)|Vista de administración dinámica|  
|sysperfinfo|[sys.dm_os_performance_counters](../../relational-databases/system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md)|Vista de administración dinámica|  
|sysprocesses|[sys.dm_exec_connections](../../relational-databases/system-dynamic-management-views/sys-dm-exec-connections-transact-sql.md)<br /><br /> [sys.dm_exec_sessions](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md)<br /><br /> [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)|Vista de administración dinámica<br /><br /> Vista de administración dinámica<br /><br /> Vista de administración dinámica|  
|sysremotelogins|[sys.remote_logins](../../relational-databases/system-catalog-views/sys-remote-logins-transact-sql.md)|Vista de catálogo|  
|sysservers|[sys.servers](../../relational-databases/system-catalog-views/sys-servers-transact-sql.md)|Vista de catálogo|  
  
 En la siguiente tabla se asignan las tablas o funciones del sistema que se encuentran en todas las bases de datos de [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] a sus vistas o funciones del sistema correspondientes de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
|Tabla o función del sistema|Vista o función del sistema|Tipo de vista o función|  
|------------------------------|-----------------------------|------------------------------|  
|fn_virtualfilestats|[sys.dm_io_virtual_file_stats](../../relational-databases/system-dynamic-management-views/sys-dm-io-virtual-file-stats-transact-sql.md)|Vista de administración dinámica|  
|syscolumns|[sys.columns](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)|Vista de catálogo|  
|syscomments|[sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)|Vista de catálogo|  
|sysconstraints|[sys.check_constraints](../../relational-databases/system-catalog-views/sys-check-constraints-transact-sql.md)<br /><br /> [sys.default_constraints](../../relational-databases/system-catalog-views/sys-default-constraints-transact-sql.md)<br /><br /> [sys.key_constraints](../../relational-databases/system-catalog-views/sys-key-constraints-transact-sql.md)<br /><br /> [sys.foreign_keys](../../relational-databases/system-catalog-views/sys-foreign-keys-transact-sql.md)|Vista de catálogo<br /><br /> Vista de catálogo<br /><br /> Vista de catálogo<br /><br /> Vista de catálogo|  
|sysdepends|[sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)|Vista de catálogo|  
|sysfilegroups|[sys.filegroups](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md)|Vista de catálogo|  
|sysfiles|[sys.database_files](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)|Vista de catálogo|  
|sysforeignkeys|[sys.foreign_key_columns](../../relational-databases/system-catalog-views/sys-foreign-key-columns-transact-sql.md)|Vista de catálogo|  
|sysindexes|[sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)<br /><br /> [sys.partitions](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md)<br /><br /> [sys.allocation_units](../../relational-databases/system-catalog-views/sys-allocation-units-transact-sql.md)<br /><br /> [sys.dm_db_partition_stats](../../relational-databases/system-dynamic-management-views/sys-dm-db-partition-stats-transact-sql.md)|Vista de catálogo<br /><br /> Vista de catálogo<br /><br /> Vista de catálogo<br /><br /> Vista de administración dinámica|  
|sysindexkeys|[sys.index_columns](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)|Vista de catálogo|  
|sysmembers|[sys.database_role_members](../../relational-databases/system-catalog-views/sys-database-role-members-transact-sql.md)|Vista de catálogo|  
|sysobjects|[sys.objects](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)|Vista de catálogo|  
|syspermissions|[sys.database_permissions](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md)<br /><br /> [sys.server_permissions](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md)|Vista de catálogo<br /><br /> Vista de catálogo|  
|sysprotects|[sys.database_permissions](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md)<br /><br /> [sys.server_permissions](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md)|Vista de catálogo<br /><br /> Vista de catálogo|  
|sysreferences|[sys.foreign_keys](../../relational-databases/system-catalog-views/sys-foreign-keys-transact-sql.md)|Vista de catálogo|  
|systypes|[sys.types](../../relational-databases/system-catalog-views/sys-types-transact-sql.md)|Vista de catálogo|  
|sysusers|[sys.database_principals](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)|Vista de catálogo|  
|sysfulltextcatalogs|[sys.fulltext_catalogs](../../relational-databases/system-catalog-views/sys-fulltext-catalogs-transact-sql.md)|Vista de catálogo|  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Tablas del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  
