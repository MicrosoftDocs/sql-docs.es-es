---
description: sp_dropremotelogin (Transact-SQL)
title: sp_dropremotelogin (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_dropremotelogin
- sp_dropremotelogin_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_dropremotelogin
ms.assetid: 9f097652-a286-40b2-be73-568d77ada698
ms.author: vanto
author: VanMSFT
ms.openlocfilehash: 7b468ff83bffdd581695b9553c636b06fbe6c784
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187032"
---
# <a name="sp_dropremotelogin-transact-sql"></a>sp_dropremotelogin (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Quita un inicio de sesión remoto asignado a un inicio de sesión local que se utiliza para ejecutar procedimientos almacenados remotos en el servidor local que ejecuta [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepNextDontUse](../../includes/ssnotedepnextdontuse-md.md)] En su lugar, use servidores vinculados y procedimientos almacenados de servidores vinculados.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
sp_dropremotelogin [ @remoteserver = ] 'remoteserver'   
     [ , [ @loginame = ] 'login' ]   
     [ , [ @remotename = ] 'remote_name' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @remoteserver = ] 'remoteserver'` Es el nombre del servidor remoto asignado al inicio de sesión remoto que se va a quitar. *RemoteServer* es de **tipo sysname** y no tiene ningún valor predeterminado. *RemoteServer* ya debe existir.  
  
`[ @loginame = ] 'login'` Es el nombre de inicio de sesión opcional en el servidor local que está asociado con el servidor remoto. *login* es de tipo **sysname** y su valor predeterminado es NULL. el *Inicio de sesión* ya debe existir si se especifica.  
  
`[ @remotename = ] 'remote_name'` Es el nombre opcional del inicio de sesión remoto que se asigna al *Inicio de sesión* cuando se inicia sesión desde el servidor remoto. *remote_name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="remarks"></a>Observaciones  
 Si solo se especifica *RemoteServer* , se quitarán del servidor local todos los inicios de sesión remotos del servidor remoto. Si también se especifica *login* , se quitarán del servidor local todos los inicios de sesión remotos de *RemoteServer* asignados a ese inicio de sesión local específico. Si también se especifica *remote_name* , solo se quita del servidor local el inicio de sesión remoto para ese usuario remoto de *RemoteServer* .  
  
 Para agregar usuarios del servidor local, use **sp_addlogin**. Para quitar usuarios del servidor local, use **sp_droplogin**.  
  
 Los inicios de sesión remotos solo son necesarios cuando se usan versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] versión 7.0 y posteriores utilizan en su lugar inicios de sesión de servidores vinculados. Use **sp_addlinkedsrvlogin** y **sp_droplinkedsrvlogin** para agregar y quitar inicios de sesión de servidor vinculado.  
  
 **sp_dropremotelogin** no se puede ejecutar en una transacción definida por el usuario.  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia a los roles fijos de servidor **sysadmin** o **securityadmin** .  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-dropping-all-remote-logins-for-a-remote-server"></a>A. Quitar todos los inicios de sesión remotos de un servidor remoto  
 En el siguiente ejemplo se quita la entrada del servidor remoto `ACCOUNTS`, con lo que se quitan también todas las asignaciones entre inicios de sesión del servidor local e inicios de sesión remotos del servidor remoto.  
  
```sql
EXEC sp_dropremotelogin 'ACCOUNTS';  
```  
  
### <a name="b-dropping-a-login-mapping"></a>B. Quitar una asignación de inicio de sesión  
 En el siguiente ejemplo se quita la entrada que asigna inicios de sesión remotos del servidor remoto `ACCOUNTS` al nombre de inicio de sesión local `Albert`.  
  
```sql
EXEC sp_dropremotelogin 'ACCOUNTS', 'Albert';  
```  
  
### <a name="c-dropping-a-remote-user"></a>C. Quitar un usuario remoto  
 En el siguiente ejemplo se quita el inicio de sesión remoto `Chris` del servidor remoto `ACCOUNTS`, asignado al inicio de sesión local `salesmgr`.  
  
```sql
EXEC sp_dropremotelogin 'ACCOUNTS', 'salesmgr', 'Chris';  
```  
  
## <a name="see-also"></a>Vea también  
 [Procedimientos almacenados de seguridad &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_addlinkedsrvlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedsrvlogin-transact-sql.md)   
 [sp_addlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlogin-transact-sql.md)   
 [sp_addremotelogin &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addremotelogin-transact-sql.md)   
 [sp_addserver &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addserver-transact-sql.md)   
 [sp_droplinkedsrvlogin &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-droplinkedsrvlogin-transact-sql.md)   
 [sp_droplogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droplogin-transact-sql.md)   
 [sp_helpremotelogin &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpremotelogin-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
