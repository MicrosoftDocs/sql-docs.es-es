---
description: sys.services (Transact-SQL)
title: Sys. Services (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.services
- services
- services_TSQL
- sys.services_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.services catalog view
ms.assetid: 16d0b0c5-5cce-469b-aa3d-4b9248e0c085
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 49bda8cfb3e8d62824b92a9faaac8d02db100b12
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204468"
---
# <a name="sysservices-transact-sql"></a>sys.services (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Esta vista de catálogo contiene una fila por cada servicio en la base de datos.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Nombre que distingue entre mayúsculas y minúsculas del servicio, único en la base de datos. No acepta valores NULL.|  
|**service_id**|**int**|Identificador del servicio. No acepta valores NULL.|  
|**principal_id**|**int**|Identificador de la entidad de seguridad de base de datos propietaria del servicio. Acepta valores NULL.|  
|**service_queue_id**|**int**|Id. del objeto de la cola que usa este servicio. No acepta valores NULL.|  
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
  
