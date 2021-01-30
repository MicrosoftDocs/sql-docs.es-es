---
description: sys.backup_devices (Transact-SQL)
title: sys.backup_devices (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- backup_devices_TSQL
- backup_devices
- sys.backup_devices
- sys.backup_devices_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- backup devices [SQL Server], viewing information
- sys.backup_devices catalog view
ms.assetid: 457edaa4-aca1-4bd3-bf8d-734490b80fcd
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 1698ea0f99a92c9b93af3eb7eb479bb10b200928
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198583"
---
# <a name="sysbackup_devices-transact-sql"></a>sys.backup_devices (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una fila para cada dispositivo de copia de seguridad registrado mediante **sp_addumpdevice** o creado en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Nombre del dispositivo de copia de seguridad. Es único en el conjunto.|  
|**type**|**tinyint**|Tipo de dispositivo de copia de seguridad:<br /><br /> 2 = Disco<br /><br /> 3 = Disquete (obsoleto)<br /><br /> 5 = Cinta<br /><br /> 6 = Canalización (obsoleto)<br /><br /> 7 = Virtual (opcional, para su uso por proveedores de copia de seguridad de otros fabricantes)<br /><br /> 9 = URL<br /><br />Normalmente, solo se usan el disco (2) y la dirección URL (9).|  
|**type_desc**|**nvarchar(60)**|Descripción del tipo de dispositivo de copia de seguridad:<br /><br /> DISK<br /><br /> DISKETTE (obsoleto)<br /><br /> TAPE<br /><br /> PIPE (obsoleto)<br /><br /> VIRTUAL_DEVICE (opcional, para su uso por proveedores de copia de seguridad de otros fabricantes)<br /><br /> URL <br /><br /> Normalmente, solo se usan el disco y la dirección URL.|  
|**physical_name**|**nvarchar(260)**|Nombre de archivo o ruta de acceso físicos del dispositivo de copia de seguridad.|  
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [Dispositivos de copia de seguridad &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md)   
 [sp_addumpdevice &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md)   
 [Vistas de catálogo de archivos y bases de datos &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/databases-and-files-catalog-views-transact-sql.md)   
 [Consultar las preguntas más frecuentes (P+F) del catálogo del sistema de SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)  
  
  
