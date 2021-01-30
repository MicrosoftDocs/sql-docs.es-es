---
description: syssubscriptions (vista del sistema de Transact-SQL)
title: syssubscriptions (vista del sistema) (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- syssubscriptions_TSQL
- syssubscriptions
dev_langs:
- TSQL
helpviewer_keywords:
- syssubscriptions view
ms.assetid: c9613858-9512-43a9-aa53-7ee8064f064c
author: stevestein
ms.author: sstein
ms.openlocfilehash: 052d7d4c3fa302fbb5962f89f69f1446bd6e7295
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181545"
---
# <a name="syssubscriptions-system-view-transact-sql"></a>syssubscriptions (vista del sistema de Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La vista **syssubscriptions** expone información de suscripción. Esta vista se almacena en la base de datos de distribución.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**artid**|**int**|Id. único de un artículo suscrito.|  
|**srvid**|**smallint**|Id. de servidor del suscriptor.|  
|**dest_db**|**sysname**|El nombre de la base de datos de suscripciones.|  
|**status**|**tinyint**|El estado de la suscripción:<br /><br /> **0** = inactivo.<br /><br /> **1** = suscrito.<br /><br /> **2** = activo.|  
|**sync_type**|**tinyint**|El tipo de sincronización inicial:<br /><br /> **1** = automático.<br /><br /> **2** = ninguno.|  
|**login_name**|**sysname**|El nombre de inicio de sesión utilizado al conectarse al publicador para agregar la suscripción.|  
|**subscription_type**|**int**|El tipo de suscripción:<br /><br /> **0** = inserciones: el agente de distribución se ejecuta en el distribuidor.<br /><br /> **1** = extracción: el agente de distribución se ejecuta en el suscriptor.|  
|**distribution_jobid**|**binario (16)**|Identifica el trabajo del Agente de distribución utilizado para sincronizar la suscripción.|  
|**timestmap**|**timestamp**|Fecha y la hora de creación de la suscripción.|  
|**update_mode**|**tinyint**|Modo de actualización:<br /><br /> **0** = solo lectura.<br /><br /> **1** = actualización inmediata.|  
|**loopback_detection**|**bit**|Se aplica a las suscripciones que forman parte de una topología de replicación transaccional bidireccional. La detección de bucles de retorno determina si el Agente de distribución envía las transacciones originadas en el suscriptor al mismo suscriptor:<br /><br /> **0** = devuelve.<br /><br /> **1** = no devuelve.|  
|**queued_reinit**|**bit**|Especifica si el artículo está marcado para inicialización o reinicialización. Un valor de **1** especifica que el artículo suscrito está marcado para inicialización o reinicialización.|  
|**nosync_type**|**tinyint**|Tipo de inicialización de la suscripción:<br /><br /> **0** = automática (instantánea)<br /><br /> **1** = solo compatibilidad con replicación<br /><br /> **2** = inicializar con copia de seguridad<br /><br /> **3** = inicializar desde el número de secuencia de registro (LSN)<br /><br /> Para obtener más información, vea el parámetro **\@ sync_type** de [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md).<br /><br /> **3** = [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**srvname**|**sysname**|Nombre del suscriptor.|  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [syssubscriptions &#40;Transact-SQL&#41;](../../relational-databases/system-tables/syssubscriptions-transact-sql.md)  
  
  
