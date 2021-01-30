---
description: sp_update_agent_profile (Transact-SQL)
title: sp_update_agent_profile (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_update_agent_profile_TSQL
- sp_update_agent_profile
helpviewer_keywords:
- sp_update_agent_profile
ms.assetid: cc81f227-0df3-4151-bb4d-4f45ea997b71
author: markingmyname
ms.author: maghan
ms.openlocfilehash: d165fa80f51d1d5b018bd67eda3c4fb06eb5bc52
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209652"
---
# <a name="sp_update_agent_profile-transact-sql"></a>sp_update_agent_profile (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Actualiza el perfil utilizado por un agente de replicación. Este procedimiento almacenado se ejecuta en el distribuidor de la base de datos de distribución.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_update_agent_profile [@agent_type=] agent_type, [ @agent_id= ] agent_id, [ @profile_id= ] profile_id  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @agent_type = ] 'agent_type'` Es el tipo de agente. *agent_type* es de **tipo int**, no tiene ningún valor predeterminado y puede tener uno de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**1**|Agente de instantáneas.|  
|**2**|Agente de registro del LOG.|  
|**3**|Agente de distribución.|  
|**4**|Agente de mezcla.|  
|**9**|Agente de lectura de cola|  
  
`[ @agent_id = ] 'agent_id'` Es el identificador del agente. *agent_id* es de **tipo int** y no tiene ningún valor predeterminado.  
  
`[ @profile_id = ] 'profile_id'` Es el identificador del perfil que debe utilizar el agente. *profile_id* es de **tipo int** y no tiene ningún valor predeterminado. Para ver una lista de los perfiles definidos para cada agente, use [sp_help_agent_profile &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-agent-profile-transact-sql.md). Para obtener más información acerca de los perfiles del sistema, consulte [Replication Agent profiles](../../relational-databases/replication/agents/replication-agent-profiles.md).  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_update_agent_profile** se utiliza en todos los tipos de replicación.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** pueden ejecutar **sp_update_agent_profile**.  
  
## <a name="see-also"></a>Consulte también  
 [Perfiles del Agente de replicación](../../relational-databases/replication/agents/replication-agent-profiles.md)   
 [sp_add_agent_profile &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-agent-profile-transact-sql.md)   
 [sp_change_agent_profile &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-change-agent-profile-transact-sql.md)   
 [sp_drop_agent_profile &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-drop-agent-profile-transact-sql.md)   
 [sp_help_agent_profile &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-agent-profile-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
