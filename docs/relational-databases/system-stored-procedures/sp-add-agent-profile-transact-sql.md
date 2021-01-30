---
description: sp_add_agent_profile (Transact-SQL)
title: sp_add_agent_profile (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_add_agent_profile
- sp_add_agent_profile_TSQL
helpviewer_keywords:
- sp_add_agent_profile
ms.assetid: 5c246a33-2c21-4a77-9c2a-a2c9f0c5dda1
author: mashamsft
ms.author: mathoma
ms.openlocfilehash: bd89f620f2640c9e88f03ef44909c844187d8c8f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186079"
---
# <a name="sp_add_agent_profile-transact-sql"></a>sp_add_agent_profile (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Crea un nuevo perfil para un agente de replicación. Este procedimiento almacenado se ejecuta en el distribuidor de cualquier base de datos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_add_agent_profile [ [ @profile_id= ] profile_id OUTPUT ]  
        , [ @profile_name= ] 'profile_name'   
        , [ @agent_type= ] 'agent_type' ]   
    [ , [ @profile_type= ] profile_type ]  
    [ , [ @description= ] 'description' ]  
    [ , [ @default= ] default ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @profile_id = ] profile_id` Es el identificador asociado al perfil que se acaba de insertar. *profile_id* es de **tipo int** y es un parámetro de salida opcional. Si se especifica, el valor se establece en el nuevo Id. de perfil.  
  
`[ @profile_name = ] 'profile_name'` Es el nombre del perfil. *profile_name* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @agent_type = ] 'agent_type'` Es el tipo de agente de replicación. *agent_type* es de **tipo int**, no tiene ningún valor predeterminado y puede tener uno de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**1**|Agente de instantáneas|  
|**2**|Agente de registro del LOG|  
|**3**|Agente de distribución|  
|**4**|Agente de mezcla|  
|**9**|Agente de lectura de cola|  
  
`[ @profile_type = ] profile_type` Es el tipo de perfil. *profile_type* es de **tipo int** y su valor predeterminado es **1**.  
  
 **0** indica un perfil del sistema. **1** indica un perfil personalizado. Solo se pueden crear perfiles personalizados mediante este procedimiento almacenado; por lo tanto, el único valor válido es **1**. Solo [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crea perfiles del sistema.  
  
`[ @description = ] 'description'` Es una descripción del perfil. la *Descripción* es de tipo **nvarchar (3000)** y no tiene ningún valor predeterminado.  
  
`[ @default = ] default` Indica si el perfil es el predeterminado para *agent_type * *.* el *valor predeterminado* es **bit**, con un valor predeterminado de **0**. **1** indica que el perfil que se va a agregar se convertirá en el nuevo perfil predeterminado para el agente especificado por *agent_type*.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_add_agent_profile** se utiliza en la replicación de instantáneas, la replicación transaccional y la replicación de mezcla.  
  
 Los perfiles de agente personalizados se agregan con los valores predeterminados de los parámetros de agente. Use [sp_change_agent_parameter &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-change-agent-parameter-transact-sql.md) para cambiar estos valores predeterminados o [sp_add_agent_parameter &#40;transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-agent-parameter-transact-sql.md) para agregar parámetros adicionales.  
  
 Cuando se ejecuta **sp_add_agent_profile** , se agrega una fila para el nuevo perfil personalizado en el [MSagent_profiles &#40;tabla de&#41;de Transact-SQL](../../relational-databases/system-tables/msagent-profiles-transact-sql.md) y los parámetros predeterminados asociados para este perfil se agregan a la MSagent_parameters &#40;tabla&#41;de [Transact-SQL](../../relational-databases/system-tables/msagent-parameters-transact-sql.md) .  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** pueden ejecutar **sp_add_agent_profile**.  
  
## <a name="see-also"></a>Consulte también  
 [Trabajar con perfiles del Agente de replicación](../../relational-databases/replication/agents/work-with-replication-agent-profiles.md)   
 [Perfiles del Agente de replicación](../../relational-databases/replication/agents/replication-agent-profiles.md)   
 [sp_add_agent_parameter &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-agent-parameter-transact-sql.md)   
 [sp_change_agent_parameter &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-change-agent-parameter-transact-sql.md)   
 [sp_change_agent_profile &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-change-agent-profile-transact-sql.md)   
 [sp_drop_agent_parameter &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-drop-agent-parameter-transact-sql.md)   
 [sp_drop_agent_profile &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-drop-agent-profile-transact-sql.md)   
 [sp_help_agent_parameter &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-agent-parameter-transact-sql.md)   
 [sp_help_agent_profile &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-agent-profile-transact-sql.md)  
  
  
