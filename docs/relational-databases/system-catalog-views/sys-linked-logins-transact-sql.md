---
description: sys.linked_logins (Transact-SQL)
title: sys.linked_logins (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.linked_logins
- sys.linked_logins_TSQL
- linked_logins_TSQL
- linked_logins
dev_langs:
- TSQL
helpviewer_keywords:
- sys.linked_logins catalog view
ms.assetid: af57bf0c-a265-410f-9bab-63b78569b4a6
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: cd771e7199ce5a120ad0ef6c4c5f0546cfdb426c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191379"
---
# <a name="syslinked_logins-transact-sql"></a>sys.linked_logins (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve una fila por asignación de inicio de sesión de servidor vinculado para su uso por parte de las RPC y las consultas distribuidas desde el servidor local al servidor vinculado correspondiente.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**server_id**|**int**|IDENTIFICADOR del servidor en **Sys.** servers.|  
|**local_principal_id**|**int**|Entidad de seguridad de servidor a la que se aplica la asignación.<br /><br /> 0 = comodín o public.|  
|**uses_self_credential**|**bit**|Si es 1, la asignación indica que la sesión debe utilizar sus propias credenciales; por el contrario, 0 indica que la sesión utiliza el nombre y la contraseña proporcionados.|  
|**remote_name**|**sysname**|Nombre de usuario remoto que se va a utilizar al conectar. La contraseña también se almacena, pero no se expone en las interfaces de la vista de catálogo.|  
|**modify_date**|**datetime**|Fecha en que se cambió el inicio de sesión vinculado por última vez.|  
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Servidores vinculados vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/linked-servers-catalog-views-transact-sql.md)  
  
  
