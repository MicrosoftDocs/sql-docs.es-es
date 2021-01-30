---
description: MSsubscriber_schedule (Transact-SQL)
title: MSsubscriber_schedule (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSsubscriber_schedule
- MSsubscriber_schedule_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSsubscriber_schedule system table
ms.assetid: ff428306-0ef4-49a3-b536-07ccdf6e2196
author: cawrites
ms.author: chadam
ms.openlocfilehash: 13acf08fb86206c6127e62db465217920d22a927
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211606"
---
# <a name="mssubscriber_schedule-transact-sql"></a>MSsubscriber_schedule (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabla **MSsubscriber_schedule** contiene programaciones de sincronización de transacciones y de mezcla predeterminadas para cada par de publicador y suscriptor. Esta tabla se almacena en la base de datos de distribución.  
  
> [!NOTE]
>  Esta tabla del sistema ha quedado desusada y se mantiene para admitir versiones anteriores de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**publisher**|**sysname**|El nombre del publicador.|  
|**suscriptor**|**sysname**|Nombre del suscriptor.|  
|**agent_type**|**smallint**|El tipo de agente:<br /><br /> 0 = Agente de distribución.<br /><br /> 1 = Agente de mezcla.|  
|**frequency_type**|**int**|Frecuencia con que se programa el Agente de distribución:<br /><br /> **1** = una vez.<br /><br /> **2** = a petición.<br /><br /> **4** = diariamente.<br /><br /> **8** = semanalmente.<br /><br /> **16** = mensualmente.<br /><br /> **32** = mensualmente relativo.<br /><br /> **64** = AutoStart.<br /><br /> **128** = recurrente.|  
|**frequency_interval**|**int**|Valor que se va a aplicar a la frecuencia establecida por **frequency_type**.|  
|**frequency_relative_interval**|**int**|Fecha del Agente de distribución:<br /><br /> **1** = primero.<br /><br /> **2** = segundo.<br /><br /> **4** = tercero.<br /><br /> **8** = cuarto.<br /><br /> **16** = último.|  
|**frequency_recurrence_factor**|**int**|El factor de periodicidad utilizado por **frequency_type**.|  
|**frequency_subday**|**int**|Frecuencia de repetición de la programación durante el periodo definido:<br /><br /> **1** = una vez.<br /><br /> **2** = segundo.<br /><br /> **4** = minuto.<br /><br /> **8** = hora.|  
|**frequency_subday_interval**|**int**|El intervalo de **frequency_subday**.|  
|**active_start_time_of_day**|**int**|Hora del día en que el Agente de distribución se programa por primera vez, con formato HHMMSS.|  
|**active_end_time_of_day**|**int**|Hora del día en que el Agente de distribución deja de estar programado, con formato HHMMSS.|  
|**active_start_date**|**int**|Fecha en que se programa por primera vez el Agente de distribución, con formato AAAAMMDD.|  
|**active_end_date**|**int**|Fecha en que se deja de programar el Agente de distribución, con formato AAAAMMDD.|  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
