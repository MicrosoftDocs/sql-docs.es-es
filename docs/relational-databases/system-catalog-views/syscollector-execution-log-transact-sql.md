---
description: syscollector_execution_log (Transact-SQL)
title: syscollector_execution_log (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- syscollector_execution_log_TSQL
- syscollector_execution_log
dev_langs:
- TSQL
helpviewer_keywords:
- data collector view
- syscollector_execution_log view
ms.assetid: 11554d64-0426-42ce-b7ce-5591f67864d2
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 1920cc669307230bd29acabd04e9ade88147261b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199737"
---
# <a name="syscollector_execution_log-transact-sql"></a>syscollector_execution_log (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Proporciona información acerca del registro de ejecución para un paquete o conjunto de recopilación.   
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|log_id|**bigint**|Identifica cada ejecución del conjunto de recopilación. Se usa para combinar esta vista con otros registros detallados. No admite valores NULL.|  
|parent_log_id|**bigint**|Identifica el conjunto de recopilación o paquete primario. No admite valores NULL. Los identificadores están encadenados en la relación primaria-secundaria, lo que permite determinar qué paquete se inició y con qué conjunto de recopilación. Esta vista agrupa las entradas de registro por su vinculación primario-secundario y aplica sangría a los nombres de los paquetes para que la cadena de llamada esté claramente visible.|  
|collection_set_id|**int**|Identifica el paquete o conjunto de recopilación que representa esta entrada de registro. No admite valores NULL.|  
|collection_item_id|**int**|Identifica un elemento de recopilación. Acepta valores NULL.|  
|start_time|**datetime**|La hora de inicio del paquete o conjunto de recopilación. No admite valores NULL.|  
|last_iteration_time|**datetime**|Para los paquetes que se ejecutan de manera continua, la última vez que el paquete capturó una instantánea. Acepta valores NULL.|  
|finish_time|**datetime**|La hora de finalización de la ejecución de los conjuntos de recopilación y paquetes completados. Acepta valores NULL.|  
|runtime_execution_mode|**smallint**|Indica si la actividad del conjunto de recopilación era recopilación de datos o carga de datos. Acepta valores NULL.<br /><br /> Los valores son:<br /><br /> 0 = Recopilación<br /><br /> 1 = Carga|  
|status|**smallint**|Indica el estado actual del paquete o conjunto de recopilación. No admite valores NULL.<br /><br /> Los valores son:<br /><br /> 0 = en ejecución<br /><br /> 1 = finalizado<br /><br /> 2 = error|  
|operator|**nvarchar(128)**|Identifica quién inició el paquete o conjunto de recopilación. No admite valores NULL.|  
|package_id|**uniqueidentifier**|Identifica el paquete o conjunto de recopilación que generó este registro. Acepta valores NULL.|  
|package_name|**nvarchar(4000)**|Nombre del paquete que generó este registro. Acepta valores NULL.|  
|package_execution_id|**uniqueidentifier**|Proporciona un vínculo a la tabla de registros [!INCLUDE[ssIS](../../includes/ssis-md.md)]. Acepta valores NULL.|  
|failure_message|**nvarchar(2048)**|En caso de error del paquete o conjunto de recopilación, el mensaje de error más reciente para ese componente. Acepta valores NULL. Para obtener información más detallada sobre el error, use el [fn_syscollector_get_execution_details &#40;función de&#41;de Transact-SQL ](../../relational-databases/system-functions/fn-syscollector-get-execution-details-transact-sql.md) .|  
  
## <a name="permissions"></a>Permisos  
 Requiere SELECT para dc_operator.  
  
## <a name="see-also"></a>Vea también  
 [Procedimientos almacenados del recopilador de datos &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)   
 [Vistas del recopilador de datos &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/data-collector-views-transact-sql.md)   
 [Recopilación de datos](../../relational-databases/data-collection/data-collection.md)  
  
  
