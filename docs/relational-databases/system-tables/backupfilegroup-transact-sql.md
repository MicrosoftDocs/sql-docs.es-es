---
description: backupfilegroup (Transact-SQL)
title: backupfilegroup (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- backupfilegroup_TSQL
- backupfilegroup
dev_langs:
- TSQL
helpviewer_keywords:
- filegroups [SQL Server], backupfilegroup system table
- backupfilegroup system table
ms.assetid: d26e8fbe-f5c5-4e10-b2bd-0d8e16ea21f9
author: cawrites
ms.author: chadam
ms.openlocfilehash: be220cdc204488f0c2f82244b543ae324218bc49
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198663"
---
# <a name="backupfilegroup-transact-sql"></a>backupfilegroup (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una fila por cada grupo de archivos de una base de datos en el momento de crear la copia de seguridad. **backupfilegroup** se almacena en la base de datos **msdb** .  
  
> [!NOTE]  
>  La tabla **backupfilegroup** muestra la configuración del grupo de archivos de la base de datos, no del conjunto de copia de seguridad. Para identificar si un archivo está incluido en el conjunto de copia de seguridad, use la **is_present** columna de la tabla [backupfile](../../relational-databases/system-tables/backupfile-transact-sql.md) .  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**backup_set_id**|**int**|Conjunto de copia de seguridad que contiene este grupo de archivos.|  
|**name**|**sysname**|Nombre del grupo de archivos.|  
|**filegroup_id**|**int**|Id. del grupo de archivos; único en la base de datos. Corresponde a **data_space_id** en **Sys. grupos** de archivos.|  
|**filegroup_guid**|**uniqueidentifier**|Identificador único global para el grupo de archivos. Puede ser NULL.|  
|**type**|**char(2)**|Tipo de contenido, uno de los siguientes:<br /><br /> FG = Grupo de archivos "Rows"<br /><br /> SL = Grupo de archivos de registro de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|**type_desc**|**nvarchar(60)**|Descripción del tipo de función, una de:<br /><br /> ROWS_FILEGROUP <br /><br /> SQL_LOG_FILEGROUP |  
|**is_default**|**bit**|Grupo de archivos predeterminado, que se utiliza cuando no se ha especificado ningún otro en CREATE TABLE o CREATE INDEX.|  
|**is_readonly**|**bit**|1 = El grupo de archivos es de solo lectura.|  
|**log_filegroup_guid**|**uniqueidentifier**|Puede ser NULL.|  
  
## <a name="remarks"></a>Observaciones  
  
> [!IMPORTANT]  
>  El mismo nombre de grupo de archivos puede aparecer en diferentes bases de datos; no obstante, cada grupo de archivos tiene su propio GUID. Por lo tanto, **(backup_set_id, filegroup_guid)** es una clave única que identifica un grupo de archivos en **backupfilegroup**.  
  
 RESTOre VERIFYONLY de *backup_device* con LOADHISTORY rellena las columnas de la tabla **backupmediaset** con los valores adecuados del encabezado de conjunto de medios.  
  
 Para reducir el número de filas de esta tabla y de otras tablas de historial y copia de seguridad, ejecute el [sp_delete_backuphistory](../../relational-databases/system-stored-procedures/sp-delete-backuphistory-transact-sql.md) procedimiento almacenado.  
  
## <a name="see-also"></a>Consulte también  
 [Copias de seguridad y restauración de tablas &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backup-and-restore-tables-transact-sql.md)   
 [backupfile &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupfile-transact-sql.md)   
 [backupmediafamily &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupmediafamily-transact-sql.md)   
 [backupmediaset &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupmediaset-transact-sql.md)   
 [backupset &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupset-transact-sql.md)   
 [Tablas del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  
