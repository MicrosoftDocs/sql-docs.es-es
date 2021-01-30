---
description: dbo.sysjobs (Transact-SQL)
title: Trabajos de dbo.sys(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysjobs
- sysjobs_TSQL
- dbo.sysjobs
- dbo.sysjobs_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysjobs system table
ms.assetid: e244a6a5-54c2-47a6-8039-dd1852b0ae59
author: cawrites
ms.author: chadam
ms.openlocfilehash: 58cf513f91b414e5ad98eb388238e69be151e9a0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210427"
---
# <a name="dbosysjobs-transact-sql"></a>dbo.sysjobs (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Almacena la información de cada trabajo programado que debe ejecutar el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Esta tabla se almacena en la base de datos **msdb** .  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**job_id**|**uniqueidentifier**|Id. único del trabajo.|  
|**originating_server_id**|**int**|Id. del servidor del que proviene el trabajo.|  
|**name**|**sysname**|Nombre del trabajo.|  
|**enabled**|**tinyint**|Indica si el trabajo está habilitado para su ejecución.|  
|**description**|**nvarchar(512)**|Descripción del trabajo.|  
|**start_step_id**|**int**|Id. del paso del trabajo en el que debe comenzar la ejecución.|  
|**category_id**|**int**|Id. de la categoría del trabajo.|  
|**owner_sid**|**varbinary(85)**|Número identificador de seguridad (SID) del propietario del trabajo.|  
|**notify_level_eventlog**|**int**|**Máscara** de archivo que indica en qué circunstancias se debe registrar un evento de notificación en el registro de aplicación de Microsoft Windows:<br /><br /> **0** = nunca<br /><br /> **1** = cuando el trabajo se realiza correctamente<br /><br /> **2** = cuando se produce un error en el trabajo<br /><br /> **3** = cada vez que se completa el trabajo (independientemente del resultado del trabajo)|  
|**notify_level_email**|**int**|Máscara de bits que indica en qué circunstancias se debe enviar una notificación por correo electrónico cuando se completa un trabajo:<br /><br /> **0** = nunca<br /><br /> **1** = cuando el trabajo se realiza correctamente<br /><br /> **2** = cuando se produce un error en el trabajo<br /><br /> **3** = cada vez que se completa el trabajo (independientemente del resultado del trabajo)|  
|**notify_level_netsend**|**int**|Máscara de bits que indica en qué circunstancias se debe enviar un mensaje de red cuando se completa un trabajo:<br /><br /> **0** = nunca<br /><br /> **1** = cuando el trabajo se realiza correctamente<br /><br /> **2** = cuando se produce un error en el trabajo<br /><br /> **3** = cada vez que se completa el trabajo (independientemente del resultado del trabajo)|  
|**notify_level_page**|**int**|Máscara de bits que indica en qué circunstancias se debe enviar un mensaje a un localizador cuando se completa un trabajo:<br /><br /> **0** = nunca<br /><br /> **1** = cuando el trabajo se realiza correctamente<br /><br /> **2** = cuando se produce un error en el trabajo<br /><br /> **3** = cada vez que se completa el trabajo (independientemente del resultado del trabajo)|  
|**notify_email_operator_id**|**int**|Nombre de correo electrónico del operador que recibe la notificación.|  
|**notify_netsend_operator_id**|**int**|Id. del equipo o del usuario utilizado para enviar mensajes de red.|  
|**notify_page_operator_id**|**int**|Id. del equipo o del usuario utilizado para enviar un mensaje de localizador.|  
|**delete_level**|**int**|**Máscara de máscara** que indica en qué circunstancias se debe eliminar el trabajo cuando se completa un trabajo:<br /><br /> **0** = nunca<br /><br /> **1** = cuando el trabajo se realiza correctamente<br /><br /> **2** = cuando se produce un error en el trabajo<br /><br /> **3** = cada vez que se completa el trabajo (independientemente del resultado del trabajo)|  
|**date_created**|**datetime**|Fecha en que se creó el trabajo.|  
|**date_modified**|**datetime**|Fecha en que se modificó el trabajo por última vez.|  
|**version_number**|**int**|Versión del trabajo.|  
  
  
