---
description: sys.fn_hadr_distributed_ag_replica (Transact-SQL)
title: sys.fn_hadr_distributed_ag_replica (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.fn_hadr_distributed_ag_replica
- sys.fn_hadr_distributed_ag_replica_TSQL
- fn_hadr_distributed_ag_replica
- fn_hadr_distributed_ag_replica_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.fn_hadr_distributed_ag_replica
ms.assetid: a1e5f9cb-c350-4bb4-a04f-7394f6f25d62
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 8d0b1e6df3a2286cfe60dbaf89358683766dc4c3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198733"
---
# <a name="sysfn_hadr_distributed_ag_replica-transact-sql"></a>sys.fn_hadr_distributed_ag_replica (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Se usa para asignar una réplica de un grupo de disponibilidad distribuido al grupo de disponibilidad local.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sys.fn_hadr_distributed_ag_replica( lag_Id, replica_id )  
```  
  
## <a name="arguments"></a>Argumentos  
 '*lag_Id*'  
 Es el identificador del grupo de disponibilidad distribuido. *lag_Id* es de tipo **uniqueidentifier**.  
  
 '*replica_id*'  
 Es el identificador de una réplica en el grupo de disponibilidad distribuido. *replica_id* es de tipo **uniqueidentifier**.  
  
## <a name="tables-returned"></a>Tablas devueltas  
 Devuelve la siguiente información.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**group_id**|**uniqueidentifier**|Identificador único (GUID) del grupo de disponibilidad local.|  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="using-sysfn_hadr_distributed_ag_replica"></a>Usar sys.fn_hadr_distributed_ag_replica  
 En el ejemplo siguiente se devuelve una tabla con el identificador de grupo de disponibilidad local que está asociado a la réplica y al grupo de disponibilidad distribuida especificado.  
  
```  
DECLARE @lagId uniqueidentifier = '4A03D1A8-4AE6-B153-E7E9-ED22A546008D'  
DECLARE @replicaId uniqueidentifier = 'D5517513-04A8-FD82-14C6-E684EC913935'  
  
SELECT * FROM sys.fn_hadr_distributed_ag_replica(@lagId, @replicaId)  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Funciones de Grupos de disponibilidad AlwaysOn &#40;&#41;de Transact-SQL ](../../relational-databases/system-functions/always-on-availability-groups-functions-transact-sql.md)   
 [Grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [Grupos de disponibilidad distribuidos &#40;Grupos de disponibilidad AlwaysOn&#41;](../../database-engine/availability-groups/windows/distributed-availability-groups.md)  
 [CREATE AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/create-availability-group-transact-sql.md)   
 [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/alter-availability-group-transact-sql.md)  
  
