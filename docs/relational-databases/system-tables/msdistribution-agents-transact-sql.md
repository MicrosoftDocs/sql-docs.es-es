---
description: MSdistribution_agents (Transact-SQL)
title: MSdistribution_agents (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/28/2015
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSdistribution_agents_TSQL
- MSdistribution_agents
dev_langs:
- TSQL
helpviewer_keywords:
- MSdistribution_agents system table
ms.assetid: 0e8f0653-1351-41d1-95d2-40f6d5a050ca
author: cawrites
ms.author: chadam
ms.openlocfilehash: a74b3cd9a569f782112ee1feee6400b01c32f00b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160417"
---
# <a name="msdistribution_agents-transact-sql"></a>MSdistribution_agents (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabla **MSdistribution_agents** contiene una fila por cada agente de distribución que se ejecuta en el distribuidor local. Esta tabla se almacena en la base de datos de distribución.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**id**|**int**|Id. del Agente de distribución.|  
|**name**|**nvarchar(100**|Nombre del Agente de distribución.|  
|**publisher_database_id**|**int**|El Id. de la base de datos del publicador.|  
|**publisher_id**|**smallint**|IDENTIFICADOR del publicador.|  
|**publisher_db**|**sysname**|Nombre de la base de datos del publicador.|  
|**publicaciones**|**sysname**|Nombre de la publicación.|  
|**subscriber_id**|**smallint**|Id. del suscriptor, que solo utilizan los agentes conocidos. En el caso de los agentes anónimos, esta columna está reservada.|  
|**subscriber_db**|**sysname**|El nombre de la base de datos de suscripciones.|  
|**subscription_type**|**int**|El tipo de suscripción:<br /><br /> **0** = extracción.<br /><br /> **1** = extracción.<br /><br /> **2** = anónimo.|  
|**local_job**|**bit**|Indica si hay un trabajo del Agente SQL Server en el distribuidor local.|  
|**job_id**|**binario (16)**|El número de identificación del trabajo.|  
|**subscription_guid**|**binario (16)**|Id. de las suscripciones de este agente.|  
|**profile_id**|**int**|El ID. de configuración de la [MSagent_profiles &#40;tabla de&#41;de Transact-SQL ](../../relational-databases/system-tables/msagent-profiles-transact-sql.md) .|  
|**anonymous_subid**|**uniqueidentifier**|Id. de un agente anónimo.|  
|**subscriber_name**|**sysname**|Nombre del suscriptor, que solo utilizan los agentes anónimos.|  
|**virtual_agent_id**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**anonymous_agent_id**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**creation_date**|**datetime**|Fecha y hora de creación del Agente de distribución o de mezcla.|  
|**queue_id**|**sysname**|Identificador que se utiliza para localizar la cola de suscripciones de actualización en cola. Para las suscripciones que no sean en cola, el valor es NULL. Para las publicaciones basadas en [!INCLUDE[msCoName](../../includes/msconame-md.md)] Message Queue Server, el valor es un GUID que identifica de forma única la cola que se va a utilizar en la suscripción. En el caso de las publicaciones de cola basadas en SQL Server, la columna contiene el valor **SQL**.<br /><br /> Nota: el uso [!INCLUDE[msCoName](../../includes/msconame-md.md)] de Message Queue Server ha quedado desusado y ya no se admite.|  
|**queue_status**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**offload_enabled**|**bit**|Indica si el agente puede activarse de manera remota.<br /><br /> **0** especifica que no se puede activar el agente de forma remota.<br /><br /> **1** especifica que el agente se activará de forma remota y en el equipo remoto especificado en la propiedad *offload_server* .|  
|**offload_server**|**sysname**|Nombre de red del servidor que se va a utilizar en la activación remota del agente.|  
|**dts_package_name**|**sysname**|Nombre del paquete DTS. Por ejemplo, para un paquete denominado **DTSPub_Package**, especifique `@dts_package_name = N'DTSPub_Package'` .|  
|**dts_package_password**|**nvarchar (524)**|Contraseña del paquete.|  
|**dts_package_location**|**int**|La ubicación del paquete. La ubicación del paquete puede ser **Distributor** o **Subscriber**.|  
|**Junction**|**varbinary(85)**|Número de identificación de seguridad (SID) del Agente de distribución o del Agente de mezcla durante su primera ejecución.|  
|**queue_server**|**sysname**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**subscriber_security_mode**|**smallint**|Modo de seguridad utilizado por el agente cuando se conecta al suscriptor, que puede ser uno de los siguientes:<br /><br /> **0**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] SQL Server autenticación de<br /><br /> **1**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] autenticación de Windows.|  
|**subscriber_login**|**sysname**|Inicio de sesión que se utilizará en la conexión con el suscriptor.|  
|**subscriber_password**|**nvarchar (524)**|Es el valor cifrado de la contraseña utilizada al conectarse al suscriptor.|  
|**reset_partial_snapshot_progress**|**bit**|Indica si se descartará una instantánea descargada parcialmente para que todo el proceso de instantánea pueda empezar de nuevo.|  
|**job_step_uid**|**uniqueidentifier**|Id. exclusivo del paso de trabajo del Agente SQL Server en el que se ha iniciado el agente.|  
|**subscriptionstreams**|**tinyint**|Establece el número de conexiones permitidas por Agente de distribución para aplicar lotes de cambios en paralelo en un suscriptor. Se admite un intervalo de valores de 1 a 64.|  
|**memory_optimized**|**bit**|1 indica que el suscriptor se puede usar para las tablas optimizadas para memoria.|  
|**job_login**|**sysname**||  
|**job_password**|**nvarchar (524)**||  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)  
  
  
