---
description: sys.fn_hadr_distributed_ag_database_replica (Transact-SQL)
title: sys.fn_hadr_distributed_ag_database_replica (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/14/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.fn_hadr_distributed_ag_database_replica
- sys.fn_hadr_distributed_ag_database_replica_TSQL
- fn_hadr_distributed_ag_database_replica
- fn_hadr_distributed_ag_database_replica_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.fn_hadr_distributed_ag_database_replica
ms.assetid: 0e6202a1-e872-4f53-99d7-c16b6f712efc
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: e704af7bcdf5df2a27bcb3a9bd8ae37494f7f6de
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199249"
---
# <a name="sysfn_hadr_distributed_ag_database_replica-transact-sql"></a>sys.fn_hadr_distributed_ag_database_replica (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Se usa para asignar una base de datos de un grupo de disponibilidad distribuido a la base de datos del grupo de disponibilidad local.  
   
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sys.fn_hadr_distributed_ag_database_replica( lag_Id, database_id )  
```  
  
## <a name="arguments"></a>Argumentos  
 '*lag_Id*'  
 Es el identificador del grupo de disponibilidad distribuido. *lag_Id* es de tipo **uniqueidentifier**.  
  
 '*database_id*'  
 Es el identificador de la base de datos en un grupo de disponibilidad distribuido. *database_id* es de tipo **uniqueidentifier**.  
  
## <a name="tables-returned"></a>Tablas devueltas  
 Devuelve la siguiente información.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**group_database_id**|**uniqueidentifier**|IDENTIFICADOR de la base de datos del grupo de disponibilidad local.|  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="using-sysfn_hadr_distributed_ag_database_replica"></a>Usar sys.fn_hadr_distributed_ag_database_replica  
 En el ejemplo siguiente se pasa el identificador de base de datos en un grupo de disponibilidad distribuido. Devuelve una tabla con el identificador de base de datos asociado al grupo de disponibilidad local.  
  
```  
DECLARE @lagId uniqueidentifier = '4A03D1A8-4AE6-B153-E7E9-ED22A546008D'  
DECLARE @databaseId uniqueidentifier = '3FFA882A-C4C3-5B9E-A203-8F44BD9659F7'  
  
SELECT * FROM sys.fn_hadr_distributed_ag_database_replica(@lagId, @databaseId)  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Always On funciones de grupos de disponibilidad &#40;Transact-SQL&#41;](../../relational-databases/system-functions/always-on-availability-groups-functions-transact-sql.md)   
 [Grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [Grupos de disponibilidad distribuidos &#40;Always On grupos de disponibilidad&#41;](../../database-engine/availability-groups/windows/distributed-availability-groups.md)   
 [CREATE AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/create-availability-group-transact-sql.md)   
 [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/alter-availability-group-transact-sql.md)  
  
