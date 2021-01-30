---
description: MSagent_profiles (Transact-SQL)
title: MSagent_profiles (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSagent_profiles
- MSagent_profiles_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSagent_profiles system table
ms.assetid: 4ab1b2ae-b6d9-42b7-9b31-98547dbb7f99
author: cawrites
ms.author: chadam
ms.openlocfilehash: 1ac49f809e023dfe08d289f746197f23d181c0a1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160513"
---
# <a name="msagent_profiles-transact-sql"></a>MSagent_profiles (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabla **MSagent_profiles** contiene una fila por cada perfil de agente de replicación definido. Esta tabla se almacena en la base de datos **msdb** .  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**profile_id**|**int**|Id. de perfil.|  
|**profile_name**|**sysname**|Nombre de perfil único para el tipo de agente.|  
|**agent_type**|**int**|El tipo de agente:<br /><br /> **1** = agente de instantáneas<br /><br /> **2** = agente de registro del log<br /><br /> **3** = agente de distribución<br /><br /> **4** = agente de mezcla<br /><br /> **9** = agente de lectura de cola|  
|**type**|**int**|Tipo de perfil:<br /><br /> **0** = sistema **1** = personalizado|  
|**description**|**nvarchar (3000)**|La descripción del perfil.|  
|**def_profile**|**bit**|Especifica si este perfil es el predeterminado para este tipo de agente.|  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
