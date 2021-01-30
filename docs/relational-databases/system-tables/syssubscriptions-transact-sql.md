---
description: syssubscriptions (Transact-SQL)
title: syssubscriptions (Transact-SQL) | Microsoft Docs
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
- syssubscriptions system table
ms.assetid: 106c1707-e0e0-49b4-ba50-25380c40fab2
author: cawrites
ms.author: chadam
ms.openlocfilehash: 724627c394fa495eaed2a5d42ea8b16de4f491fd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197601"
---
# <a name="syssubscriptions-transact-sql"></a>syssubscriptions (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una fila por cada suscripción de la base de datos. Esta tabla se almacena en la base de datos de publicación.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**artid**|**int**|Id. único de un artículo.|  
|**srvid**|**smallint**|Id. de servidor del suscriptor.|  
|**dest_db**|**sysname**|El nombre de la base de datos de destino.|  
|**status**|**tinyint**|El estado de la suscripción:<br /><br /> **0** = inactivo.<br /><br /> **1** = suscrito.<br /><br /> **2** = activo.|  
|**sync_type**|**tinyint**|El tipo de sincronización inicial:<br /><br /> **1** = automático.<br /><br /> **2** = ninguno|  
|**login_name**|**sysname**|El nombre de inicio de sesión utilizado al agregar la suscripción.|  
|**subscription_type**|**int**|El tipo de suscripción:<br /><br /> 0 = Inserción. El agente de distribución se ejecuta en el distribuidor.<br /><br /> 1 = Extracción. El agente de distribución se ejecuta en el suscriptor.|  
|**distribution_jobid**|**binario (16)**|Id. de trabajo del Agente de distribución.|  
|**timestamp**|**timestamp**|la marca de tiempo.|  
|**update_mode**|**tinyint**|Modo de actualización:<br /><br /> **0** = solo lectura.<br /><br /> **1** = actualización inmediata.|  
|**loopback_detection**|**bit**|Se aplica a las suscripciones que forman parte de una topología de replicación transaccional bidireccional. La detección de bucles de retorno determina si el Agente de distribución envía las transacciones originadas en el suscriptor al mismo suscriptor:<br /><br /> **0** = devuelve.<br /><br /> **1** = no devuelve.|  
|**queued_reinit**|**bit**|Especifica si el artículo está marcado para inicialización o reinicialización. Un valor de **1** especifica que el artículo suscrito está marcado para inicialización o reinicialización.|  
|**nosync_type**|**tinyint**|Tipo de inicialización de la suscripción:<br /><br /> **0** = automática (instantánea)<br /><br /> **1** = solo compatibilidad con replicación<br /><br /> **2** = inicializar con copia de seguridad<br /><br /> **3** = inicializar desde el número de secuencia de registro (LSN)<br /><br /> Para obtener más información, vea el parámetro **\@ sync_type** de [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md).|  
|**srvname**|**sysname**|Nombre del suscriptor.|  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
