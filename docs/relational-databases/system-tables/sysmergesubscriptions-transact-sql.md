---
description: sysmergesubscriptions (Transact-SQL)
title: sysmergesubscriptions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sysmergesubscriptions_TSQL
- sysmergesubscriptions
dev_langs:
- TSQL
helpviewer_keywords:
- sysmergesubscriptions system table
ms.assetid: 6adc78da-991d-4c08-98c3-ecb4762e0e99
author: cawrites
ms.author: chadam
ms.openlocfilehash: 30b7f652f0bb85ab0c2bdf56ebf50090699c476e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184696"
---
# <a name="sysmergesubscriptions-transact-sql"></a>sysmergesubscriptions (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una fila por cada suscriptor conocido y se trata de una tabla local del publicador. Esta tabla se almacena en las bases de datos de publicación y de suscripciones.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|subscriber_server|**sysname**|Identificador del servidor. Se utiliza para asignar el campo srvid al valor específico del servidor cuando se migra una copia de la base de datos de suscripciones a un servidor diferente.|  
|db_name|**sysname**|Nombre de la base de datos de suscripción.|  
|pubid|**uniqueidentifier**|Identificador de la publicación a partir de la que se creó la suscripción actual.|  
|datasource_type|**int**|Tipo del origen de datos:<br /><br /> **0**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .<br /><br /> **2** = OLE DB jet.|  
|subid|**uniqueidentifier**|Número de identificación único para la suscripción.|  
|replnickname|**binary**|Alias comprimido de la réplica.|  
|replicastate|**uniqueidentifier**|Identificador único que se utiliza para determinar si la sincronización anterior se realizó correctamente comparando el valor del publicador con el valor del suscriptor.|  
|status|**tinyint**|El estado de la suscripción:<br /><br /> **0** = inactivo.<br /><br /> **1** = activo.<br /><br /> **2** = eliminado.|  
|subscriber_type|**int**|El tipo de suscriptor:<br /><br /> **1** = global.<br /><br /> **2** = local.<br /><br /> **3** = anónimo.|  
|subscription_type|**int**|El tipo de suscripción:<br /><br /> **0** = extracción.<br /><br /> **1** = extracción.<br /><br /> **2** = anónimo.|  
|sync_type|**tinyint**|Tipo de sincronización:<br /><br /> **1** = automático.<br /><br /> **2** = sin sincronización.|  
|description|**nvarchar(255)**|Descripción breve de la suscripción.|  
|priority|**real**|Especifica la prioridad de la suscripción y permite la implementación de una resolución de conflictos basada en prioridades. Es igual a **0,00** para todas las suscripciones locales o anónimas.|  
|recgen|**bigint**|Número de la última generación recibida.|  
|recguid|**uniqueidentifier**|Identificador único de la última generación recibida.|  
|sentgen|**bigint**|Número de la última generación enviada.|  
|sentguid|**uniqueidentifier**|Identificador único de la última generación enviada.|  
|schemaversion|**int**|Número del último esquema recibido.|  
|schemaguid|**uniqueidentifier**|Identificador único del último esquema recibido.|  
|last_validated|**datetime**|**Fecha y hora** de la última validación correcta de los datos del suscriptor.|  
|attempted_validate|**datetime**|Última **fecha y hora** en la que se intentó realizar la validación en la suscripción.|  
|last_sync_date|**datetime**|**Fecha y hora** de la sincronización.|  
|last_sync_status|**int**|Estado de la suscripción:<br /><br /> **0** = todos los trabajos están a la espera de iniciarse.<br /><br /> **1** = uno o más trabajos se están iniciando.<br /><br /> **2** = todos los trabajos se han ejecutado correctamente.<br /><br /> **3** = al menos un trabajo se está ejecutando.<br /><br /> **4** = todos los trabajos están programados e inactivos.<br /><br /> **5** = al menos un trabajo está intentando ejecutarse después de un error anterior.<br /><br /> **6** = al menos un trabajo no se pudo ejecutar correctamente.|  
|last_sync_summary|**sysname**|Descripción de los resultados de la última sincronización.|  
|metadatacleanuptime|**datetime**|Última **fecha y hora** a la que se quitaron los metadatos expirados de las tablas del sistema de replicación de mezcla.|  
|partition_id|**int**|Identifica la partición precalculada a la que pertenece la suscripción.|  
|cleanedup_unsent_changes|**bit**|Identifica que los metadatos de los cambios no enviados se han eliminado en el suscriptor.|  
|replica_version|**int**|Identifica la versión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] del suscriptor al que pertenece la suscripción, que puede ser uno de estos valores:<br /><br /> **90** = [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]<br /><br /> **100** = [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]|  
|supportability_mode|**int**|Solo para uso interno.|  
|application_name|**nvarchar(128)**|Solo para uso interno.|  
|subscriber_number|**int**|Solo para uso interno.|  
|last_makegeneration_datetime|**datetime**|Última **fecha y hora** que el proceso makegeneration ejecutó para el publicador. Para obtener más información, consulte el parámetro-MakeGenerationInterval en [replication agente de mezcla](../../relational-databases/replication/agents/replication-merge-agent.md).|  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)  
  
  
