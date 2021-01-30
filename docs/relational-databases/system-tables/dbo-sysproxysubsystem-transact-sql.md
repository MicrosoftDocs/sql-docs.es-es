---
description: dbo.sysproxysubsystem (Transact-SQL)
title: dbo.sysproxysubsystem (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dbo.sysproxysubsystem_TSQL
- dbo.sysproxysubsystem
- sysproxysubsystem_TSQL
- sysproxysubsystem
dev_langs:
- TSQL
helpviewer_keywords:
- sysproxysubsystem system table
ms.assetid: 6d7713f5-1253-4a19-b1fb-635c377c95c1
author: cawrites
ms.author: chadam
ms.openlocfilehash: c5386b54af2487006c790d7c8849264f3e1c50f6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210730"
---
# <a name="dbosysproxysubsystem-transact-sql"></a>dbo.sysproxysubsystem (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Registra qué subsistema del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se utiliza en cada cuenta de proxy. Esta tabla se almacena en la base de datos **msdb** .  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**subsystem_id**|**int**|Id. del subsistema. Este valor corresponde al **subsystem_id** columna de la tabla **syssubsystems** .|  
|**proxy_id**|**int**|Id. de la cuenta de proxy. Este valor corresponde al **proxy_id** columna de la tabla **sysproxies** .|  
  
## <a name="remarks"></a>Observaciones  
 Solo los miembros del rol fijo de servidor **sysadmin** pueden tener acceso a esta tabla.  
  
## <a name="see-also"></a>Consulte también  
 [dbo.syssubsistemas &#40;Transact-SQL&#41;](../../relational-databases/system-tables/dbo-syssubsystems-transact-sql.md)   
 [dbo.sysproxies &#40;Transact-SQL&#41;](../../relational-databases/system-tables/dbo-sysproxies-transact-sql.md)  
  
  
