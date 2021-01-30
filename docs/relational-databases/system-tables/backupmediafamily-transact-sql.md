---
title: backupmediafamily (Transact-SQL) | Microsoft Docs
description: Referencia de backupmediafamily, que contiene una fila por cada familia de medios.
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- backupmediafamily
- backupmediafamily_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- backupmediafamily system table
- backup media [SQL Server], backupmediafamily system table
author: cawrites
ms.author: chadam
ms.openlocfilehash: df687040bf73fb1d78f15d7cca18cc0fc092ee1f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198676"
---
# <a name="backupmediafamily-transact-sql"></a>backupmediafamily (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Contiene una fila por cada familia de medios. Si una familia de medios reside en un conjunto de medios reflejado, la familia tiene una fila independiente para cada reflejo del conjunto de medios. Esta tabla se almacena en la base de datos **msdb** .  
    
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**media_set_id**|**int**|Número de identificación exclusivo que identifica el conjunto de medios al que pertenece esta familia. Referencias **backupmediaset (media_set_id)**|  
|**family_sequence_number**|**tinyint**|Posición de esta familia de medios en el conjunto de medios.|  
|**media_family_id**|**uniqueidentifier**|Número de identificación exclusivo que identifica a la familia de medios. Puede ser NULL.|  
|**media_count**|**int**|Número de medios en la familia. Puede ser NULL.|  
|**logical_device_name**|**nvarchar(128)**|Nombre de este dispositivo de copia de seguridad en **Sys.backup_devices. nombre**. Si se trata de un dispositivo de copia de seguridad temporal (en lugar de un dispositivo de copia de seguridad permanente que existe en **Sys.backup_devices**), el valor de **logical_device_name** es NULL.|  
|**physical_device_name**|**nvarchar(260)**|Nombre físico del dispositivo de copia de seguridad. Puede ser NULL. Este campo se comparte entre el proceso de copia de seguridad y restauración. Puede contener la ruta de acceso de destino de la copia de seguridad original o la ruta de acceso del origen de restauración original. Dependiendo de si la copia de seguridad o la restauración se produjeron en primer lugar en un servidor para una base de datos. Tenga en cuenta que las restauraciones consecutivas del mismo archivo de copia de seguridad no actualizarán la ruta de acceso, independientemente de su ubicación en el momento de la restauración. Por este motivo, **physical_device_name** campo no se puede usar para ver la ruta de acceso de restauración utilizada.|  
|**device_type**|**tinyint**|Tipo de dispositivo de copia de seguridad:<br /><br /> 2 = Disco<br /><br /> 5 = Cinta<br /><br /> 7 = Dispositivo virtual<br /><br /> 9 = Azure Storage<br /><br /> 105 = Dispositivo de copia de seguridad permanente<br /><br /> Puede ser NULL.<br /><br /> Todos los nombres de dispositivo y números de dispositivo permanentes se pueden encontrar en **Sys.backup_devices**.|  
|**physical_block_size**|**int**|Tamaño de bloque físico utilizado para escribir en la familia de medios. Puede ser NULL.|  
|**mirror**|**tinyint**|Número de reflejo (0-3)|  
  
## <a name="remarks"></a>Observaciones  
 RESTOre VERIFYONLY de *backup_device* con LOADHISTORY rellena las columnas de la tabla **backupmediaset** con los valores adecuados del encabezado de conjunto de medios.  
  
 Para reducir el número de filas de esta tabla y de otras tablas de historial y copia de seguridad, ejecute el [sp_delete_backuphistory](../../relational-databases/system-stored-procedures/sp-delete-backuphistory-transact-sql.md) procedimiento almacenado.  
  
## <a name="see-also"></a>Consulte también  
 [Copias de seguridad y restauración de tablas &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backup-and-restore-tables-transact-sql.md)   
 [backupfile &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupfile-transact-sql.md)   
 [backupfilegroup &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupfilegroup-transact-sql.md)   
 [backupmediaset &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupmediaset-transact-sql.md)   
 [backupset &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupset-transact-sql.md)   
 [Tablas del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  
