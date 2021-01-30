---
description: sp_enum_sqlagent_subsystems (Transact-SQL)
title: sp_enum_sqlagent_subsystems (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_enum_sqlagent_subsystems
- sp_enum_sqlagent_subsystems_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_enum_sqlagent_subsystems
ms.assetid: 019a3c9d-bac3-495b-a70a-2c19f1d2e20e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 3a65369cb7c4769d930acdaf655a006c60bdd55f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186953"
---
# <a name="sp_enum_sqlagent_subsystems-transact-sql"></a>sp_enum_sqlagent_subsystems (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Muestra una lista de los subsistemas del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_enum_sqlagent_subsystems  
```  
  
## <a name="arguments"></a>Argumentos  
 Ninguno  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**subsistema**|**nvarchar(40)**|Nombre del subsistema.|  
|**description**|**nvarchar(512)**|Descripción del subsistema.|  
|**subsystem_dll**|**nvarchar (510)**|Módulo DLL que contiene el subsistema.|  
|**agent_exe**|**nvarchar (510)**|Módulo ejecutable utilizado por el subsistema.|  
|**start_entry_point**|**nvarchar(30)**|Procedimiento al que el Agente SQL Server llama durante la ejecución de pasos de trabajo.|  
|**event_entry_point**|**nvarchar(30)**|Procedimiento al que el Agente SQL Server llama durante la ejecución de pasos de trabajo.|  
|**stop_entry_point**|**nvarchar(30)**|Procedimiento al que el Agente SQL Server llama durante la ejecución de pasos de trabajo.|  
|**max_worker_threads**|**int**|Número máximo de subprocesos que el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] iniciará para este subsistema.|  
|**subsystem_id**|**int**|Identificador del subsistema.|  
  
## <a name="remarks"></a>Observaciones  
 Este procedimiento enumera los subsistemas disponibles en la instancia.  
  
## <a name="permissions"></a>Permisos  
 De forma predeterminada, los miembros del rol fijo de servidor **sysadmin** pueden ejecutar este procedimiento almacenado. A otros usuarios debe concederse el rol fijo de base de datos **SQLAgentOperatorRole** en la base de datos **msdb** .  
  
 Para obtener más información sobre **SQLAgentOperatorRole**, vea [Agente SQL Server roles fijos de base de datos](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
## <a name="see-also"></a>Consulte también  
 [Implementar la seguridad de Agente SQL Server](../../ssms/agent/implement-sql-server-agent-security.md)   
 [sp_add_jobstep &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md)  
  
  
