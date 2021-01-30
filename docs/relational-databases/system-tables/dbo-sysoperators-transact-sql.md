---
description: dbo.sysoperators (Transact-SQL)
title: Operadores de dbo.sys(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysoperators
- dbo.sysoperators
- dbo.sysoperators_TSQL
- sysoperators_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysoperators system table
ms.assetid: c2afa20c-b15f-46ca-ae74-2eb65909409e
author: cawrites
ms.author: chadam
ms.openlocfilehash: 80084de44f0bd1f859e6df5a83116b55cca74045
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195051"
---
# <a name="dbosysoperators-transact-sql"></a>dbo.sysoperators (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una fila por cada operador del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**id**|**int**|Id. del operador.|  
|**name**|**sysname**|nombre de la operación.|  
|**enabled**|**tinyint**|Estado de las notificaciones de alerta (booleano). Si es **1**, este operador puede recibir notificaciones cuando se produce una alerta.|  
|**email_address**|**nvarchar(100**|Dirección de correo electrónico para este operador.|  
|**last_email_date**|**int**|Fecha en que el operador recibió por última vez una alerta por correo electrónico.|  
|**last_email_time**|**int**|Hora a la que el operador recibió por última vez una alerta por correo electrónico.|  
|**pager_address**|**nvarchar(100**|Dirección de buscapersonas del operador.|  
|**last_pager_date**|**int**|Fecha en que el operador recibió por última vez una notificación de alerta por buscapersonas.|  
|**last_pager_time**|**int**|Hora a la que el operario recibió por última vez una notificación de alerta por buscapersonas.|  
|**weekday_pager_start_time**|**int**|Hora de día laborable (de lunes a viernes) a partir de la cual el operador puede recibir una notificación de alerta por buscapersonas.|  
|**weekday_pager_end_time**|**int**|Hora de día laborable (de lunes a viernes) a partir de la cual el operador no puede recibir una notificación de alerta por buscapersonas.|  
|**saturday_pager_start_time**|**int**|Hora del sábado a partir de la cual el operador puede recibir una notificación de alerta por buscapersonas.|  
|**saturday_pager_end_time**|**int**|Hora del sábado a partir de la cual el operador no puede recibir una notificación de alerta por buscapersonas.|  
|**sunday_pager_start_time**|**int**|Hora del domingo a partir de la cual el operador puede recibir una notificación de alerta por buscapersonas.|  
|**sunday_pager_end_time**|**int**|Hora del domingo a partir de la cual el operador no puede recibir una notificación de alerta por buscapersonas.|  
|**pager_days**|**tinyint**|Máscara de bits que indica los días de la semana en que el operador puede recibir notificaciones de alerta por buscapersonas.|  
|**netsend_address**|**nvarchar(100**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**last_netsend_date**|**int**|Fecha en que se envió el mensaje de red más reciente al Id. de operador especificado.|  
|**last_netsend_time**|**int**|Hora a la que se envió el mensaje de red más reciente al Id. de operador especificado.|  
|**category_id**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de Agente SQL Server &#40;&#41;de Transact-SQL ](../../relational-databases/system-tables/sql-server-agent-tables-transact-sql.md)  
  
  
