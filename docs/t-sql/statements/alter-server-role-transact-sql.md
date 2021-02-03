---
description: ALTER SERVER ROLE (Transact-SQL)
title: ALTER SERVER ROLE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/16/2020
ms.prod: sql
ms.prod_service: pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_SERVER_ROLE_TSQL
- ALTER SERVER ROLE
dev_langs:
- TSQL
helpviewer_keywords:
- SERVER ROLE, ALTER
- ALTER SERVER ROLE statement
ms.assetid: 7a4db7bb-c442-4e12-9a8a-114da5bc7710
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d0708ee4181ce959b1bc307d9fa29b3c7ae408c4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182152"
---
# <a name="alter-server-role-transact-sql"></a>ALTER SERVER ROLE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-pdw-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-pdw-md.md)]

Cambia la pertenencia de un rol de servidor o cambia el nombre de un rol de servidor definido por el usuario. No se puede cambiar el nombre de los roles fijos de servidor.  
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
-- Syntax for SQL Server  
  
ALTER SERVER ROLE server_role_name   
{  
    [ ADD MEMBER server_principal ]  
  | [ DROP MEMBER server_principal ]  
  | [ WITH NAME = new_server_role_name ]  
} [ ; ]  
```  
  
```syntaxsql
-- Syntax for Parallel Data Warehouse  
  
ALTER SERVER ROLE  server_role_name  ADD MEMBER login;  
  
ALTER SERVER ROLE  server_role_name  DROP MEMBER login;  
```  
  
## <a name="arguments"></a>Argumentos  
*server_role_name*  
Nombre del rol de servidor que se va a cambiar.  
  
ADD MEMBER *server_principal*  
Agrega la entidad de seguridad del servidor especificada al rol de servidor. *server_principal* puede ser un inicio de sesión o un rol de servidor definido por el usuario. *server_principal* no puede ser un rol fijo de servidor, un rol de base de datos ni sa.  
  
DROP MEMBER *server_principal*  
Quita la entidad de seguridad del servidor especificada del rol de servidor. *server_principal* puede ser un inicio de sesión o un rol de servidor definido por el usuario. *server_principal* no puede ser un rol fijo de servidor, un rol de base de datos ni sa.  
  
WITH NAME **=** _new_server_role_name_  
Especifica el nuevo nombre del rol de servidor definido por el usuario. Este nombre no puede existir ya en el servidor.  
  
## <a name="remarks"></a>Observaciones  
El cambio de nombre de un rol de servidor definido por el usuario no cambia el número de identificación, el propietario ni los permisos del rol.  
  
Para cambiar la pertenencia a roles, `ALTER SERVER ROLE` reemplaza sp_addsrvrolemember y sp_dropsrvrolemember. Estos procedimientos almacenados están desusados.  
  
Puede ver los roles de servidor si consulta las vistas de catálogo `sys.server_role_members` y `sys.server_principals`.  
  
Para cambiar el propietario de un rol de servidor definido por el usuario, utilice [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md).  
  
## <a name="permissions"></a>Permisos  
Se necesita el permiso `ALTER ANY SERVER ROLE` en el servidor para cambiar el nombre de un rol de servidor definido por el usuario.  
  
**Roles fijos de servidor**  
  
Para agregar un miembro a un rol fijo de servidor, debe ser miembro de ese rol fijo de servidor o del rol fijo de servidor `sysadmin`.  
  
> [!NOTE]  
>  Los permisos `CONTROL SERVER` y `ALTER ANY SERVER ROLE` no bastan para ejecutar `ALTER SERVER ROLE` para un rol fijo de servidor y el permiso `ALTER` no se puede conceder en un rol fijo de servidor.  
  
**Roles de servidor definidos por el usuario**  
  
Para agregar un miembro a un rol de servidor definido por el usuario, debe ser miembro del rol fijo de servidor `sysadmin` o disponer de los permisos `CONTROL SERVER` o `ALTER ANY SERVER ROLE`. O bien, debe disponer del permiso `ALTER` en ese rol.  
  
> [!NOTE]  
>  A diferencia de los roles fijos de servidor, los miembros de un rol de servidor definido por el usuario no disponen inherentemente de permiso para agregar miembros a ese mismo rol.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-changing-the-name-of-a-server-role"></a>A. Cambiar el nombre de un rol de servidor  
En el siguiente ejemplo se crea un rol de servidor denominado `Product` y, a continuación, se cambia el nombre del rol de servidor a `Production`.  
  
```sql
CREATE SERVER ROLE Product ;  
ALTER SERVER ROLE Product WITH NAME = Production ;  
GO  
```  
  
### <a name="b-adding-a-domain-account-to-a-server-role"></a>B. Agregar una cuenta de dominio a un rol de servidor  
En el siguiente ejemplo se agrega una cuenta de dominio denominada `adventure-works\roberto0` al rol de servidor definido por el usuario denominado `Production`.  
  
```sql
ALTER SERVER ROLE Production ADD MEMBER [adventure-works\roberto0] ;  
```  
  
### <a name="c-adding-a-sql-server-login-to-a-server-role"></a>C. Agregar un inicio de sesión de SQL Server a un rol de servidor  
En el siguiente ejemplo se agrega un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] denominado `Ted` al rol fijo de servidor `diskadmin`.  
  
```sql
ALTER SERVER ROLE diskadmin ADD MEMBER Ted ;  
GO  
```  
  
### <a name="d-removing-a-domain-account-from-a-server-role"></a>D. Quitar una cuenta de dominio de un rol de servidor  
En el siguiente ejemplo se quita una cuenta de dominio denominada `adventure-works\roberto0` del rol de servidor definido por el usuario denominado `Production`.  
  
```sql
ALTER SERVER ROLE Production DROP MEMBER [adventure-works\roberto0] ;  
```  
  
### <a name="e-removing-a-sql-server-login-from-a-server-role"></a>E. Quitar un inicio de sesión de SQL Server de un rol de servidor  
En el siguiente ejemplo se quita el inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] `Ted` del rol fijo de servidor `diskadmin`.  
  
```sql
ALTER SERVER ROLE Production DROP MEMBER Ted ;  
GO  
```  
  
### <a name="f-granting-a-login-the-permission-to-add-logins-to-a-user-defined-server-role"></a>F. Conceder a un inicio de sesión el permiso para agregar inicios de sesión a un rol de servidor definido por el usuario  
En el siguiente ejemplo se permite a `Ted` agregar otros inicio de sesión al rol de servidor definido por el usuario denominado `Production`.  
  
```sql
GRANT ALTER ON SERVER ROLE::Production TO Ted ;  
GO  
```  
  
### <a name="g-to-view-role-membership"></a>G. Para ver la pertenencia al rol  
Para ver la pertenencia al rol, utilice la página **Rol de servidor (miembros)** de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o ejecute la siguiente consulta:  
  
```sql
SELECT SRM.role_principal_id, SP.name AS Role_Name,   
SRM.member_principal_id, SP2.name  AS Member_Name  
FROM sys.server_role_members AS SRM  
JOIN sys.server_principals AS SP  
    ON SRM.Role_principal_id = SP.principal_id  
JOIN sys.server_principals AS SP2   
    ON SRM.member_principal_id = SP2.principal_id  
ORDER BY  SP.name,  SP2.name  
```  
  
## <a name="examples-sspdw"></a>Ejemplos: [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="h-basic-syntax"></a>H. Sintaxis básica  
En el siguiente ejemplo se agrega el inicio de sesión `Anna` al rol fijo de servidor `LargeRC`.  
  
```sql
ALTER SERVER ROLE LargeRC ADD MEMBER Anna;  
```  
  
### <a name="i-remove-a-login-from-a-resource-class"></a>I. Quita un inicio de sesión de una clase de recursos.  
En el siguiente ejemplo se quita el rol de Ana del rol de servidor `LargeRC`.  
  
```sql
ALTER SERVER ROLE LargeRC DROP MEMBER Anna;  
```  
  
## <a name="see-also"></a>Consulte también  
[CREATE SERVER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-server-role-transact-sql.md)   
[DROP SERVER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-server-role-transact-sql.md)   
[CREATE ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-role-transact-sql.md)   
[ALTER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-role-transact-sql.md)   
[DROP ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-role-transact-sql.md)   
[Procedimientos almacenados de seguridad &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
[Funciones de seguridad &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)   
[Entidades de seguridad &#40;motor de base de datos&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
[sys.server_role_members &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-role-members-transact-sql.md)   
[sys.server_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)  
  
