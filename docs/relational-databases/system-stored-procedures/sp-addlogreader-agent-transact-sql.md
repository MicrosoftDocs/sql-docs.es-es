---
description: sp_addlogreader_agent (Transact-SQL)
title: sp_addlogreader_agent (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addlogreader_agent
- sp_addlogreader_agent_TSQL
helpviewer_keywords:
- sp_addlogreader_agent
ms.assetid: d83096b9-96ee-4789-bde0-940d4765b9ed
author: mashamsft
ms.author: mathoma
ms.openlocfilehash: a511c4f47f84a8f083dbab86bf1d851762142d90
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202368"
---
# <a name="sp_addlogreader_agent-transact-sql"></a>sp_addlogreader_agent (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Agrega un Agente de registro del LOG a una base de datos dada. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación.  
  
> [!IMPORTANT]  
>  Al configurar un publicador con un distribuidor remoto, los valores suministrados para todos los parámetros, incluidos *job_login* y *job_password*, se envían al distribuidor como texto sin formato. Antes de ejecutar este procedimiento almacenado, se recomienda cifrar la conexión entre el publicador y su distribuidor remoto. Para obtener más información, vea [Habilitar conexiones cifradas en el motor de base de datos &#40;Administrador de configuración de SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_addlogreader_agent [ @job_login = ] 'job_login'  
        , [ @job_password = ] 'job_password'  
    [ , [ @job_name = ] 'job_name' ]  
    [ , [ @publisher_security_mode = ] publisher_security_mode ]  
    [ , [ @publisher_login = ] 'publisher_login' ]  
    [ , [ @publisher_password = ] 'publisher_password' ]   
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @job_login = ] 'job_login'` Es el inicio de sesión de la [!INCLUDE[msCoName](../../includes/msconame-md.md)] cuenta de Windows con la que se ejecuta el agente. *job_login* es de tipo **nvarchar (257)** y su valor predeterminado es NULL. Esta cuenta de Windows siempre se utiliza para conexiones del agente con el distribuidor.  
  
> [!NOTE]
>  En el caso de los publicadores que no son de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , debe ser el mismo inicio de sesión especificado en [sp_adddistpublisher &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md).  
  
`[ @job_password = ] 'job_password'` Es la contraseña de la cuenta de Windows con la que se ejecuta el agente. *job_password* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> [!IMPORTANT]  
>  No almacene información de autenticación en archivos de script. Para una mayor seguridad, los nombres de inicio de sesión y las contraseñas se deben proporcionar en tiempo de ejecución.  
  
`[ @job_name = ] 'job_name'` Es el nombre de un trabajo del agente existente. *job_name* es de **tipo sysname y su** valor predeterminado es NULL. Este parámetro solamente se especifica cuando se inicia el agente a partir de un trabajo existente, en lugar de hacerlo con un trabajo recién creado (el valor predeterminado).  
  
`[ @publisher_security_mode = ] publisher_security_mode` Es el modo de seguridad que utiliza el agente al conectarse al publicador. *publisher_security_mode* es de **smallint** y su valor predeterminado es **1**. **0** especifica [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticación y **1** especifica la autenticación de Windows. Se debe especificar un valor de **0** para los publicadores que no son de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
`[ @publisher_login = ] 'publisher_login'` Es el inicio de sesión que se usa al conectarse al publicador. *publisher_login* es de **tipo sysname y su** valor predeterminado es NULL. se debe especificar *publisher_login* cuando *publisher_security_mode* es **0**. Si *publisher_login* es NULL y *publisher_security_mode* es **1**, se utilizará la cuenta de Windows especificada en *job_login* al conectarse al publicador.  
  
`[ @publisher_password = ] 'publisher_password'` Es la contraseña que se usa al conectarse al publicador. *publisher_password* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> [!IMPORTANT]  
>  No almacene información de autenticación en archivos de script. Para una mayor seguridad, los nombres de inicio de sesión y las contraseñas se deben proporcionar en tiempo de ejecución.  
  
`[ @publisher = ] 'publisher'` Es el nombre del publicador que no es de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *Publisher* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> [!NOTE]  
>  No debe especificar este parámetro para un publicador de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_addlogreader_agent** se utiliza en la replicación transaccional.  
  
 Debe ejecutar **sp_addlogreader_agent** para agregar un agente de registro del log si ha actualizado una base de datos que se ha habilitado para la replicación en esta versión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] antes de que se creara una publicación que usaba la base de datos.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_addlogreader_agent**.  
  
## <a name="example"></a>Ejemplo  
 [!code-sql[HowTo#sp_AddTranPub](../../relational-databases/replication/codesnippet/tsql/sp-addlogreader-agent-tr_1.sql)]  
  
## <a name="see-also"></a>Consulte también  
 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)   
 [sp_addpublication &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md)   
 [sp_changelogreader_agent &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-changelogreader-agent-transact-sql.md)   
 [Procedimientos almacenados de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
