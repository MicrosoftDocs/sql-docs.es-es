---
description: sp_helpdynamicsnapshot_job (Transact-SQL)
title: sp_helpdynamicsnapshot_job (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpdynamicsnapshot_TSQL
- sp_helpdynamicsnapshot_job_TSQL
- job_TSQL
- helpdynamicsnapshot
- job
- sp_helpdynamicsnapshot
- sp_helpdynamicsnapshot_job
- helpdynamicsnapshot_TSQL
helpviewer_keywords:
- sp_helpdynamicsnapshot_job
ms.assetid: d6dfdf26-f874-495f-a8a6-8780699646d7
author: markingmyname
ms.author: maghan
ms.openlocfilehash: faf8fcfe5636dc588b111701c5b2cf0668feed78
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204435"
---
# <a name="sp_helpdynamicsnapshot_job-transact-sql"></a>sp_helpdynamicsnapshot_job (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Devuelve información sobre trabajos del agente que generan instantáneas de datos filtrados. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_helpdynamicsnapshot_job [ [ @publication = ] 'publication' ]   
    [ , [ @dynamic_snapshot_jobname = ] 'dynamic_snapshot_jobname' ]  
    [ , [ @dynamic_snapshot_jobid = ] 'dynamic_snapshot_jobid' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publication = ] 'publication'` Es el nombre de la publicación. *Publication* es de **tipo sysname y su** valor predeterminado es **%** , que devuelve información sobre todos los trabajos de instantáneas de datos filtrados que coinciden con el *dynamic_snapshot_jobid* especificado y *dynamic_snapshot_jobname* para todas las publicaciones.  
  
`[ @dynamic_snapshot_jobname = ] 'dynamic_snapshot_jobname'` Es el nombre de un trabajo de instantánea de datos filtrados. *dynamic_snapshot_jobname* es de **tipo sysname y su** valor predeterminado es **%** ', que devuelve todos los trabajos dinámicos de una publicación con el *dynamic_snapshot_jobid* especificado. Si no se especificó un nombre de trabajo explícitamente cuando se creó el trabajo, el formato del nombre del trabajo es el siguiente:  
  
```  
'dyn_' + <name of the standard snapshot job> + <GUID>  
```  
  
`[ @dynamic_snapshot_jobid = ] 'dynamic_snapshot_jobid'` Es un identificador para un trabajo de instantánea de datos filtrados. *dynamic_snapshot_jobid* es de tipo **uniqueidentifier** y su valor predeterminado es null, que devuelve todos los trabajos de instantáneas que coinciden con el *dynamic_snapshot_jobname* especificado.  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**id**|**int**|Identifica el trabajo de instantáneas de datos filtrados.|  
|**job_name**|**sysname**|Nombre del trabajo de instantáneas de datos filtrados.|  
|**job_id**|**uniqueidentifier**|Identifica el [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] trabajo del agente en el distribuidor.|  
|**dynamic_filter_login**|**sysname**|Valor utilizado para evaluar la función de [SUSER_SNAME](../../t-sql/functions/suser-sname-transact-sql.md) en un filtro de fila con parámetros definido para la publicación.|  
|**dynamic_filter_hostname**|**sysname**|Valor utilizado para evaluar la función de [host_name](../../t-sql/functions/host-name-transact-sql.md) en un filtro de fila con parámetros definido para la publicación.|  
|**dynamic_snapshot_location**|**nvarchar(255)**|Ruta a la carpeta de la que se leen los archivos de instantáneas si se utiliza un filtro de fila con parámetros.|  
|**frequency_type**|**int**|Se trata de la frecuencia de ejecución programada del agente, que puede ser uno de estos valores.<br /><br /> **1** = una vez<br /><br /> **2** = a petición<br /><br /> **4** = diariamente<br /><br /> **8** = semanalmente<br /><br /> **16** = mensualmente<br /><br /> **32** = mensualmente relativo<br /><br /> **64** = AutoStart<br /><br /> **128** = recurrente|  
|**frequency_interval**|**int**|Los días en los que se ejecuta el agente; puede tener los valores siguientes.<br /><br /> **1** = Domingo<br /><br /> **2** = lunes<br /><br /> **3** = martes<br /><br /> **4** = miércoles<br /><br /> **5** = jueves<br /><br /> **6** = viernes<br /><br /> **7** = sábado<br /><br /> **8** = día<br /><br /> **9** = días de la semana<br /><br /> **10** = días del fin de semana|  
|**frequency_subday_type**|**int**|Es el tipo que define la frecuencia con que se ejecuta el agente cuando *frequency_type* es **4** (diariamente) y puede tener uno de estos valores.<br /><br /> **1** = en el momento especificado<br /><br /> **2** = segundos<br /><br /> **4** = minutos<br /><br /> **8** = horas|  
|**frequency_subday_interval**|**int**|Número de intervalos de *frequency_subday_type* que se producen entre la ejecución programada del agente.|  
|**frequency_relative_interval**|**int**|Es la semana en la que el agente se ejecuta en un mes determinado cuando *frequency_type* es **32** (relativo mensual) y puede tener uno de estos valores.<br /><br /> **1** = primero<br /><br /> **2** = segundo<br /><br /> **4** = tercero<br /><br /> **8** = cuarto<br /><br /> **16** = último|  
|**frequency_recurrence_factor**|**int**|Número de semanas o meses entre las ejecuciones programadas del agente.|  
|**active_start_date**|**int**|Fecha en que se programa la primera ejecución del agente, con el formato AAAAMMDD.|  
|**active_end_date**|**int**|Fecha en que se programa la última ejecución del agente, con el formato AAAAMMDD.|  
|**active_start_time**|**int**|Hora en que se programa la primera ejecución del agente, con el formato HHMMSS.|  
|**active_end_time**|**int**|Hora en que se programa la última ejecución del agente, con el formato HHMMSS.|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_helpdynamicsnapshot_job** se utiliza en la replicación de mezcla.  
  
 Si se utilizan todos los valores de parámetros predeterminados, se devuelve información sobre todos los trabajos de instantáneas de datos con particiones para toda la base de datos de publicaciones.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** , el rol fijo de base de datos **db_owner** y la lista de acceso a la publicación pueden ejecutar **sp_helpdynamicsnapshot_job**.  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
