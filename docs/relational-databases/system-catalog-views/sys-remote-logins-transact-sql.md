---
description: sys.remote_logins (Transact-SQL)
title: sys.remote_logins (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.remote_logins_TSQL
- remote_logins
- remote_logins_TSQL
- sys.remote_logins
dev_langs:
- TSQL
helpviewer_keywords:
- sys.remote_logins catalog view
ms.assetid: 38477e91-d084-4df7-b1de-b930c5580189
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: b66a0dc17660d4ff5c379dbedc9ebd629aa8c371
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206850"
---
# <a name="sysremote_logins-transact-sql"></a>sys.remote_logins (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve una fila por cada asignación de inicio de sesión remoto. Esta vista de catálogo se utiliza para asignar inicios de sesión locales entrantes originados en el servidor correspondiente para un inicio de sesión local real.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**server_id**|**int**|IDENTIFICADOR del servidor en **Sys.** servers. Este nombre es suministrado por la conexión desde el servidor "remoto".|  
|**remote_name**|**sysname**|Nombre de inicio de sesión que suministra la conexión para su asignación. Si el valor es NULL, se utiliza el nombre de inicio de sesión especificado en la conexión.|  
|**local_principal_id**|**int**|Id. de la entidad de seguridad del servidor a la que se asigna el inicio de sesión. Si el valor es 0, el inicio de sesión remoto se asigna al inicio de sesión con el mismo nombre.|  
|**modify_date**|**datetime**|Fecha en que se cambió el inicio de sesión vinculado por última vez.|  
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Consulte también  
 [Servidores vinculados vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/linked-servers-catalog-views-transact-sql.md)   
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
