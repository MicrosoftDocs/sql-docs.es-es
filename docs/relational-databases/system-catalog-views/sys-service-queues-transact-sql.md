---
description: sys.service_queues (Transact-SQL)
title: sys.service_queues (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.service_queues
- service_queues
- service_queues_TSQL
- sys.service_queues_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.service_queues catalog view
ms.assetid: 9fd9fa76-6128-410c-896f-741e6050143a
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f85fac244c9530a29e14b1aeaf14430feed337b3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204477"
---
# <a name="sysservice_queues-transact-sql"></a>sys.service_queues (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una fila para cada objeto de la base de datos que es una cola de servicio, con **Sys. Objects. Type** = sq.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**\<inherited columns>**||Para obtener una lista de las columnas que hereda esta vista, vea [Sys. objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).|  
|**max_readers**|**smallint**|Número máximo de lectores simultáneos permitido en la cola.|  
|**activation_procedure**|**nvarchar(776**|Nombre de tres partes del procedimiento de activación.|  
|**execute_as_principal_id**|**int**|Id. de la entidad de seguridad de base de datos EXECUTE AS.<br /><br /> NULL de manera predeterminada o si EXECUTE AS CALLER.<br /><br /> IDENTIFICADOR de la entidad de seguridad especificada si EXECUTe AS SELF EXECUTe AS \<principal> .<br /><br /> -2 = EXECUTE AS OWNER.|  
|**is_activation_enabled**|**bit**|1 = La activación está habilitada.|  
|**is_receive_enabled**|**bit**|1 = La recepción está habilitada.|  
|**is_enqueue_enabled**|**bit**|1 = La colocación en cola está habilitada.|  
|**is_retention_enabled**|**bit**|1 = Los mensajes se retienen hasta que finaliza el diálogo.|  
|**is_poison_message_handling_enabled**|**bit**|**Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.<br /><br /> 1 = Se habilita el control de mensajes dudosos.|  
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Consulte también  
 [Object Catalog Views &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)  (Vistas de catálogo de objetos [Transact-SQL])  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
