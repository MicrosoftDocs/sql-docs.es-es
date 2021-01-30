---
description: MSsnapshot_agents (Transact-SQL)
title: MSsnapshot_agents (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSsnapshot_agents
- MSsnapshot_agents_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSsnapshot_agents system table
ms.assetid: aeae0a2e-4c21-4c45-be65-1e426fa52bdd
author: cawrites
ms.author: chadam
ms.openlocfilehash: ba527028dc1b217022b1f0364b4cb2439d30ef61
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199082"
---
# <a name="mssnapshot_agents-transact-sql"></a>MSsnapshot_agents (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabla **MSsnapshot_agents** contiene una fila por cada agente de instantáneas asociada al distribuidor local. Esta tabla se almacena en la base de datos de distribución.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**id**|**int**|Id. del Agente de instantáneas.|  
|**name**|**nvarchar(100**|Nombre del Agente de instantáneas.|  
|**publisher_id**|**smallint**|IDENTIFICADOR del publicador.|  
|**publisher_db**|**sysname**|Nombre de la base de datos del publicador.|  
|**publicaciones**|**sysname**|Nombre de la publicación.|  
|**publication_type**|**int**|Tipo de publicación:<br /><br /> **0** = transaccional.<br /><br /> **1** = instantánea.<br /><br /> **2** = fusionar mediante combinación.|  
|**local_job**|**bit**|Indica si hay un trabajo de Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  en el distribuidor local.|  
|**job_id**|**binario (16)**|El número de identificación del trabajo.|  
|**profile_id**|**int**|El ID. de configuración de la [MSagent_profiles &#40;tabla de&#41;de Transact-SQL ](../../relational-databases/system-tables/msagent-profiles-transact-sql.md) .|  
|**dynamic_filter_login**|**sysname**|El valor que se usa para evaluar el [SUSER_SNAME &#40;función de&#41;de Transact-SQL ](../../t-sql/functions/suser-sname-transact-sql.md) en los filtros con parámetros que definen una partición. Esta columna se utiliza para una instantánea con particiones.|  
|**dynamic_filter_hostname**|**sysname**|El valor que se usa para evaluar el [HOST_NAME &#40;función de&#41;de Transact-SQL ](../../t-sql/functions/host-name-transact-sql.md) en los filtros con parámetros que definen una partición. Esta columna se utiliza para una instantánea con particiones.|  
|**publisher_security_mode**|**smallint**|El modo de seguridad utilizado por el agente al conectar al publicador. Puede ser uno de los siguientes:<br /><br /> **0**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticación<br /><br /> **1**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] autenticación de Windows.|  
|**publisher_login**|**sysname**|Inicio de sesión utilizado al conectar al publicador.|  
|**publisher_password**|**nvarchar (524)**|Valor cifrado de la contraseña que se utiliza al conectar al publicador.|  
|**job_step_uid**|**uniqueidentifier**|El Id. único del paso de trabajo del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el que se inicia el agente.|  
|**job_login**|**sysname**||  
|**job_password**|**nvarchar (524)**||  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
