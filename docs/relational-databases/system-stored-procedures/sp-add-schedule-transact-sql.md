---
description: sp_add_schedule (Transact-SQL)
title: sp_add_schedule (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_schedule_TSQL
- sp_add_schedule
dev_langs:
- TSQL
helpviewer_keywords:
- sp_add_schedule
ms.assetid: 9060aae3-3ddd-40a5-83bb-3ea7ab1ffbd7
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c9199eaf3e9ee7a78053eb92d45648b89e76dd56
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192405"
---
# <a name="sp_add_schedule-transact-sql"></a>sp_add_schedule (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Crea una programación que puede ser utilizada por un número indeterminado de trabajos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_add_schedule [ @schedule_name = ] 'schedule_name'   
    [ , [ @enabled = ] enabled ]  
    [ , [ @freq_type = ] freq_type ]  
    [ , [ @freq_interval = ] freq_interval ]   
    [ , [ @freq_subday_type = ] freq_subday_type ]   
    [ , [ @freq_subday_interval = ] freq_subday_interval ]   
    [ , [ @freq_relative_interval = ] freq_relative_interval ]   
    [ , [ @freq_recurrence_factor = ] freq_recurrence_factor ]   
    [ , [ @active_start_date = ] active_start_date ]   
    [ , [ @active_end_date = ] active_end_date ]   
    [ , [ @active_start_time = ] active_start_time ]   
    [ , [ @active_end_time = ] active_end_time ]   
    [ , [ @owner_login_name = ] 'owner_login_name' ]  
    [ , [ @schedule_uid = ] schedule_uid OUTPUT ]  
    [ , [ @schedule_id = ] schedule_id OUTPUT ]
    [ , [ @originating_server = ] server_name ] /* internal */  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @schedule_name = ] 'schedule_name'` Nombre de la programación. *schedule_name* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @enabled = ] enabled` Indica el estado actual de la programación. *Enabled* es de **tinyint** y su valor predeterminado es **1** (habilitado). Si es **0**, la programación no está habilitada. Si la programación no está habilitada, no se ejecuta ningún trabajo en esta programación.  
  
`[ @freq_type = ] freq_type` Valor que indica cuándo se va a ejecutar un trabajo. *freq_type* es de **tipo int**, su valor predeterminado es **0** y puede tener uno de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**1**|Una sola vez|  
|**4**|Diario|  
|**8**|Cada semana|  
|**16**|Mensual|  
|**32**|Mensualmente, con respecto a *freq_interval*|  
|**64**|Ejecutar cuando se inicia el servicio Agente SQL|  
|**128**|Ejecutar cuando el equipo está inactivo (no se admite en [Azure SQL instancia administrada](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)) |  
  
`[ @freq_interval = ] freq_interval` Los días en los que se ejecuta un trabajo. *freq_interval* es de **tipo int**, su valor predeterminado es **1** y depende del valor de *freq_type*.  
  
|Valor de *freq_type*|Efecto en *freq_interval*|  
|---------------------------|--------------------------------|  
|**1** (una vez)|*freq_interval* no se usa.|  
|**4** (diariamente)|Cada *freq_interval* días.|  
|**8** (semanalmente)|*freq_interval* es uno o más de los siguientes (combinados con un operador lógico or):<br /><br /> **1** = Domingo<br /><br /> **2** = lunes<br /><br /> **4** = martes<br /><br /> **8** = miércoles<br /><br /> **16** = jueves<br /><br /> **32** = viernes<br /><br /> **64** = sábado|  
|**16** (mensualmente)|En el *freq_interval* día del mes.|  
|**32** (relativo mensual)|*freq_interval* es uno de los siguientes:<br /><br /> **1** = Domingo<br /><br /> **2** = lunes<br /><br /> **3** = martes<br /><br /> **4** = miércoles<br /><br /> **5** = jueves<br /><br /> **6** = viernes<br /><br /> **7** = sábado<br /><br /> **8** = día<br /><br /> **9** = día de la semana<br /><br /> **10** = día del fin de semana|  
|**64** (cuando se inicia el servicio SQLServerAgent)|*freq_interval* no se usa.|  
|**128**|*freq_interval* no se usa.|  
  
`[ @freq_subday_type = ] freq_subday_type` Especifica las unidades para *freq_subday_interval*. *freq_subday_type* es de **tipo int**, su valor predeterminado es **0** y puede tener uno de estos valores.  
  
|Value|Descripción (unidad)|  
|-----------|--------------------------|  
|**0x1**|A la hora especificada|  
|**0X2**|Segundos|  
|**0x4**|Minutos|  
|**0x8**|Horas|  
  
`[ @freq_subday_interval = ] freq_subday_interval` Número de períodos de *freq_subday_type* que se van a producir entre cada ejecución de un trabajo. *freq_subday_interval* es de **tipo int** y su valor predeterminado es **0**. Nota: el intervalo debe ser mayor que 10 segundos. *freq_subday_interval* se omite en los casos en los que *freq_subday_type* es igual a **1**.  
  
`[ @freq_relative_interval = ] freq_relative_interval` La repetición de un trabajo de *freq_interval* en cada mes, si *freq_interval* es 32 (relativo mensual). *freq_relative_interval* es de **tipo int**, su valor predeterminado es **0** y puede tener uno de estos valores. *freq_relative_interval* se omite en los casos en los que *freq_type* no es igual a 32.  
  
|Value|Descripción (unidad)|  
|-----------|--------------------------|  
|**1**|First|  
|**2**|Segundo|  
|**4**|Tercero|  
|**8**|Cuarto|  
|**16**|Último|  
  
`[ @freq_recurrence_factor = ] freq_recurrence_factor` Número de semanas o meses entre la ejecución programada de un trabajo. *freq_recurrence_factor* solo se utiliza si *freq_type* es **8**, **16** o **32**. *freq_recurrence_factor* es de **tipo int** y su valor predeterminado es **0**.  
  
`[ @active_start_date = ] active_start_date` Fecha en la que puede comenzar la ejecución de un trabajo. *active_start_date* es de **tipo int** y su valor predeterminado es null, lo que indica la fecha de hoy. La fecha tiene el formato AAAAMMDD. Si *active_start_date* no es null, la fecha debe ser mayor o igual que 19900101.  
  
 Una vez creada la programación, revise la fecha de inicio y confirme que es correcta. Para obtener más información, vea la sección sobre la programación de la fecha de inicio en [crear y adjuntar programaciones a trabajos](../../ssms/agent/create-and-attach-schedules-to-jobs.md).  
  
 En las programaciones semanales o mensuales, el agente desconoce si active_start_date corresponde al pasado, y en su lugar utiliza la fecha actual. Cuando se crea una programación del Agente SQL mediante sp_add_schedule, existe la opción de especificar el parámetro active_start_date, que es la fecha en la que comenzará la ejecución del trabajo. Si el tipo de programación es semanal o mensual y el parámetro active_start_date se establece en una fecha en el pasado, se hace caso omiso de dicho parámetro y se utilizará en su lugar la fecha actual.  
  
`[ @active_end_date = ] active_end_date` Fecha en la que se puede detener la ejecución de un trabajo. *active_end_date* es de **tipo int** y su valor predeterminado es **99991231**, que indica el 31 de diciembre de 9999. Tiene el formato AAAAMMDD.  
  
`[ @active_start_time = ] active_start_time` Hora de un día entre *active_start_date* y *active_end_date* para iniciar la ejecución de un trabajo. *active_start_time* es de **tipo int** y su valor predeterminado es **000000**, lo que indica 12:00:00 A.M. en un reloj de 24 horas. Se debe especificar con el formato HHMMSS.  
  
`[ @active_end_time = ] active_end_time` La hora de un día entre *active_start_date* y *active_end_date* a la finalización de la ejecución de un trabajo. *active_end_time* es de **tipo int** y su valor predeterminado es **235959**, que indica 11:59:59 P.M. en un reloj de 24 horas. Se debe especificar con el formato HHMMSS.  
  
`[ @owner_login_name = ] 'owner_login_name'` El nombre de la entidad de seguridad del servidor que posee la programación. *owner_login_name* es de **tipo sysname** y su valor predeterminado es null, lo que indica que la programación es propiedad del creador.  
  
`[ @schedule_uid = ] _schedule_uidOUTPUT` Identificador único de la programación. *schedule_uid* es una variable de tipo **uniqueidentifier**.  
  
`[ @schedule_id = ] _schedule_idOUTPUT` Identificador de la programación. *schedule_id* es una variable de tipo **int**.  
  
`[ @originating_server = ] server_name` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="remarks"></a>Observaciones  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ofrece un método gráfico sencillo para administrar trabajos y es el método recomendado para crear y administrar la infraestructura de trabajo.  
  
## <a name="permissions"></a>Permisos  
 De forma predeterminada, los miembros del rol fijo de servidor **sysadmin** pueden ejecutar este procedimiento almacenado. Al resto de usuarios se les debe conceder uno de los siguientes roles fijos de base de datos del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la base de datos **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Para detalles sobre los permisos de estos roles, consulte [Roles fijos de base de datos del Agente SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-creating-a-schedule"></a>A. Creación de una programación  
 En el siguiente ejemplo se crea una programación llamada `RunOnce`. La programación se ejecuta una vez a las `23:30` el día en que se ha creado la programación.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_add_schedule  
    @schedule_name = N'RunOnce',  
    @freq_type = 1,  
    @active_start_time = 233000 ;  
  
GO  
```  
  
### <a name="b-creating-a-schedule-attaching-the-schedule-to-multiple-jobs"></a>B. Crear una programación y adjuntarla a varios trabajos  
 En el siguiente ejemplo se crea una programación llamada `NightlyJobs`. Los trabajos que usan esta programación se ejecutan a diario cuando la hora del servidor es `01:00`. En el ejemplo se asocia la programación a los trabajos `BackupDatabase` y `RunReports`.  
  
> [!NOTE]  
>  En este ejemplo se da por supuesto que el trabajo `BackupDatabase` y el trabajo `RunReports` ya existen.  
  
```  
USE msdb ;  
GO  
  
EXEC sp_add_schedule  
    @schedule_name = N'NightlyJobs' ,  
    @freq_type = 4,  
    @freq_interval = 1,  
    @active_start_time = 010000 ;  
GO  
  
EXEC sp_attach_schedule  
   @job_name = N'BackupDatabase',  
   @schedule_name = N'NightlyJobs' ;  
GO  
  
EXEC sp_attach_schedule  
   @job_name = N'RunReports',  
   @schedule_name = N'NightlyJobs' ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Crear y adjuntar programaciones a trabajos](../../ssms/agent/create-and-attach-schedules-to-jobs.md)   
 [Programar un trabajo](../../ssms/agent/schedule-a-job.md)   
 [Crear una programación](../../ssms/agent/create-a-schedule.md)   
 [Agente SQL Server procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)   
 [sp_add_jobschedule &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-jobschedule-transact-sql.md)   
 [sp_update_schedule &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-update-schedule-transact-sql.md)   
 [sp_delete_schedule &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-schedule-transact-sql.md)   
 [sp_help_schedule &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-schedule-transact-sql.md)   
 [sp_attach_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-attach-schedule-transact-sql.md)  
  
