---
title: sp_MSchange_logreader_agent_properties (T-SQL)
description: Describe el sp_MSchange_logreader_agent_properties procedimiento almacenado que se usa para cambiar las propiedades de la Agente de registro del LOG de una topología de Replicación de SQL Server.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_MSchange_logreader_agent_properties_TSQL
- sp_MSchange_logreader_agent_properties
helpviewer_keywords:
- sp_MSchange_logreader_agent_properties
ms.assetid: 925df9d3-a041-4046-8e17-c47f40edb86d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 84508c1437ae6ef51f1904dfbd414b0e373b32e7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195452"
---
# <a name="sp_mschange_logreader_agent_properties-transact-sql"></a>sp_MSchange_logreader_agent_properties (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cambia las propiedades de un trabajo Agente de registro del LOG que se ejecuta en un [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] distribuidor de o una versión posterior. Este procedimiento almacenado se utiliza para cambiar las propiedades cuando el publicador se ejecuta en una instancia de [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)]. Este procedimiento almacenado se ejecuta en el distribuidor de la base de datos de distribución.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_MSchange_logreader_agent_properties [ @publisher = ] 'publisher'  
        , [ @publisher_db = ] 'publisher_db'  
        , [ @publisher_security_mode = ] publisher_security_mode  
        , [ @publisher_login = ] 'publisher_login'  
        , [ @publisher_password = ] 'publisher_password'   
        , [ @job_login = ] 'job_login'  
        , [ @job_password = ] 'job_password'  
        , [ @publisher_type = ] 'publisher_type'  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publisher = ] 'publisher'` Es el nombre del publicador. *Publisher* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @publisher_db = ] 'publisher_db'` Es el nombre de la base de datos de publicación. *publisher_db* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @publisher_security_mode = ] publisher_security_mode` Es el modo de seguridad que utiliza el agente al conectarse al publicador. *publisher_security_mode* es **smallint** y no tiene ningún valor predeterminado.  
  
 **0** especifica [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticación.  
  
 **1** especifica la autenticación de Windows.  
  
`[ @publisher_login = ] 'publisher_login'` Es el inicio de sesión que se usa al conectarse al publicador. *publisher_login* es de **tipo sysname** y no tiene ningún valor predeterminado. se debe especificar *publisher_login* cuando *publisher_security_mode* es **0**. Si *publisher_login* es NULL y *publisher_security_mode* es **1**, se utilizará la cuenta de Windows especificada en *job_login* al conectarse al publicador.  
  
`[ @publisher_password = ] 'publisher_password'` Es la contraseña que se usa al conectarse al publicador. *publisher_password* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @job_login = ] 'job_login'` Es el inicio de sesión de la cuenta de Windows con la que se ejecuta el agente. *job_login* es de tipo **nvarchar (257)** y no tiene ningún valor predeterminado. *No se puede cambiar para un no* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *publicador.*  
  
`[ @job_password = ] 'job_password'` Es la contraseña de la cuenta de Windows con la que se ejecuta el agente. *job_password* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @publisher_type = ] 'publisher_type'` Especifica el tipo de publicador cuando el publicador no se está ejecutando en una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *publisher_type* es de **tipo sysname** y puede tener uno de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**MSSQLSERVER**|Especifica un publicador de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**ORACLE**|Especifica un publicador estándar de Oracle.|  
|**ORACLE GATEWAY**|Especifica un publicador de puerta de enlace de Oracle.|  
  
 Para obtener más información acerca de las diferencias entre un publicador de Oracle y un publicador de puerta de enlace de Oracle, consulte [información general de publicación de Oracle](../../relational-databases/replication/non-sql/oracle-publishing-overview.md).  
  
## <a name="remarks"></a>Observaciones  
 **sp_MSchange_logreader_agent_properties** se utiliza en la replicación transaccional.  
  
 Debe especificar todos los parámetros al ejecutar **sp_MSchange_logreader_agent_properties**. Ejecute [sp_helplogreader_agent &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helplogreader-agent-transact-sql.md) para devolver las propiedades actuales del trabajo de agente de registro del log.  
  
 Después de cambiar un inicio de sesión o una contraseña de agente, debe detener y reiniciar el agente para que el cambio surta efecto.  
  
 Cuando el publicador se ejecuta en una instancia de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] o una versión posterior, debe utilizar [sp_changelogreader_agent](../../relational-databases/system-stored-procedures/sp-changelogreader-agent-transact-sql.md) para cambiar las propiedades de la agente de registro del log.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** en el distribuidor pueden ejecutar **sp_MSchange_logreader_agent_properties**.  
  
## <a name="see-also"></a>Consulte también  
 [sp_addlogreader_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlogreader-agent-transact-sql.md)  
  
  
