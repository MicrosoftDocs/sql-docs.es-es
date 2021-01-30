---
description: sp_helpmergesubscription (Transact-SQL)
title: sp_helpmergesubscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpmergesubscription
- sp_helpmergesubscription_TSQL
helpviewer_keywords:
- sp_helpmergesubscription
ms.assetid: da564112-f769-4e67-9251-5699823e8c86
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 274048dcfcd76e815d3f4ec159dded0571f66e43
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210885"
---
# <a name="sp_helpmergesubscription-transact-sql"></a>sp_helpmergesubscription (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve información sobre una suscripción a una publicación de combinación, tanto de inserción como de extracción. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicaciones o en el suscriptor de republicaciones de la base de datos de suscripciones.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_helpmergesubscription [ [ @publication=] 'publication']  
    [ , [ @subscriber=] 'subscriber']  
    [ , [ @subscriber_db=] 'subscriber_db']  
    [ , [ @publisher=] 'publisher']  
    [ , [ @publisher_db=] 'publisher_db']  
    [ , [ @subscription_type=] 'subscription_type']  
    [ , [ @found=] 'found' OUTPUT]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publication = ] 'publication'` Es el nombre de la publicación. *Publication* es de **tipo sysname y su** valor predeterminado es **%** . La publicación debe existir y debe cumplir las normas de los identificadores. Si es NULL o **%** , se devuelve información acerca de todas las publicaciones y suscripciones de combinación en la base de datos actual.  
  
`[ @subscriber = ] 'subscriber'` Es el nombre del suscriptor. *Subscriber* es de **tipo sysname y su** valor predeterminado es **%** . Si es NULL o %, se devuelve información acerca de todas las suscripciones a la publicación dada.  
  
`[ @subscriber_db = ] 'subscriber_db'` Es el nombre de la base de datos de suscripciones. *subscriber_db* es de **tipo sysname y su** valor predeterminado es **%** , que devuelve información acerca de todas las bases de datos de suscripciones.  
  
`[ @publisher = ] 'publisher'` Es el nombre del publicador. El publicador debe ser un servidor válido. *Publisher* es de **tipo sysname y su** valor predeterminado es **%** , que devuelve información acerca de todos los publicadores.  
  
`[ @publisher_db = ] 'publisher_db'` Es el nombre de la base de datos del publicador. *publisher_db* es de **tipo sysname y su** valor predeterminado es **%** , que devuelve información acerca de todas las bases de datos del publicador.  
  
`[ @subscription_type = ] 'subscription_type'` Es el tipo de suscripción. *subscription_type* es **nvarchar (15)** y puede tener uno de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|de **extracción** (valor predeterminado)|Suscripción de inserción.|  
|**obtener**|Suscripción de extracción|  
|**ambos**|Una suscripción de inserción y de extracción|  
  
`[ @found = ] 'found'OUTPUT` Es una marca que indica que se devuelven filas. *found* es de **tipo int** y un parámetro output, con un valor predeterminado de NULL. **1** indica que se ha encontrado la publicación. **0** indica que no se encuentra la publicación.  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**subscription_name**|**sysname**|Nombre de la suscripción.|  
|**publicaciones**|**sysname**|Nombre de la publicación.|  
|**publisher**|**sysname**|Nombre del publicador.|  
|**publisher_db**|**sysname**|Nombre de la base de datos del publicador.|  
|**suscriptor**|**sysname**|Nombre del suscriptor.|  
|**subscriber_db**|**sysname**|Nombre de la base de datos de suscripciones.|  
|**status**|**int**|Estado de la suscripción:<br /><br /> **0** = todos los trabajos están a la espera de iniciarse<br /><br /> **1** = uno o más trabajos se están iniciando<br /><br /> **2** = todos los trabajos se han ejecutado correctamente<br /><br /> **3** = al menos un trabajo se está ejecutando<br /><br /> **4** = todos los trabajos están programados e inactivos<br /><br /> **5** = al menos un trabajo está intentando ejecutarse después de un error anterior<br /><br /> **6** = al menos un trabajo no se pudo ejecutar correctamente|  
|**subscriber_type**|**int**|Tipo de suscriptor.|  
|**subscription_type**|**int**|Tipo de suscripción:<br /><br /> **0** = inserciones<br /><br /> **1** = extracción<br /><br /> **2** = ambos|  
|**priority**|**Float (8)**|Número que indica la prioridad de la suscripción.|  
|**sync_type**|**tinyint**|Tipo de sincronización de la suscripción.|  
|**description**|**nvarchar(255)**|Breve descripción de esta suscripción de mezcla.|  
|**merge_jobid**|**binario (16)**|Id. de trabajo del Agente de mezcla.|  
|**full_publication**|**tinyint**|Indica si la suscripción es a una publicación completa o filtrada.|  
|**offload_enabled**|**bit**|Especifica si se ha establecido que la ejecución de la descarga de un agente de replicación se lleve a cabo en el suscriptor. Si es NULL, la ejecución se lleva a cabo en el publicador.|  
|**offload_server**|**sysname**|Nombre del servidor donde se está ejecutando el agente.|  
|**use_interactive_resolver**|**int**|Devuelve si se utiliza o no el solucionador interactivo durante la reconciliación. Si es **0**, no se utiliza el solucionador interactivo.|  
|**hostname**|**sysname**|Valor proporcionado cuando una suscripción se filtra por el valor de la función [host_name](../../t-sql/functions/host-name-transact-sql.md) .|  
|**subscriber_security_mode**|**smallint**|Es el modo de seguridad en el suscriptor, donde **1** significa autenticación de Windows y **0** significa [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticación.|  
|**subscriber_login**|**sysname**|Es el nombre de inicio de sesión del suscriptor.|  
|**subscriber_password**|**sysname**|La contraseña real del suscriptor no se devuelve nunca. El resultado se enmascara con una cadena " **\*\*\*\*\*\*** ".|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_helpmergesubscription** se utiliza en la replicación de mezcla para devolver la información de suscripción almacenada en el publicador o en el suscriptor de republicación.  
  
 En el caso de las suscripciones anónimas, el valor de *subscription_type* es siempre **1** (extracción). Sin embargo, debe ejecutar [sp_helpmergepullsubscription](../../relational-databases/system-stored-procedures/sp-helpmergepullsubscription-transact-sql.md) en el suscriptor para obtener información sobre las suscripciones anónimas.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** , el rol fijo de base de datos **db_owner** o la lista de acceso a la publicación para la publicación a la que pertenece la suscripción pueden ejecutar **sp_helpmergesubscription**.  
  
## <a name="see-also"></a>Consulte también  
 [sp_addmergesubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addmergesubscription-transact-sql.md)   
 [sp_changemergesubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-changemergesubscription-transact-sql.md)   
 [sp_dropmergesubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropmergesubscription-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
