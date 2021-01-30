---
description: dbo.sysjobhistory (Transact-SQL)
title: dbo.sysjobhistory (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/24/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dbo.sysjobhistory_TSQL
- dbo.sysjobhistory
- sysjobhistory
- sysjobhistory_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysjobhistory system table
ms.assetid: 1b1fcdbb-2af2-45e6-bf3f-e8279432ce13
author: cawrites
ms.author: chadam
ms.openlocfilehash: 9cbb5238e8cb41d892f336413cf7773281476f35
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186313"
---
# <a name="dbosysjobhistory-transact-sql"></a>dbo.sysjobhistory (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Contiene información acerca de la ejecución de los trabajos programados por el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
  
> [!NOTE]
> En la mayoría de los casos, los datos se actualizan solo después de completarse el paso de trabajo y la tabla *normalmente no contiene* ningún registro para los pasos de trabajo que están actualmente en curso, pero en algunos casos los procesos subyacentes proporcionan información acerca de los pasos de trabajo en curso.

Esta tabla se almacena en la base de datos **msdb** .  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**instance_id**|**int**|Identificador único de la fila.|  
|**job_id**|**uniqueidentifier**|Id. del trabajo.|  
|**step_id**|**int**|Id. del paso en el trabajo.|  
|**step_name**|**sysname**|Nombre del paso.|  
|**sql_message_id**|**int**|Id. de los mensajes de error de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devueltos si el trabajo genera algún error.|  
|**sql_severity**|**int**|Gravedad de cualquier error de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**message**|**nvarchar(4000)**|Texto de un error de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], si existe.|  
|**run_status**|**int**|Estado de la ejecución del trabajo:<br /><br /> **0** = error<br /><br /> **1** = correcto<br /><br /> **2** = reintento<br /><br /> **3** = cancelado<br /><br />**4** = en curso|  
|**run_date**|**int**|Fecha en que se empezó a ejecutar el trabajo o paso. En el caso de un historial En curso, es la fecha y hora en que fue escrito.|  
|**run_time**|**int**|Hora en que se inició el trabajo o el paso en formato **HHMMSS** .|  
|**run_duration**|**int**|Tiempo transcurrido en la ejecución del trabajo o paso en formato **HHMMSS** .|  
|**operator_id_emailed**|**int**|Id. del operador al que se notificó la terminación del trabajo.|  
|**operator_id_netsent**|**int**|Id. del operador al que se notificó con un mensaje la terminación del trabajo.|  
|**operator_id_paged**|**int**|Id. del operador al que se notificó por buscapersonas la terminación del trabajo.|  
|**retries_attempted**|**int**|Número de intentos de ejecución del trabajo o paso.|  
|**servidor**|**sysname**|Nombre del servidor en el que se ejecutó el trabajo.|  
  
  ## <a name="example"></a>Ejemplo
 La siguiente [!INCLUDE[tsql](../../includes/tsql-md.md)] consulta convertirá las columnas **run_time** y **run_duration** en un formato más descriptivo.  Ejecute el script en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .
 
 ```sql
 SET NOCOUNT ON;
 
 SELECT sj.name,
        sh.run_date,
        sh.step_name,
        STUFF(STUFF(RIGHT(REPLICATE('0', 6) +  CAST(sh.run_time as varchar(6)), 6), 3, 0, ':'), 6, 0, ':') 'run_time',
        STUFF(STUFF(STUFF(RIGHT(REPLICATE('0', 8) + CAST(sh.run_duration as varchar(8)), 8), 3, 0, ':'), 6, 0, ':'), 9, 0, ':') 'run_duration (DD:HH:MM:SS)  '
FROM msdb.dbo.sysjobs sj
JOIN msdb.dbo.sysjobhistory sh
ON sj.job_id = sh.job_id
```
