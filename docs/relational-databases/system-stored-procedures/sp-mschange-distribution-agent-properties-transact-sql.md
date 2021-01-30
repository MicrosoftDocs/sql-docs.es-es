---
title: sp_MSchange_distribution_agent_properties (T-SQL)
description: Describe el sp_MSchange_distribution_agent_properties procedimiento almacenado que se usa para cambiar las propiedades de la Agente de distribución de una topología de Replicación de SQL Server.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_MSchange_distribution_agent_properties
- sp_MSchange_distribution_agent_properties_TSQL
helpviewer_keywords:
- sp_MSchange_distribution_agent_properties
ms.assetid: 7dac5e68-bf84-433a-a531-66921f35126f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 37bb9e7af79fbc3ef99febd84cce78eb8db8a557
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195458"
---
# <a name="sp_mschange_distribution_agent_properties-transact-sql"></a>sp_MSchange_distribution_agent_properties (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cambia las propiedades de un trabajo Agente de distribución que se ejecuta en un [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] distribuidor de o una versión posterior. Este procedimiento almacenado se utiliza para cambiar las propiedades cuando el publicador se ejecuta en una instancia de [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)]. Este procedimiento almacenado se ejecuta en el distribuidor de la base de datos de distribución.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_MSchange_distribution_agent_properties [ @publisher = ] 'publisher'  
        , [ @publisher_db = ] 'publisher_db'  
        , [ @publication = ] 'publication'   
        , [ @subscriber = ] 'subscriber'   
        , [ @subscriber_db = ] 'subscriber_db'   
        , [ @property = ] 'property'   
        , [ @value = ] 'value' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publisher = ] 'publisher'` Es el nombre del publicador. *Publisher* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @publisher_db = ] 'publisher_db'` Es el nombre de la base de datos de publicación. *publisher_db* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @publication = ] 'publication'` Es el nombre de la publicación. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @subscriber = ] 'subscriber'` Es el nombre del suscriptor. *Subscriber* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @subscriber_db = ] 'subscriber_db'` Es el nombre de la base de datos de suscripciones. *subscriber_db* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @property = ] 'property'` Es la propiedad de la publicación que se va a cambiar. *Property* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @value = ] 'value'` Es el nuevo valor de propiedad. el *valor* es **nvarchar (524)** y su valor predeterminado es NULL.  
  
 Esta tabla describe las propiedades del trabajo del Agente de distribución que se pueden cambiar y las restricciones en los valores de esas propiedades.  
  
|Propiedad|Value|Descripción|  
|--------------|-----------|-----------------|  
|**distrib_job_login**||Inicio de sesión de la cuenta de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows en la que se ejecuta el agente.|  
|**distrib_job_password**||Contraseña de la cuenta de Windows en la que se ejecuta el trabajo del agente.|  
|**subscriber_catalog**||Catálogo que debe utilizarse al establecer una conexión con el proveedor OLE DB. *Esta propiedad solo es válida para* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *Suscriptores.*|  
|**subscriber_datasource**||Nombre del origen de datos tal y como lo entiende el proveedor OLE DB. *Esta propiedad solo es válida para* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *Suscriptores.*|  
|**subscriber_location**||Ubicación de la base de datos tal y como la interpreta el proveedor OLE DB. *Esta propiedad solo es válida para* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *Suscriptores.*|  
|**subscriber_login**||Inicio de sesión que se debe utilizar al conectarse a un suscriptor para sincronizar la suscripción.|  
|**subscriber_password**||Contraseña del suscriptor.<br /><br /> [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)]|  
|**subscriber_provider**||Identificador de programación único (PROGID) mediante el cual se registra el proveedor OLE DB para los orígenes de datos que no son de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. *Esta propiedad solo es válida para* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *Suscriptores.*|  
|**subscriber_providerstring**||Cadena de conexión específica del proveedor OLE DB que identifica el origen de datos. *Esta propiedad solo es válida para suscriptores que no sean de SQL Server.*|  
|**subscriber_security_mode**|**1**|Autenticación de Windows.<br /><br /> [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]|  
||**0**|Autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**subscriber_type**|**0**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Suscriptor|  
||**1**|Servidor del origen de datos ODBC|  
||**3**|Proveedor OLE DB|  
|**subscriptionstreams**||Indica el número de conexiones permitidas por Agente de distribución para aplicar lotes de cambios de forma paralela a un suscriptor. *No es* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] compatible con *Suscriptores, publicadores de Oracle o suscripciones punto a punto.*|  
  
> [!NOTE]  
>  Después de cambiar un inicio de sesión o una contraseña de agente, debe detener y reiniciar el agente para que el cambio surta efecto.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_MSchange_distribution_agent_properties** se utiliza en la replicación de instantáneas y en la replicación transaccional.  
  
 Cuando el publicador se ejecuta en una instancia de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] o una versión posterior, debe utilizar [sp_changesubscription](../../relational-databases/system-stored-procedures/sp-changesubscription-transact-sql.md) para cambiar las propiedades de un trabajo de agente de mezcla que sincroniza una suscripción de extracción que se ejecuta en el distribuidor.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** en el distribuidor pueden ejecutar **sp_MSchange_distribution_agent_properties**.  
  
## <a name="see-also"></a>Consulte también  
 [sp_addpushsubscription_agent &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addpushsubscription-agent-transact-sql.md)   
 [sp_addsubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md)  
  
  
