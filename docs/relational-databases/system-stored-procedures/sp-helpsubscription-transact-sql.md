---
description: sp_helpsubscription (Transact-SQL)
title: sp_helpsubscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpsubscription_TSQL
- sp_helpsubscription
helpviewer_keywords:
- sp_helpsubscription
ms.assetid: ff96bcbf-e2b9-4da8-8515-d80d4ce86c16
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 73fc43bf20a9306d0224392d984b3f2bff74ec6f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192898"
---
# <a name="sp_helpsubscription-transact-sql"></a>sp_helpsubscription (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Presenta la información de suscripción asociada con una publicación particular, un artículo, un suscriptor o un conjunto de suscripciones. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicaciones.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_helpsubscription [ [ @publication = ] 'publication' ]   
    [ , [ @article = ] 'article' ]  
    [ , [ @subscriber = ] 'subscriber' ]  
    [ , [ @destination_db = ] 'destination_db' ]   
    [ , [ @found=] found OUTPUT ]  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publication = ] 'publication'` Es el nombre de la publicación asociada. *Publication* es de **tipo sysname y su** valor predeterminado es **%** , que devuelve toda la información de suscripción de este servidor.  
  
`[ @article = ] 'article'` Es el nombre del artículo. *article* es de **tipo sysname y su** valor predeterminado es **%** , que devuelve toda la información de suscripción de las publicaciones y los suscriptores seleccionados. Si es **All**, solo se devuelve una entrada para la suscripción completa en una publicación.  
  
`[ @subscriber = ] 'subscriber'` Es el nombre del suscriptor en el que se va a obtener información de suscripción. *Subscriber* es de **tipo sysname y su** valor predeterminado es **%** , que devuelve toda la información de suscripción de las publicaciones y los artículos seleccionados.  
  
`[ @destination_db = ] 'destination_db'` Es el nombre de la base de datos de destino. *destination_db* es de **tipo sysname y su** valor predeterminado es **%** .  
  
`[ @found = ] 'found'OUTPUT` Es una marca que indica que se devuelven filas. *found* es de **tipo int** y un parámetro output, con un valor predeterminado de 23456.  
  
 **1** indica que se ha encontrado la publicación.  
  
 **0** indica que no se encuentra la publicación.  
  
`[ @publisher = ] 'publisher'` Es el nombre del publicador. *Publisher* es de **tipo sysname** y su valor predeterminado es el nombre del servidor actual.  
  
> [!NOTE]  
>  no se debe especificar el *publicador* , excepto cuando se trata de un publicador de Oracle.  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**suscriptor**|**sysname**|Nombre del suscriptor.|  
|**publicaciones**|**sysname**|Nombre de la publicación.|  
|**artículo**|**sysname**|Nombre del artículo.|  
|**base de datos de destino**|**sysname**|Nombre de la base de datos de destino a la que se envían los datos duplicados.|  
|**estado de la suscripción**|**tinyint**|Estado de la suscripción:<br /><br /> **0** = inactivo<br /><br /> **1** = suscrito<br /><br /> **2** = activo|  
|**synchronization type**|**tinyint**|Tipo de sincronización de suscripción:<br /><br /> **1** = automática<br /><br /> **2** = ninguno|  
|**tipo de suscripción**|**int**|Tipo de suscripción:<br /><br /> **0** = inserciones<br /><br /> **1** = extracción<br /><br /> **2** = anónimo|  
|**full subscription**|**bit**|Indica si la suscripción es a todos los artículos de la publicación:<br /><br /> **0** = No<br /><br /> **1** = Sí|  
|**nombre de la suscripción**|**nvarchar(255)**|Nombre de la suscripción.|  
|**modo de actualización**|**int**|**0** = solo lectura<br /><br /> **1** = suscripción de actualización inmediata|  
|**distribution job id**|**binario (16)**|Id. de trabajo del agente de distribución.|  
|**loopback_detection**|**bit**|La detección de bucles de retorno determina si el Agente de distribución envía las transacciones originadas en el suscriptor al mismo suscriptor:<br /><br /> **0** = devuelve.<br /><br /> **1** = no devuelve.<br /><br /> Se utilizan con replicación transaccional bidireccional. Para más información, consulte [Bidirectional Transactional Replication](../../relational-databases/replication/transactional/bidirectional-transactional-replication.md).|  
|**offload_enabled**|**bit**|Especifica si se ha establecido que la descarga de un agente de duplicación se lleve a cabo en el suscriptor.<br /><br /> Si es **0**, el agente se ejecuta en el publicador.<br /><br /> Si es **1**, el agente se ejecuta en el suscriptor.|  
|**offload_server**|**sysname**|Nombre del servidor habilitado para la activación remota de agentes. Si es NULL, se usa el offload_server actual que aparece en [MSdistribution_agents](../../relational-databases/system-tables/msdistribution-agents-transact-sql.md) tabla.|  
|**dts_package_name**|**sysname**|Especifica el nombre del paquete de Servicios de transformación de datos (DTS).|  
|**dts_package_location**|**int**|Ubicación del paquete DTS, si se asigna uno a la suscripción. Si hay un paquete, un valor de **0** especifica la ubicación del paquete en el **distribuidor**. Un valor de **1** especifica el **suscriptor**.|  
|**subscriber_security_mode**|**smallint**|Es el modo de seguridad en el suscriptor, donde **1** significa autenticación de Windows y **0** significa [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticación.|  
|**subscriber_login**|**sysname**|Es el nombre de inicio de sesión del suscriptor.|  
|**subscriber_password**||La contraseña real del suscriptor no se devuelve nunca. El resultado se enmascara con una cadena "**&#42;&#42;&#42;&#42;&#42;&#42;**".|  
|**job_login**|**sysname**|Nombre de la cuenta de Windows en la que se ejecuta el Agente de distribución.|  
|**job_password**||La contraseña real del trabajo no se devuelve nunca. El resultado se enmascara con una cadena "**&#42;&#42;&#42;&#42;&#42;&#42;**".|  
|**distrib_agent_name**|**nvarchar(100**|Nombre del trabajo del agente que sincroniza la suscripción.|  
|**subscriber_type**|**tinyint**|Tipo de suscriptor, que puede ser uno de los siguientes:<br /><br /> **0** = suscriptor SQL Server<br /><br /> **1** = servidor de origen de datos ODBC<br /><br /> **2** = base de datos de Microsoft Jet (desusado)<br /><br /> **3** = proveedor de OLE DB|  
|**subscriber_provider**|**sysname**|Identificador de programación único (PROGID) mediante el cual se registra el proveedor OLE DB para los orígenes de datos que no son de SQL Server.|  
|**subscriber_datasource**|**nvarchar(4000)**|Nombre del origen de datos tal y como lo entiende el proveedor OLE DB.|  
|**subscriber_providerstring**|**nvarchar(4000)**|Cadena de conexión específica del proveedor OLE DB que identifica el origen de datos.|  
|**subscriber_location**|**nvarchar(4000)**|Ubicación de la base de datos tal y como la entiende el proveedor OLE DB|  
|**subscriber_catalog**|**sysname**|Catálogo que debe utilizarse al establecer una conexión con el proveedor OLE DB.|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_helpsubscription** se utiliza en la replicación de instantáneas y transaccional.  
  
## <a name="permissions"></a>Permisos  
 De forma predeterminada, los permisos de ejecución corresponden al rol **public** . Los usuarios solo reciben información de las suscripciones que hayan creado. Se devuelve información sobre todas las suscripciones a los miembros del rol fijo de servidor **sysadmin** en el publicador o los miembros del rol fijo de base de datos **db_owner** en la base de datos de publicación.  
  
## <a name="see-also"></a>Consulte también  
 [sp_addsubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md)   
 [sp_changesubstatus &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-changesubstatus-transact-sql.md)   
 [sp_dropsubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropsubscription-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
