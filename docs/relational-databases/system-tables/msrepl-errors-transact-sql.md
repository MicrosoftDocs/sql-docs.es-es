---
description: MSrepl_errors (Transact-SQL)
title: MSrepl_errors (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSrepl_errors
- MSrepl_errors_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSrepl_errors system table
ms.assetid: c6e023c1-2c32-4269-8d76-e442ea309e4b
author: cawrites
ms.author: chadam
ms.openlocfilehash: 0899b22bbd4c31676dd5520eaa8d6dfe4a0830a1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194358"
---
# <a name="msrepl_errors-transact-sql"></a>MSrepl_errors (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabla **MSrepl_errors** contiene filas con agente de distribución extendidos e información de errores agente de mezcla. Esta tabla se almacena en la base de datos de distribución.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**id**|**int**|IDENTIFICADOR del error.|  
|**time**|**datetime**|Hora del error.|  
|**error_type_id**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**source_type_id**|**int**|IDENTIFICADOR del tipo de origen del error.|  
|**source_name**|**nvarchar(100**|Nombre del origen del error.|  
|**error_code**|**sysname**|Código de error.|  
|**error_text**|**ntext**|Mensaje de error.|  
|**xact_seqno**|**varbinary(16)**|Número de secuencia de registro de la transacción inicial del lote de ejecución erróneo. Solo lo utilizan los Agentes de distribución; se trata del número de secuencia del registro de transacciones de la primera transacción en el proceso por lotes de ejecución errónea.|  
|**command_id**|**int**|Id. del comando del proceso por lotes de ejecución fallida. Solo lo utilizan los Agentes de distribución, y se trata del Id. del comando del primer comando del proceso por lotes de ejecución fallida.|  
|**session_id**|**int**|Id. de la sesión del agente en la que ocurrió el error.|  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
