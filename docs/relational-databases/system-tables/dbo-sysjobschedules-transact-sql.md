---
description: dbo.sysjobschedules (Transact-SQL)
title: dbo.sysjobschedules (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysjobschedules
- dbo.sysjobschedules
- dbo.sysjobschedules_TSQL
- sysjobschedules_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysjobschedules system table
ms.assetid: ccdafec7-2a9b-4356-bffb-1caa3a12db59
author: cawrites
ms.author: chadam
ms.openlocfilehash: 3880d33e9c69a4911aa64487c34e636ce45a3a05
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096281"
---
# <a name="dbosysjobschedules-transact-sql"></a>dbo.sysjobschedules (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene información de programación de los trabajos que el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe ejecutar. Esta tabla se almacena en la base de datos **msdb** .  
  
> **Nota:** La tabla **sysjobschedules** se actualiza cada 20 minutos, lo que puede afectar a los valores devueltos por la **sp_help_jobschedule** procedimiento almacenado.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**schedule_id**|**int**|Id. de la programación.|  
|**job_id**|**uniqueidentifier**|IDENTIFICADOR del trabajo.|  
|**next_run_date**|**int**|La siguiente fecha programada para que se ejecute el trabajo. La fecha tiene el formato AAAAMMDD.|  
|**next_run_time**|**int**|La hora a la que está programada la ejecución del trabajo. La hora tiene el formato HHMMSS y utiliza un reloj de 24 horas.|  
  
## <a name="see-also"></a>Consulte también  
 [ Programaciones dedbo.sys&#40;Transact-SQL&#41;](../../relational-databases/system-tables/dbo-sysschedules-transact-sql.md)  
  
  
