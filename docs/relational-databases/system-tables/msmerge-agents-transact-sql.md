---
description: MSmerge_agents (Transact-SQL)
title: MSmerge_agents (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSmerge_agents
- MSmerge_agents_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_agents system table
ms.assetid: 639d2ebb-2c37-4fe0-b14b-1637bc5fc221
author: cawrites
ms.author: chadam
ms.openlocfilehash: 1be54b9324dd32de683d442d65472360cf247e71
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212285"
---
# <a name="msmerge_agents-transact-sql"></a>MSmerge_agents (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabla **MSmerge_agents** contiene una fila por cada agente de mezcla que se ejecuta en el suscriptor. Esta tabla se almacena en la base de datos de distribución.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**id**|**int**|El Id. del Agente de mezcla.|  
|**name**|**nvarchar(100**|Nombre del Agente de mezcla.|  
|**publisher_id**|**smallint**|IDENTIFICADOR del publicador.|  
|**publisher_db**|**sysname**|Nombre de la base de datos del publicador.|  
|**publicaciones**|**sysname**|Nombre de la publicación.|  
|**subscriber_id**|**smallint**|IDENTIFICADOR del suscriptor.|  
|**subscriber_db**|**sysname**|El nombre de la base de datos de suscripciones.|  
|**local_job**|**bit**|Indica si hay un trabajo de Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  en el distribuidor local.|  
|**job_id**|**binario (16)**|El número de identificación del trabajo.|  
|**profile_id**|**int**|IDENTIFICADOR de configuración de la tabla **MSagent_profiles** .|  
|**anonymous_subid**|**uniqueidentifier**|Id. de un agente anónimo.|  
|**subscriber_name**|**sysname**|Nombre del suscriptor.|  
|**creation_date**|**datetime**|Fecha y hora en que se creó la distribución o el Agente de mezcla.|  
|**offload_enabled**|**bit**|Especifica que el agente puede activarse de manera remota.<br /><br /> **0** especifica que no se puede activar el agente de forma remota.<br /><br /> **1** especifica que el agente se activará de forma remota y en el equipo remoto especificado en la propiedad offload_server.|  
|**offload_server**|**sysname**|Especifica el nombre de red del servidor a utilizar en la activación remota del agente.|  
|**Junction**|**varbinary(85)**|Número de identificación de seguridad (SID) del Agente de distribución o del Agente de mezcla durante su primera ejecución.|  
|**subscriber_security_mode**|**smallint**|Modo de seguridad utilizado por el agente cuando se conecta al suscriptor, que puede ser uno de los siguientes:<br /><br /> **0**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticación.<br /><br /> **1**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] autenticación de Windows.|  
|**subscriber_login**|**sysname**|Inicio de sesión que se utilizará en la conexión con el suscriptor.|  
|**subscriber_password**|**nvarchar (524)**|Valor cifrado de la contraseña que se utilizará en la conexión con el suscriptor.|  
|**publisher_security_mode**|**smallint**|El modo de seguridad utilizado por el agente al conectar al publicador. Puede ser uno de los siguientes:<br /><br /> **0**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticación.<br /><br /> **1**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] autenticación de Windows.|  
|**publisher_login**|**sysname**|Inicio de sesión utilizado al conectar al publicador.|  
|**publisher_password**|**nvarchar (524)**|Valor cifrado de la contraseña que se utiliza al conectar al publicador.|  
|**job_step_uid**|**uniqueidentifier**|El Id. único del paso de trabajo del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el que se inicia el agente.|  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
