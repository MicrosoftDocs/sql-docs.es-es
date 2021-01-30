---
description: sp_link_publication (Transact-SQL)
title: sp_link_publication (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_link_publication_TSQL
- sp_link_publication
helpviewer_keywords:
- sp_link_publication
ms.assetid: 1945ed24-f9f1-4af6-94ca-16d8e864706e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b6a6e0b057f0d01c838aa5ac7b57143221fec13c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210774"
---
# <a name="sp_link_publication-transact-sql"></a>sp_link_publication (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Establece la información de configuración y de seguridad que utilizan los desencadenadores de sincronización de las suscripciones de actualización inmediata al conectar con el publicador. Este procedimiento almacenado se ejecuta en el suscriptor de la base de datos de suscripciones.  
  
> [!IMPORTANT]
>  Al configurar un publicador con un distribuidor remoto, los valores suministrados para todos los parámetros, incluidos *job_login* y *job_password*, se envían al distribuidor como texto sin formato. Antes de ejecutar este procedimiento almacenado, se recomienda cifrar la conexión entre el publicador y su distribuidor remoto. Para obtener más información, vea [Habilitar conexiones cifradas en el motor de base de datos &#40;Administrador de configuración de SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
> 
> [!IMPORTANT]
>  En determinadas condiciones, este procedimiento almacenado puede producir un error si el suscriptor ejecuta el [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 1 o posterior y el publicador ejecuta una versión anterior. Si el procedimiento almacenado genera un error en este escenario, actualice el publicador al Service Pack 1 de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] o posterior.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_link_publication [ @publisher = ] 'publisher'   
        , [ @publisher_db = ] 'publisher_db'   
        , [ @publication = ] 'publication'   
        , [ @security_mode = ] security_mode  
    [ , [ @login = ] 'login' ]  
    [ , [ @password = ]'password' ]  
    [ , [ @distributor = ] 'distributor' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publisher = ] 'publisher'` Es el nombre del publicador al que se va a vincular. *Publisher* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @publisher_db = ] 'publisher_db'` Es el nombre de la base de datos del publicador a la que se va a vincular. *publisher_db* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @publication = ] 'publication'` Es el nombre de la publicación a la que se va a vincular. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @security_mode = ] security_mode` Es el modo de seguridad utilizado por el suscriptor para conectarse a un publicador remoto para la actualización inmediata. *security_mode* es de **tipo int** y puede tener uno de estos valores. [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
|Value|Descripción|  
|-----------|-----------------|  
|**0**|Usa [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] la autenticación con el inicio de sesión especificado en este procedimiento almacenado como *Inicio de sesión* y *contraseña*.<br /><br /> Nota: en versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , esta opción se usaba para especificar una llamada a procedimiento remoto (RPC) dinámica.|  
|**1**|Utiliza el contexto de seguridad (autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o autenticación de Windows) del usuario que realiza el cambio en el suscriptor.<br /><br /> Nota: esta cuenta también debe existir en el publicador con privilegios suficientes. Al usar la autenticación de Windows, se debe admitir la delegación de cuentas de seguridad.|  
|**2**|Utiliza un inicio de sesión de servidor vinculado definido por el usuario existente creado mediante **sp_link_publication**.|  
  
`[ @login = ] 'login'` Es el inicio de sesión. *login* es de tipo **sysname** y su valor predeterminado es NULL. Este parámetro debe especificarse cuando *security_mode* es **0**.  
  
`[ @password = ] 'password'` Es la contraseña. *password* es de **tipo sysname y su** valor predeterminado es NULL. Este parámetro debe especificarse cuando *security_mode* es **0**.  
  
`[ @distributor = ] 'distributor'` Es el nombre del distribuidor. *Distributor* es de **tipo sysname y su** valor predeterminado es NULL.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_link_publication** se usa en las suscripciones de actualización inmediata en la replicación transaccional.  
  
 **sp_link_publication** puede usarse para las suscripciones de inserción y extracción. Se puede llamar antes o después de haber creado la suscripción. Una entrada se inserta o se actualiza en el MSsubscription_properties &#40;tabla del sistema de [Transact-SQL&#41;](../../relational-databases/system-tables/mssubscription-properties-transact-sql.md) .  
  
 En el caso de las suscripciones de inserción, la entrada se puede limpiar mediante [sp_subscription_cleanup &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-subscription-cleanup-transact-sql.md). En el caso de las suscripciones de extracción, la entrada se puede limpiar [sp_droppullsubscription &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-droppullsubscription-transact-sql.md) o [sp_subscription_cleanup &#40;transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-subscription-cleanup-transact-sql.md). También puede llamar a **sp_link_publication** con una contraseña nula para borrar la entrada del MSsubscription_properties &#40;tabla del sistema de [Transact-SQL&#41;](../../relational-databases/system-tables/mssubscription-properties-transact-sql.md) para cuestiones de seguridad.  
  
 El modo predeterminado que utiliza un suscriptor de actualización inmediata cuando se conecta al publicador no permite una conexión mediante la autenticación de Windows. Para conectar con un modo de autenticación de Windows, se deberá configurar un servidor vinculado como publicador, y el suscriptor de actualización inmediata debe utilizar esta conexión cuando actualice el suscriptor. Esto requiere que el **sp_link_publication** se ejecute con *security_mode*  =  **2**. Al usar la autenticación de Windows, se debe admitir la delegación de cuentas de seguridad.  
  
## <a name="example"></a>Ejemplo  
 [!code-sql[HowTo#sp_addtranpullsubscriptionagent_failover](../../relational-databases/replication/codesnippet/tsql/sp-link-publication-tran_1.sql)]  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** pueden ejecutar **sp_link_publication**.  
  
## <a name="see-also"></a>Consulte también  
 [sp_droppullsubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-droppullsubscription-transact-sql.md)   
 [sp_helpsubscription_properties &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpsubscription-properties-transact-sql.md)   
 [sp_subscription_cleanup &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-subscription-cleanup-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
