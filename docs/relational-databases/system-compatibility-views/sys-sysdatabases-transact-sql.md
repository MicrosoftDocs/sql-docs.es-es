---
description: sys.sysdatabases (Transact-SQL)
title: Bases de datos de sys.sys(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysdatabases_TSQL
- sys.sysdatabases_TSQL
- sys.sysdatabases
- sysdatabases
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sysdatabases compatibility view
- sysdatabases system table
ms.assetid: 60a93880-62f1-4eda-a886-f046706ba90c
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f742ce25d05d588cb70e9d68b254e06f8e478500
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201416"
---
# <a name="syssysdatabases-transact-sql"></a>sys.sysdatabases (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Contiene una fila por cada base de datos de una instancia de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Cuando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se instala por primera vez, **sysdatabases** contiene entradas para las bases de datos **Master**, **Model**, **msdb** y **tempdb** .  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Nombre de la base de datos|  
|**DBID**|**smallint**|Identificador de base de datos|  
|**Junction**|**varbinary(85)**|Id. de sistema del creador de la base de datos|  
|**mode**|**smallint**|Utilizado internamente para bloquear una base de datos mientras se crea.|  
|**status**|**int**|Bits de estado, algunos de los cuales se pueden establecer mediante [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md) , como se indica a continuación:<br /><br /> 1 = **autoclose** (ALTER DATABASE)<br /><br /> 4 = **SELECT INTO/bulkcopy** (ALTER DATABASE con set Recovery)<br /><br /> 8 = **trunc. log on chkpt** (ALTER DATABASE con set Recovery)<br /><br /> 16 = **detección de página rasgada** (ALTER DATABASE)<br /><br /> 32 = **cargando**<br /><br /> 64 = **pre-Recovery**<br /><br /> 128 = en **recuperación**<br /><br /> 256 = **no recuperado**<br /><br /> 512 = **sin conexión** (ALTER DATABASE)<br /><br /> 1024 = **solo lectura** (ALTER DATABASE)<br /><br /> 2048 = **solo uso de DBO** (ALTER DATABASE con set RESTRICTED_USER)<br /><br /> 4096 = **usuario único** (ALTER DATABASE)<br /><br /> 32768 = **modo de emergencia**<br /><br /> 65536 = **checksum** (ALTER DATABASE)<br /><br /> 4194304 = **AutoShrink** (ALTER DATABASE)<br /><br /> 1073741824 = **apagado limpio**<br /><br /> Puede haber varios bits establecidos en ON a la vez.|  
|**status2**|**int**|16384 = **ANSI null default** (ALTER DATABASE)<br /><br /> 65536 = **concat null produce null** (ALTER DATABASE)<br /><br /> 131072 = **desencadenadores recursivos** (ALTER DATABASE)<br /><br /> 1048576 = el **valor predeterminado es cursor local** (ALTER DATABASE)<br /><br /> 8388608 = **identificador entre comillas** (ALTER DATABASE)<br /><br /> 33554432 = **cierre del cursor al confirmar** (ALTER DATABASE)<br /><br /> 67108864 = **valores NULL ANSI** (ALTER DATABASE)<br /><br /> 268435456 = **advertencias ANSI** (ALTER DATABASE)<br /><br /> 536870912 = **texto completo habilitado** (establecido mediante **sp_fulltext_database**)|  
|**crdate**|**datetime**|Fecha de creación|  
|**sector**|**datetime**|Reservado para un uso futuro.|  
|**category**|**int**|Contiene un mapa de bits de información utilizado en la replicación:<br /><br /> 1 = Publicado para réplica de instantáneas o replicación transaccional.<br /><br /> 2 = Suscrito a una publicación de instantáneas o transaccional.<br /><br /> 4 = Publicado para replicación de mezcla.<br /><br /> 8 = Suscrito a una publicación de combinación.<br /><br /> 16 = Base de datos de distribución.|  
|**cmptlevel**|**tinyint**|Nivel de compatibilidad de la base de datos. Para obtener más información, vea [Nivel de compatibilidad de ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).|  
|**filename**|**nvarchar(260)**|Ruta de acceso y nombre en el sistema operativo del archivo principal de la base de datos.<br /><br /> **filename** es visible para **dbcreator**, **sysadmin**, el propietario de la base de datos con permisos para crear cualquier base de datos o receptores con cualquiera de los siguientes permisos: ALTER any Database, Create any Database, View any Definition. Para devolver la ruta de acceso y el nombre de archivo, consulte la vista de compatibilidad de [sys.sysarchivos](../../relational-databases/system-compatibility-views/sys-sysfiles-transact-sql.md) o la vista de [Sys.database_files](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md) .|  
|**version**|**smallint**|Número interno de versión del código de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con el que se creó la base de datos. [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
  
## <a name="see-also"></a>Consulte también  
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [Asignar tablas del sistema a vistas del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Vistas de compatibilidad &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
