---
description: sp_add_agent_parameter (Transact-SQL)
title: sp_add_agent_parameter (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_add_agent_parameter_TSQL
- sp_add_agent_parameter
helpviewer_keywords:
- sp_add_agent_parameter
ms.assetid: 055f4765-0574-47c3-bf7d-6ef6e9bd8b34
author: mashamsft
ms.author: mathoma
ms.openlocfilehash: f0b1879f0dad9d99f24bca5eb5f43a38eba4d2d3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186074"
---
# <a name="sp_add_agent_parameter-transact-sql"></a>sp_add_agent_parameter (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Agrega un nuevo parámetro y su valor al perfil de un agente. Este procedimiento almacenado se ejecuta en el distribuidor de cualquier base de datos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_add_agent_parameter [ @profile_id = ] profile_id  
        , [ @parameter_name = ] 'parameter_name'   
        , [ @parameter_value = ] 'parameter_value'   
```  
  
## <a name="arguments"></a>Argumentos  
`[ @profile_id = ] profile_id` Es el identificador del perfil de la tabla **MSagent_profiles** de la base de datos **msdb** . *profile_id* es de **tipo int** y no tiene ningún valor predeterminado.  
  
 Para averiguar qué tipo de agente representa este *profile_id* , busque el *profile_id* en la [MSagent_profiles &#40;tabla&#41;de Transact-SQL](../../relational-databases/system-tables/msagent-profiles-transact-sql.md) y anote el valor del campo *agent_type* . Éstos son sus valores:  
  
|Value|Descripción|  
|-----------|-----------------|  
|**1**|Agente de instantáneas|  
|**2**|Agente de registro del LOG|  
|**3**|Agente de distribución|  
|**4**|Agente de mezcla|  
|**9**|Agente de lectura de cola|  
  
`[ @parameter_name = ] 'parameter_name'` Es el nombre del parámetro. *parameter_name* es de **tipo sysname** y no tiene ningún valor predeterminado. Para obtener una lista de parámetros ya definidos en los perfiles del sistema, vea [Replication Agent profiles](../../relational-databases/replication/agents/replication-agent-profiles.md). Para obtener una lista completa de parámetros válidos para cada agente, vea los siguientes temas:  
  
-   [Replication Snapshot Agent](../../relational-databases/replication/agents/replication-snapshot-agent.md)  
  
-   [Agente de registro del LOG de replicación](../../relational-databases/replication/agents/replication-log-reader-agent.md)  
  
-   [Replication Distribution Agent](../../relational-databases/replication/agents/replication-distribution-agent.md)  
  
-   [Replication Merge Agent](../../relational-databases/replication/agents/replication-merge-agent.md)  
  
-   [Agente de lectura de cola de replicación](../../relational-databases/replication/agents/replication-queue-reader-agent.md)  
  
`[ @parameter_value = ] 'parameter_value'` Es el valor que se va a asignar al parámetro. *parameter_value* es de tipo **nvarchar (255)** y no tiene ningún valor predeterminado.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_add_agent_parameter** se utiliza en la replicación de instantáneas, la replicación transaccional y la replicación de mezcla.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** pueden ejecutar **sp_add_agent_parameter**.  
  
## <a name="see-also"></a>Consulte también  
 [Trabajar con perfiles del Agente de replicación](../../relational-databases/replication/agents/work-with-replication-agent-profiles.md)   
 [Perfiles del Agente de replicación](../../relational-databases/replication/agents/replication-agent-profiles.md)   
 [sp_add_agent_profile &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-agent-profile-transact-sql.md)   
 [sp_change_agent_profile &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-change-agent-profile-transact-sql.md)   
 [sp_change_agent_parameter &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-change-agent-parameter-transact-sql.md)   
 [sp_drop_agent_parameter &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-drop-agent-parameter-transact-sql.md)   
 [sp_help_agent_parameter &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-agent-parameter-transact-sql.md)  
  
  
