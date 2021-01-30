---
description: dbo.sysjobsteps (Transact-SQL)
title: dbo.sysjobsteps (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dbo.sysjobsteps
- dbo.sysjobsteps_TSQL
- sysjobsteps
- sysjobsteps_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysjobsteps system table
ms.assetid: 978b8205-535b-461c-91f3-af9b08eca467
author: cawrites
ms.author: chadam
ms.openlocfilehash: d4e06b76f069086a6223ba02969c15e5cc1c7908
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202318"
---
# <a name="dbosysjobsteps-transact-sql"></a>dbo.sysjobsteps (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene la información de cada paso de un trabajo que debe ejecutar el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Esta tabla se almacena en la base de datos **msdb** .  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**job_id**|**uniqueidentifier**|IDENTIFICADOR del trabajo.|  
|**step_id**|**int**|Id. del paso en el trabajo.|  
|**step_name**|**sysname**|Nombre del paso del trabajo.|  
|**subsistema**|**nvarchar(40)**|Nombre del subsistema que utiliza el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para ejecutar el paso del trabajo.|  
|**command**|**nvarchar(max)**|Comando que va a ejecutar el **subsistema**.|  
|**flags**|**int**|Reservado.|  
|**additional_parameters**|**ntext**|Reservado.|  
|**cmdexec_success_code**|**int**|Valor de nivel de error devuelto por los pasos del subsistema **CmdExec** para indicar que la operación se ha realizado correctamente.|  
|**on_success_action**|**tinyint**|Acción que se debe realizar cuando un paso se ejecuta correctamente.<br /><br /> **1** = (valor predeterminado) salir con correcto<br /><br /> **2** = salir con error<br /><br /> **3** = ir al paso siguiente<br /><br /> **4** = ir al paso _on_success_step_id_|
|**on_success_step_id**|**int**|Id. del siguiente paso que se debe ejecutar cuando un paso se ejecuta correctamente.|  
|**on_fail_action**|**tinyint**|Acción que se debe realizar cuando un paso no se ejecuta correctamente.<br /><br /> **1** = salir con éxito<br /><br /> **2** = (valor predeterminado) salir con error<br /><br /> **3** = ir al paso siguiente<br /><br /> **4** = ir al paso _on_fail_step_id_|
|**on_fail_step_id**|**int**|Id. del siguiente paso que se debe ejecutar cuando un paso no se ejecuta correctamente.|  
|**servidor**|**sysname**|Reservado.|  
|**database_name**|**sysname**|Nombre de la base de datos en la que se ejecuta el **comando** si el **subsistema** es TSQL.|  
|**database_user_name**|**sysname**|Nombre del usuario de base de datos cuya cuenta se utiliza al ejecutar el paso.|  
|**retry_attempts**|**int**|Número de reintentos si el paso no se ejecuta correctamente.|  
|**retry_interval**|**int**|Tiempo que se debe esperar entre cada intento.|  
|**os_run_priority**|**int**|Reservado.|  
|**output_file_name**|**nvarchar(200)**|Nombre del archivo en el que se guarda la salida del paso cuando **Subsystem** es TSQL, PowerShell o **CmdExec**_._|  
|**last_run_outcome**|**int**|Resultado de la ejecución anterior del paso del trabajo.<br /><br /> **0** = error<br /><br /> **1** = correcto<br /><br /> **2** = reintento<br /><br /> **3** = cancelado<br /><br /> **5** = desconocido|  
|**last_run_duration**|**int**|Duración (hhmmss) del paso la última vez que se ejecutó.|  
|**last_run_retries**|**int**|Número de reintentos en la última ejecución del paso del trabajo.|  
|**last_run_date**|**int**|Fecha (aaaammdd) en que se inició la ejecución del paso por última vez.|  
|**last_run_time**|**int**|Hora (hhmmss) en que se inició la ejecución del paso por última vez.|  
|**proxy_id**|**int**|Proxy del paso de trabajo.|  
|**step_uid**|**uniqueidentifier**|Identificador del paso de trabajo.|  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de Agente SQL Server &#40;&#41;de Transact-SQL ](../../relational-databases/system-tables/sql-server-agent-tables-transact-sql.md)  
  
  
