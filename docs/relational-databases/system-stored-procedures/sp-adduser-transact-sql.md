---
description: sp_adduser (Transact-SQL)
title: sp_adduser (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_adduser
- sp_adduser_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_adduser
ms.assetid: 61a40eb4-573f-460c-9164-bd1bbfaf8b25
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: c250837ad65e0b14fc5612eae69be6f93d8f23e6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198372"
---
# <a name="sp_adduser-transact-sql"></a>sp_adduser (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Agrega un nuevo usuario a la base de datos actual.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] En su lugar, use [Create User](../../t-sql/statements/create-user-transact-sql.md) .  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_adduser [ @loginame = ] 'login'   
    [ , [ @name_in_db = ] 'user' ]   
    [ , [ @grpname = ] 'role' ]   
```  
  
## <a name="arguments"></a>Argumentos  
`[ @loginame = ] 'login'` Es el nombre del inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o el inicio de sesión de Windows. *login* es de **tipo sysname** y no tiene ningún valor predeterminado. *login* debe ser un inicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de sesión de o inicio de sesión de Windows existente.  
  
`[ @name_in_db = ] 'user'` Es el nombre del nuevo usuario de la base de datos. *User* es de **tipo sysname y su** valor predeterminado es NULL. Si no se especifica *User* , el nombre del nuevo usuario de la base de datos tiene como valor predeterminado el nombre de *Inicio de sesión* . Al especificar *User* , se asigna al nuevo usuario un nombre en la base de datos diferente del nombre de inicio de sesión de nivel de servidor.  
  
`[ @grpname = ] 'role'` Es el rol de base de datos del que el nuevo usuario se convierte en miembro. *role* es de **tipo sysname y su** valor predeterminado es NULL. *role* debe ser un rol de base de datos válido en la base de datos actual.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_adduser** también creará un esquema con el nombre del usuario.  
  
 Después de agregar un usuario, utilice las instrucciones GRANT, DENY y REVOKE para definir los permisos que controlan las actividades del usuario.  
  
 Use **Sys.server_principals** para mostrar una lista de nombres de inicio de sesión válidos.  
  
 Use **sp_helprole** para mostrar una lista de los nombres de rol válidos. Cuando se especifica un rol, el usuario obtiene automáticamente los permisos definidos para ese rol. Si no se especifica un rol, el usuario obtiene los permisos concedidos al rol **público** predeterminado. Para agregar un usuario a un rol, se debe proporcionar un valor para el *nombre de usuario* . (el *nombre de usuario* puede ser el mismo que el *login_id*).  
  
 El **invitado** del usuario ya existe en cada base de datos. Al agregar el usuario **invitado** , se habilitará este usuario, si estaba deshabilitado anteriormente. De forma predeterminada, el **invitado** del usuario está deshabilitado en las bases de datos nuevas.  
  
 no se puede ejecutar **sp_adduser** dentro de una transacción definida por el usuario.  
  
 No se puede Agregar un usuario **invitado** porque ya existe un usuario **invitado** en cada base de datos. Para habilitar el usuario **invitado** , conceda el permiso Connect de **invitado** como se muestra a continuación:  
  
```  
GRANT CONNECT TO guest;  
GO  
```  
  
## <a name="permissions"></a>Permisos  
 Requiere la propiedad de la base de datos.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-adding-a-database-user"></a>A. Agregar un usuario de base de datos  
 En el siguiente ejemplo se agrega el usuario de base de datos `Vidur` al rol `Recruiting` existente en la base de datos actual, utilizando el inicio de sesión `Vidur` de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] existente.  
  
```  
EXEC sp_adduser 'Vidur', 'Vidur', 'Recruiting';  
```  
  
### <a name="b-adding-a-database-user-with-the-same-login-id"></a>B. Agregar un usuario de base de datos con el mismo Id. de inicio de sesión  
 En el siguiente ejemplo se agrega el usuario `Arvind` a la base de datos para el inicio de sesión `Arvind` de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Este usuario pertenece al rol **público** predeterminado.  
  
```  
EXEC sp_adduser 'Arvind';  
```  
  
### <a name="c-adding-a-database-user-with-a-different-name-than-its-server-level-login"></a>C. Agregar un usuario de base de datos con un nombre diferente de su inicio de sesión de nivel de servidor  
 En el siguiente ejemplo se agrega el inicio de sesión `BjornR` de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a la base de datos actual que tiene un nombre de usuario `Bjorn`, y agrega un usuario de base de datos `Bjorn` al rol de base de datos `Production`.  
  
```  
EXEC sp_adduser 'BjornR', 'Bjorn', 'Production';  
```  
  
## <a name="see-also"></a>Vea también  
 [Procedimientos almacenados de seguridad &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sys.server_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)   
 [sp_addrole &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addrole-transact-sql.md)   
 [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)   
 [sp_dropuser &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropuser-transact-sql.md)   
 [sp_grantdbaccess &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-grantdbaccess-transact-sql.md)   
 [sp_grantlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-grantlogin-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
