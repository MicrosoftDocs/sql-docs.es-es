---
description: sp_adddynamicsnapshot_job (Transact-SQL)
title: sp_adddynamicsnapshot_job (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_adddynamicsnapshot_job
- sp_adddynamicsnapshot_job_TSQL
helpviewer_keywords:
- sp_adddynamicsnapshot_job
ms.assetid: ef50ccf6-e360-4e4b-91b9-6706b8fabefa
author: mashamsft
ms.author: mathoma
ms.openlocfilehash: 9798613901be96552813ebe6f419f2e2c24229fd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182798"
---
# <a name="sp_adddynamicsnapshot_job-transact-sql"></a>sp_adddynamicsnapshot_job (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Crea un trabajo de agente que genera una instantánea de datos filtrados para una publicación con filtros de fila con parámetros. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación. Lo utiliza el administrador para crear manualmente trabajos de instantáneas de datos filtrados para los suscriptores.  
  
> [!NOTE]  
>  Para crear un trabajo de instantáneas de datos filtrados, antes debe existir un trabajo de instantáneas estándar para la publicación.  
  
 Para más información, consulte [Instantáneas para publicaciones de combinación con filtros con parámetros](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_adddynamicsnapshot_job [ @publication = ] 'publication'   
    [ , [ @suser_sname = ] 'suser_sname' ]   
    [ , [ @host_name = ] 'host_name' ]   
    [ , [ @dynamic_snapshot_jobname = ] 'dynamic_snapshot_jobname' OUTPUT ]   
    [ , [ @dynamic_snapshot_jobid = ] 'dynamic_snapshot_jobid' OUTPUT ]   
    [ , [ @frequency_type= ] frequency_type ]  
    [ , [ @frequency_interval= ] frequency_interval ]  
    [ , [ @frequency_subday= ] frequency_subday ]  
    [ , [ @frequency_subday_interval= ] frequency_subday_interval ]  
    [ , [ @frequency_relative_interval= ] frequency_relative_interval ]  
    [ , [ @frequency_recurrence_factor= ] frequency_recurrence_factor ]  
    [ , [ @active_start_date= ] active_start_date ]  
    [ , [ @active_end_date= ] active_end_date ]  
    [ , [ @active_start_time_of_day= ] active_start_time_of_day ]  
    [ , [ @active_end_time_of_day= ] active_end_time_of_day ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publication = ] 'publication'` Es el nombre de la publicación a la que se va a agregar el trabajo de instantáneas de datos filtrados. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @suser_sname = ] 'suser_sname'` Es el valor utilizado al crear una instantánea de datos filtrados para una suscripción que se filtra por el valor de la función [SUSER_SNAME](../../t-sql/functions/suser-sname-transact-sql.md) en el suscriptor. *SUSER_SNAME* es de **tipo sysname** y no tiene ningún valor predeterminado. *SUSER_SNAME* debe ser null si esta función no se utiliza para filtrar dinámicamente la publicación.  
  
`[ @host_name = ] 'host_name'` Es el valor utilizado al crear una instantánea de datos filtrados para una suscripción que se filtra por el valor de la función [host_name](../../t-sql/functions/host-name-transact-sql.md) en el suscriptor. *host_name* es de **tipo sysname** y no tiene ningún valor predeterminado. *host_name* debe ser null si esta función no se utiliza para filtrar dinámicamente la publicación.  
  
`[ @dynamic_snapshot_jobname = ] 'dynamic_snapshot_jobname'` Es el nombre del trabajo de instantáneas de datos filtrados creado. *dynamic_snapshot_jobname* es de **tipo sysname, su** valor predeterminado es NULL y es un parámetro Output opcional. Si se especifica, *dynamic_snapshot_jobname* debe resolverse en un trabajo único en el distribuidor. Si no se especifica, se generará un nombre de trabajo automáticamente, el cual se devolverá al conjunto de resultados. Allí, el nombre se crea del siguiente modo:  
  
```  
'dyn_' + <name of the standard snapshot job> + <GUID>  
```  
  
> [!NOTE]  
>  Al generar el nombre del trabajo de instantáneas dinámicas, es posible truncar el nombre del trabajo de instantáneas estándar.  
  
`[ @dynamic_snapshot_jobid = ] 'dynamic_snapshot_jobid'` Es un identificador para el trabajo de instantáneas de datos filtrados creado. *dynamic_snapshot_jobid* es de tipo **uniqueidentifier**, su valor predeterminado es NULL y es un parámetro Output opcional.  
  
`[ @frequency_type = ] frequency_type` Es la frecuencia con que se programa el trabajo de instantáneas de datos filtrados. *frequency_type* es de **tipo int** y puede tener uno de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**1**|Una vez|  
|**2**|A petición|  
|**4** (valor predeterminado)|Diario|  
|**8**|Cada semana|  
|**16**|Mensual|  
|**32**|Mensualmente relativa|  
|**64**|Iniciar automáticamente|  
|**128**|Periódica|  
  
`[ @frequency_interval = ] frequency_interval` Es el período (medido en días) en el que se ejecuta el trabajo de instantáneas de datos filtrados. *frequency_interval* es de **tipo int**, su valor predeterminado es 1 y depende del valor de *frequency_type*.  
  
|Valor de *frequency_type*|Efecto en *frequency_interval*|  
|--------------------------------|-------------------------------------|  
|**1**|*frequency_interval* no se usa.|  
|**4** (valor predeterminado)|Cada *frequency_interval* días, con un valor predeterminado de Daily.|  
|**8**|*frequency_interval* es uno o más de los siguientes (combinados con un [&#124; &#40;operador lógico OR bit a bit&#41; &#40;de TRANSACT-&#41;SQL](../../t-sql/language-elements/bitwise-or-transact-sql.md) ):<br /><br /> **1** = Domingo &#124; **2** = lunes &#124; **4** = martes &#124; **8** = miércoles &#124; **16** = jueves &#124; **32** = viernes &#124; **64** = sábado|  
|**16**|En el *frequency_interval* día del mes.|  
|**32**|*frequency_interval* es uno de los siguientes:<br /><br /> **1** = Domingo &#124; **2** = lunes &#124; **3** = martes &#124; **4** = miércoles &#124; **5** = jueves &#124; **6** = viernes &#124; **7** = sábado &#124; **8** = día &#124; **9** = día de la semana &#124; **10** = día del fin de semana|  
|**64**|*frequency_interval* no se usa.|  
|**128**|*frequency_interval* no se usa.|  
  
`[ @frequency_subday = ] frequency_subday` Especifica las unidades para *frequency_subday_interval*. *frequency_subday* es de **tipo int** y puede tener uno de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**1**|Una sola vez|  
|**2**|Segundo|  
|**4** (valor predeterminado)|Minute|  
|**8**|Hora|  
  
`[ @frequency_subday_interval = ] frequency_subday_interval` Es el número de períodos *frequency_subday* que se producen entre cada ejecución del trabajo. *frequency_subday_interval* es de **tipo int** y su valor predeterminado es 5.  
  
`[ @frequency_relative_interval = ] frequency_relative_interval` Es la aparición del trabajo de instantáneas de datos filtrados en cada mes. Este parámetro se utiliza cuando *frequency_type* se establece en **32** (relativo mensual). *frequency_relative_interval* es de **tipo int** y puede tener uno de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**1** (predeterminado)|First|  
|**2**|Segundo|  
|**4**|Tercero|  
|**8**|Cuarto|  
|**16**|Último|  
  
`[ @frequency_recurrence_factor = ] frequency_recurrence_factor` Es el factor de periodicidad utilizado por *frequency_type*. *frequency_recurrence_factor* es de **tipo int** y su valor predeterminado es 0.  
  
`[ @active_start_date = ] active_start_date` Es la fecha en que el trabajo de instantáneas de datos filtrados se programa por primera vez, con el formato AAAAMMDD. *active_start_date* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @active_end_date = ] active_end_date` Es la fecha en la que el trabajo de instantáneas de datos filtrados deja de estar programado, con el formato AAAAMMDD. *active_end_date* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @active_start_time_of_day = ] active_start_time_of_day` Es la hora del día en que el trabajo de instantáneas de datos filtrados se programa por primera vez, con el formato HHMMSS. *active_start_time_of_day* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @active_end_time_of_day = ] active_end_time_of_day` Es la hora del día en que el trabajo de instantáneas de datos filtrados deja de estar programado, con el formato HHMMSS. *active_end_time_of_day* es de **tipo int** y su valor predeterminado es NULL.  
  
## <a name="result-set"></a>Tipo de cursor  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**id**|**int**|Identifica el trabajo de instantánea de datos filtrados en la tabla del sistema [MSdynamicsnapshotjobs](../../relational-databases/system-tables/msdynamicsnapshotjobs-transact-sql.md) .|  
|**dynamic_snapshot_jobname**|**sysname**|Nombre del trabajo de instantáneas de datos filtrados.|  
|**dynamic_snapshot_jobid**|**uniqueidentifier**|Identifica de forma única el [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] trabajo del agente en el distribuidor.|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_adddynamicsnapshot_job** se utiliza en la replicación de mezcla para las publicaciones que usan un filtro con parámetros.  
  
## <a name="example"></a>Ejemplo  
 [!code-sql[HowTo#sp_MergeDynamicPubPlusPartition](../../relational-databases/replication/codesnippet/tsql/sp-adddynamicsnapshot-jo_1.sql)]  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_adddynamicsnapshot_job**.  
  
## <a name="see-also"></a>Consulte también  
 [Crear una instantánea para una publicación de combinación con filtros con parámetros](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md)   
 [Filtros de fila con parámetros](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)   
 [sp_dropdynamicsnapshot_job &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropdynamicsnapshot-job-transact-sql.md)   
 [sp_helpdynamicsnapshot_job &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpdynamicsnapshot-job-transact-sql.md)  
  
  
