---
description: sp_help_jobhistory (Transact-SQL)
title: sp_help_jobhistory (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_jobhistory_TSQL
- sp_help_jobhistory
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_jobhistory
ms.assetid: a944d44e-411b-4735-8ce4-73888d4262d7
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 2cfef1ba5f28b498ab360daf67cc1bf3c79f0d2a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200137"
---
# <a name="sp_help_jobhistory-transact-sql"></a>sp_help_jobhistory (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Proporciona información acerca de los trabajos en los servidores de un dominio de administración multiservidor.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_help_jobhistory [ [ @job_id = ] job_id ]   
     [ , [ @job_name = ] 'job_name' ]   
     [ , [ @step_id = ] step_id ]   
     [ , [ @sql_message_id = ] sql_message_id ]   
     [ , [ @sql_severity = ] sql_severity ]   
     [ , [ @start_run_date = ] start_run_date ]   
     [ , [ @end_run_date = ] end_run_date ]   
     [ , [ @start_run_time = ] start_run_time ]   
     [ , [ @end_run_time = ] end_run_time ]   
     [ , [ @minimum_run_duration = ] minimum_run_duration ]   
     [ , [ @run_status = ] run_status ]   
     [ , [ @minimum_retries = ] minimum_retries ]   
     [ , [ @oldest_first = ] oldest_first ]   
     [ , [ @server = ] 'server' ]   
     [ , [ @mode = ] 'mode' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @job_id = ] job_id` El número de identificación del trabajo. *job_id* es de tipo **uniqueidentifier** y su valor predeterminado es NULL.  
  
`[ @job_name = ] 'job_name'` Nombre del trabajo. *job_name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @step_id = ] step_id` El número de identificación del paso. *step_id* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @sql_message_id = ] sql_message_id` El número de identificación del mensaje de error devuelto por Microsoft SQL Server al ejecutar el trabajo. *sql_message_id* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @sql_severity = ] sql_severity` Nivel de gravedad del mensaje de error devuelto por SQL Server al ejecutar el trabajo. *sql_severity* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @start_run_date = ] start_run_date` Fecha en que se inició el trabajo. *start_run_date* es de **tipo int** y su valor predeterminado es NULL. *start_run_date* se debe escribir con el formato AAAAMMDD, donde aaaa es un año con cuatro cifras, mm es el mes con dos cifras y dd es el día con dos cifras.  
  
`[ @end_run_date = ] end_run_date` Fecha en que se completó el trabajo. *end_run_date* es de **tipo int** y su valor predeterminado es NULL. *end_run_date* se debe escribir con el formato AAAAMMDD, donde aaaa es un año de cuatro dígitos, mm es el mes con dos cifras y dd es el día con dos cifras.  
  
`[ @start_run_time = ] start_run_time` La hora a la que se inició el trabajo. *start_run_time* es de **tipo int** y su valor predeterminado es NULL. *start_run_time* debe especificarse con el formato HHMMSS, donde HH es la hora de dos caracteres del día, mm son los minutos con dos cifras y SS son los segundos con dos cifras del día.  
  
`[ @end_run_time = ] end_run_time` La hora a la que el trabajo finalizó su ejecución. *end_run_time* es de **tipo int** y su valor predeterminado es NULL. *end_run_time* debe especificarse con el formato HHMMSS, donde HH es la hora de dos caracteres del día, mm son los minutos con dos cifras y SS son los segundos con dos cifras del día.  
  
`[ @minimum_run_duration = ] minimum_run_duration` La cantidad mínima de tiempo para la finalización del trabajo. *minimum_run_duration* es de **tipo int** y su valor predeterminado es NULL. *minimum_run_duration* debe especificarse con el formato HHMMSS, donde HH es la hora de dos caracteres del día, mm son los minutos con dos cifras y SS son los segundos con dos cifras del día.  
  
`[ @run_status = ] run_status` El estado de ejecución del trabajo. *run_status* es de **tipo int**, su valor predeterminado es NULL y puede tener uno de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**0**|Con error|  
|**1**|Correcto|  
|**2**|Reintento (solo pasos)|  
|**3**|Canceled|  
|**4**|Mensaje en curso|  
|**5**|Unknown|  
  
`[ @minimum_retries = ] minimum_retries` El número mínimo de veces que un trabajo debe volver a ejecutarse. *minimum_retries* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @oldest_first = ] oldest_first` Indica si se va a presentar la salida con los trabajos más antiguos en primer lugar. *oldest_first* es de **tipo int** y su valor predeterminado es **0**, que presenta los trabajos más recientes en primer lugar. **1** presenta los trabajos más antiguos en primer lugar.  
  
`[ @server = ] 'server'` Nombre del servidor en el que se realizó el trabajo. el *servidor* es de tipo **nvarchar (30)** y su valor predeterminado es NULL.  
  
`[ @mode = ] 'mode'` Indica si SQL Server imprime todas las columnas del conjunto de resultados (**Full**) o un resumen de las columnas. el *modo* es **VARCHAR (7)** y su valor predeterminado es **Summary**.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 La lista de columnas real depende del valor de *mode*. A continuación se muestra el conjunto de columnas más completo y se devuelve cuando el *modo* está lleno.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**instance_id**|**int**|Número de identificación de la entrada en el historial.|  
|**job_id**|**uniqueidentifier**|Número de identificación del trabajo.|  
|**job_name**|**sysname**|Nombre del trabajo.|  
|**step_id**|**int**|Número de identificación del paso (será **0** para un historial de trabajos).|  
|**step_name**|**sysname**|Nombre de paso (será NULL para un historial de trabajos).|  
|**sql_message_id**|**int**|Para pasos [!INCLUDE[tsql](../../includes/tsql-md.md)], número de error de [!INCLUDE[tsql](../../includes/tsql-md.md)] más reciente encontrado en la ejecución del comando.|  
|**sql_severity**|**int**|Para pasos [!INCLUDE[tsql](../../includes/tsql-md.md)], nivel de gravedad más alto de los errores de [!INCLUDE[tsql](../../includes/tsql-md.md)] encontrado en la ejecución del comando.|  
|**message**|**nvarchar(1024)**|Mensaje del historial de trabajos o pasos.|  
|**run_status**|**int**|Resultado del trabajo o del paso.|  
|**run_date**|**int**|Fecha en que empezó la ejecución del trabajo o del paso.|  
|**run_time**|**int**|Hora en que empezó la ejecución del trabajo o del paso.|  
|**run_duration**|**int**|Tiempo transcurrido en la ejecución del trabajo o paso en formato HHMMSS.|  
|**operator_emailed**|**nvarchar (20)**|Operador que recibió mensajes de correo electrónico relativos a este trabajo (es NULL para el historial de pasos).|  
|**operator_netsent**|**nvarchar (20)**|Operador que recibió mensajes de red relativos a este trabajo (es NULL para el historial de pasos).|  
|**operator_paged**|**nvarchar (20)**|Operador que recibió mensajes de localizador relativos a este trabajo (es NULL para el historial de pasos).|  
|**retries_attempted**|**int**|Número de veces que se ha vuelto a intentar el paso (siempre es 0 para un historial de trabajos).|  
|**servidor**|**nvarchar(30)**|Servidor en el que se ejecuta el paso o el trabajo. Siempre es (**local**).|  
  
## <a name="remarks"></a>Observaciones  
 **sp_help_jobhistory** devuelve un informe con el historial de los trabajos programados especificados. Si no se especifican parámetros, el informe contiene el historial de todos los trabajos programados.  
  
## <a name="permissions"></a>Permisos  
 De forma predeterminada, los miembros del rol fijo de servidor **sysadmin** pueden ejecutar este procedimiento almacenado. Al resto de usuarios se les debe conceder uno de los siguientes roles fijos de base de datos del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la base de datos **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Para detalles sobre los permisos de estos roles, consulte [Roles fijos de base de datos del Agente SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Los miembros del rol de base de datos **SQLAgentUserRole** solo pueden ver el historial de los trabajos que les pertenecen.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-listing-all-job-information-for-a-job"></a>A. Mostrar toda la información de un trabajo  
 En el siguiente ejemplo se muestra toda la información del trabajo `NightlyBackups`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_jobhistory   
    @job_name = N'NightlyBackups' ;  
GO  
```  
  
### <a name="b-listing-information-for-jobs-that-match-certain-conditions"></a>B. Mostrar información de trabajos que cumplen determinadas condiciones  
 En el siguiente ejemplo se imprimen todas las columnas y toda la información de los trabajos o pasos de trabajo que no progresaron y devolvieron el mensaje de error `50100` (mensaje de error definido por el usuario) y una gravedad de `20`.  
  
```  
USE msdb  
GO  
  
EXEC dbo.sp_help_jobhistory  
    @sql_message_id = 50100,  
    @sql_severity = 20,  
    @run_status = 0,  
    @mode = N'FULL' ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_purge_jobhistory &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-purge-jobhistory-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
