---
description: sp_helpsubscriberinfo (Transact-SQL)
title: sp_helpsubscriberinfo (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpsubscriberinfo
- sp_helpsubscriberinfo_TSQL
helpviewer_keywords:
- sp_helpsubscriberinfo
ms.assetid: fbabe1ec-57cf-425c-bae7-af7f5d3198fd
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 25eb68ab0e5c825b646c254f494ab11e1b297b60
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199117"
---
# <a name="sp_helpsubscriberinfo-transact-sql"></a>sp_helpsubscriberinfo (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Presenta información acerca de un suscriptor. Este procedimiento almacenado se ejecuta en el publicador de cualquier base de datos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_helpsubscriberinfo [ [ @subscriber =] 'subscriber']  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @subscriber = ] 'subscriber'` Es el nombre del suscriptor. *Subscriber* es de **tipo sysname y su** valor predeterminado es **%** , que devuelve toda la información.  
  
`[ @publisher = ] 'publisher'` Es el nombre del publicador. *Publisher* es de **tipo sysname** y su valor predeterminado es el nombre del servidor actual.  
  
> [!NOTE]  
>  no se debe especificar el *publicador* , excepto cuando se trata de un publicador de Oracle.  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**publisher**|**sysname**|Nombre del publicador.|  
|**suscriptor**|**sysname**|Nombre del suscriptor.|  
|**type**|**tinyint**|Tipo de suscriptor:<br /><br /> **0**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] base de datos **1** = origen de datos ODBC|  
|**login**|**sysname**|Id. de inicio de sesión para la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**password**|**sysname**|Contraseña para la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticación.|  
|**commit_batch_size**|**int**|No se admite.|  
|**status_batch_size**|**int**|No se admite.|  
|**flush_frequency**|**int**|No se admite.|  
|**frequency_type**|**int**|Frecuencia de ejecución del agente de distribución:<br /><br /> **1** = una vez<br /><br /> **2** = a petición<br /><br /> **4** = diariamente<br /><br /> **8** = semanalmente<br /><br /> **16** = mensualmente<br /><br /> **32** = mensualmente relativo<br /><br /> **64** = AutoStart<br /><br /> **128** = recurrente|  
|**frequency_interval**|**int**|Valor que se aplica a la frecuencia establecida por *frequency_type*.|  
|**frequency_relative_interval**|**int**|Fecha de la Agente de distribución que se usa cuando *frequency_type* está establecida en **32** (relativo mensual):<br /><br /> **1** = primero<br /><br /> **2** = segundo<br /><br /> **4** = tercero<br /><br /> **8** = cuarto<br /><br /> **16** = último|  
|**frequency_recurrence_factor**|**int**|Factor de periodicidad utilizado por *frequency_type*.|  
|**frequency_subday**|**int**|Frecuencia de repetición de la programación durante el periodo definido:<br /><br /> **1** = una vez<br /><br /> **2** = segundo<br /><br /> **4** = minuto<br /><br /> **8** = hora|  
|**frequency_subday_interval**|**int**|Intervalo de *frequency_subday*.|  
|**active_start_time_of_day**|**int**|Hora del día en que el agente de distribución se programa por primera vez, con el formato HHMMSS.|  
|**active_end_time_of_day**|**int**|Hora del día en que el agente de distribución deja de estar programado, con el formato HHMMSS.|  
|**active_start_date**|**int**|Fecha en que el agente de distribución se programa por primera vez, con el formato AAAAMMDD.|  
|**active_end_date**|**int**|Fecha en la que el agente de distribución deja de estar programado, con el formato AAAAMMDD.|  
|**retryattempt**|**int**|No se admite.|  
|**retrydelay**|**int**|No se admite.|  
|**description**|**nvarchar(255)**|Descripción del suscriptor.|  
|**security_mode**|**int**|Modo de seguridad implementado.<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticación<br /><br /> **1**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] autenticación de Windows|  
|**frequency_type2**|**int**|Frecuencia de ejecución del Agente de mezcla:<br /><br /> **1** = una vez<br /><br /> **2** = a petición<br /><br /> **4** = diariamente<br /><br /> **8** = semanalmente<br /><br /> **16** = mensualmente<br /><br /> **32** = mensualmente relativo<br /><br /> **64** = AutoStart<br /><br /> **128** = recurrente|  
|**frequency_interval2**|**int**|Valor que se aplica a la frecuencia establecida por *frequency_type*.|  
|**frequency_relative_interval2**|**int**|Fecha de la Agente de mezcla utilizada cuando *frequency_type* está establecido en 32 (relativo mensual):<br /><br /> **1** = primero<br /><br /> **2** = segundo<br /><br /> **4** = tercero<br /><br /> **8** = cuarto<br /><br /> **16** = último|  
|**frequency_recurrence_factor2**|**int**|Factor de periodicidad utilizado por *frequency_type * *.*|  
|**frequency_subday2**|**int**|Frecuencia de repetición de la programación durante el periodo definido:<br /><br /> **1** = una vez<br /><br /> **2** = segundo<br /><br /> **4** = minuto<br /><br /> **8** = hora|  
|**frequency_subday_interval2**|**int**|Intervalo de *frequency_subday*.|  
|**active_start_time_of_day2**|**int**|Hora del día en que el Agente de mezcla se programa por primera vez, con el formato HHMMSS.|  
|**active_end_time_of_day2**|**int**|Hora del día en que el Agente de mezcla deja de estar programado, con el formato HHMMSS.|  
|**active_start_date2**|**int**|Fecha en que el Agente de mezcla se programa por primera vez, con el formato AAAAMMDD.|  
|**active_end_date2**|**int**|Fecha en la que el Agente de mezcla deja de estar programado, con el formato AAAAMMDD.|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_helpsubscriberinfo** se utiliza en la replicación de instantáneas, la replicación transaccional y la replicación de mezcla.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** , del rol fijo de base de datos **db_owner** o de la lista de acceso a la publicación pueden ejecutar **sp_helpsubscriberinfo**.  
  
## <a name="see-also"></a>Consulte también  
 [sp_adddistpublisher &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md)   
 [sp_addpullsubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addpullsubscription-transact-sql.md)   
 [sp_changesubscriber &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-changesubscriber-transact-sql.md)   
 [sp_dropsubscriber &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropsubscriber-transact-sql.md)   
 [sp_helpdistributor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdistributor-transact-sql.md)   
 [sp_helpserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpserver-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
