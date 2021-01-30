---
description: sp_addremotelogin (Transact-SQL)
title: sp_addremotelogin (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_addremotelogin_TSQL
- sp_addremotelogin
dev_langs:
- TSQL
helpviewer_keywords:
- sp_addremotelogin
ms.assetid: 71b7cd36-a17d-4b12-b102-10aeb0f9268b
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 13b935d91772d70d58b38f9f995341d81e04388f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165187"
---
# <a name="sp_addremotelogin-transact-sql"></a>sp_addremotelogin (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Agrega un nuevo identificador de inicio de sesión remoto en el servidor local. Esto permite a los servidores remotos conectarse y ejecutar llamadas a procedimientos remotos.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepNextDontUse](../../includes/ssnotedepnextdontuse-md.md)] Utilice servidores vinculados y procedimientos almacenados de servidores vinculados en su lugar.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_addremotelogin [ @remoteserver = ] 'remoteserver'   
     [ , [ @loginame = ] 'login' ]   
     [ , [ @remotename = ] 'remote_name' ]  
```  
  
## <a name="arguments"></a>Argumentos  
 [ @remoteserver **=** ] **'**_ServidorRemoto_**'**  
 Es el nombre del servidor remoto al que se aplica el inicio de sesión remoto. *RemoteServer* es de **tipo sysname** y no tiene ningún valor predeterminado. Si solo se especifica *RemoteServer* , todos los usuarios de *RemoteServer* se asignan a los inicios de sesión existentes con el mismo nombre en el servidor local. El servidor debe ser un servidor conocido por el servidor local. Se agrega con sp_addserver. Cuando los usuarios de *RemoteServer* se conectan al servidor local que ejecuta [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para ejecutar un procedimiento almacenado remoto, se conectan como el inicio de sesión local que coincide con su propio inicio de sesión en *ServidorRemoto*. *RemoteServer* es el servidor que inicia la llamada a procedimiento remoto.  
  
 [ @loginame **=** ] **'**_Inicio de sesión_**'**  
 Es el identificador de inicio de sesión del usuario en la instancia local de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. *login* es de tipo **sysname** y su valor predeterminado es NULL. el *Inicio de sesión* ya debe existir en la instancia local de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Si se especifica *login* , todos los usuarios de *RemoteServer* se asignan a ese inicio de sesión local específico. Cuando los usuarios de *RemoteServer* se conectan a la instancia local de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para ejecutar un procedimiento almacenado remoto, se conectan como *Inicio de sesión*.  
  
 [ @remotename **=** ] **'**_remote_name_**'**  
 Es el identificador de inicio de sesión del usuario en el servidor remoto. *remote_name* es de **tipo sysname y su** valor predeterminado es NULL. *remote_name* debe existir en *RemoteServer*. Si se especifica *remote_name* , el *remote_name* de usuario específico se asigna al *Inicio de sesión* en el servidor local. Cuando *remote_name* en *RemoteServer* se conecta a la instancia local de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para ejecutar un procedimiento almacenado remoto, se conecta como *Inicio de sesión*. El identificador de inicio de sesión de *remote_name* puede ser diferente del identificador de inicio de sesión del servidor remoto, *Inicio de sesión*.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="remarks"></a>Observaciones  
 Para ejecutar consultas distribuidas, utilice sp_addlinkedsrvlogin.  
  
 sp_addremotelogin no se puede utilizar en una transacción definida por el usuario.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros de los roles fijos de servidor sysadmin y securityadmin pueden ejecutar sp_addremotelogin.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-mapping-one-to-one"></a>A. Asignar uno a uno  
 En el siguiente ejemplo se asignan nombres remotos a nombres locales cuando el servidor remoto `ACCOUNTS` y el servidor local tienen los mismos inicios de sesión de usuario.  
  
```  
EXEC sp_addremotelogin 'ACCOUNTS';  
```  
  
### <a name="b-mapping-many-to-one"></a>B. Asignar varios a uno  
 En el siguiente ejemplo se crea una entrada que asigna todos los usuarios del servidor remoto `ACCOUNTS` al Id. de inicio de sesión local `Albert`.  
  
```  
EXEC sp_addremotelogin 'ACCOUNTS', 'Albert';  
```  
  
### <a name="c-using-explicit-one-to-one-mapping"></a>C. Usar una asignación uno a uno explícita  
 En el siguiente ejemplo se asigna un inicio de sesión remoto desde el usuario remoto `Chris` en el servidor remoto `ACCOUNTS` al usuario local `salesmgr`.  
  
```  
EXEC sp_addremotelogin 'ACCOUNTS', 'salesmgr', 'Chris';  
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_addlinkedsrvlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedsrvlogin-transact-sql.md)   
 [sp_addlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlogin-transact-sql.md)   
 [sp_addserver &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addserver-transact-sql.md)   
 [sp_dropremotelogin &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropremotelogin-transact-sql.md)   
 [sp_grantlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-grantlogin-transact-sql.md)   
 [sp_helpremotelogin &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpremotelogin-transact-sql.md)   
 [sp_helpserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpserver-transact-sql.md)   
 [sp_remoteoption &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-remoteoption-transact-sql.md)   
 [sp_revokelogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-revokelogin-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
