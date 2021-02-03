---
description: CREATE SERVER ROLE (Transact-SQL)
title: CREATE SERVER ROLE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/02/2016
ms.prod: sql
ms.prod_service: pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SERVER_ROLE_TSQL
- CREATE SERVER ROLE
- SERVER ROLE
- CREATE_SERVER_ROLE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- SERVER ROLE
- SERVER ROLE, CREATE
- CREATE SERVER ROLE statement
- ROLE
- user-defined server roles [SQL Server]
- roles, server
ms.assetid: 30c92f80-f7f6-4a84-ae89-16e69add0de6
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: cfaf0335c2ba61e093764352b16847f0f96b295c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192652"
---
# <a name="create-server-role-transact-sql"></a>CREATE SERVER ROLE (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crea un nuevo rol de servidor definido por el usuario.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql  
CREATE SERVER ROLE role_name [ AUTHORIZATION server_principal ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *role_name*  
 Es el nombre del rol de servidor que se va a crear.  
  
 AUTHORIZATION *server_principal*  
 Es el inicio de sesión que será propietario del nuevo rol de servidor. Si no se especifica un inicio de sesión, el rol de servidor será propiedad del inicio de sesión que ejecute CREATE SERVER ROLE.  
  
## <a name="remarks"></a>Comentarios  
 Los roles de servidor son elementos protegibles en el nivel de servidor. Después de crear un rol de servidor, configure los permisos de nivel de servidor del rol utilizando GRANT, DENY y REVOKE. Para agregar inicios de sesión a un rol de servidor y quitarlos, use [ALTER SERVER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-role-transact-sql.md). Para quitar un rol de servidor, utilice [DROP SERVER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-server-role-transact-sql.md). Para obtener más información, vea [sys.server_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md).  
  
 Puede ver los roles de servidor consultando las vistas de catálogo [sys.server_role_members](../../relational-databases/system-catalog-views/sys-server-role-members-transact-sql.md) y [sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md).  
  
 No se puede conceder permisos a los roles de servidor en elementos protegibles de nivel de base de datos. Para crear roles de base de datos, vea [CREATE ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-role-transact-sql.md).  
  
 Para más información acerca de cómo diseñar un sistema de permisos, consulte [Getting Started with Database Engine Permissions](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md).  
  
## <a name="permissions"></a>Permisos  
 Se debe disponer del permiso CREATE SERVER CONTROL o pertenecer al rol fijo de servidor sysadmin.  
  
 También se requiere el permiso IMPERSONATE en *server_principal* para los inicios de sesión, el permiso ALTER para los roles de servidor que se han usado como *server_principal* o la pertenencia a un grupo de Windows que se ha usado como server_principal.  
  
 Esto desencadenará el evento Audit Server Principal Management con el tipo de objeto establecido en el rol de servidor y el tipo de evento que se va a agregar.  
  
 Cuando se utiliza la opción AUTHORIZATION para asignar la propiedad de roles de servidor, también se requieren los siguientes permisos:  
  
-   La asignación de la propiedad de un rol de servidor a otro inicio de sesión requiere el permiso IMPERSONATE para ese inicio de sesión.  
  
-   La asignación de la propiedad de un rol de servidor a otro rol de servidor requiere la pertenencia al rol de servidor receptor o el permiso ALTER para ese rol de servidor.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-creating-a-server-role-that-is-owned-by-a-login"></a>A. Crear un rol de servidor que sea propiedad de un inicio de sesión  
 En el siguiente ejemplo se crea el rol de servidor `buyers` que es propiedad del inicio de sesión `BenMiller`.  
  
```sql  
USE master;  
CREATE SERVER ROLE buyers AUTHORIZATION BenMiller;  
GO  
```  
  
### <a name="b-creating-a-server-role-that-is-owned-by-a-fixed-server-role"></a>B. Crear un rol de servidor que sea propiedad de un rol fijo de servidor  
 En el siguiente ejemplo se crea el rol de servidor `auditors` que es propiedad del rol fijo de servidor `securityadmin`.  
  
```sql  
USE master;  
CREATE SERVER ROLE auditors AUTHORIZATION securityadmin;  
GO  
```  
  
## <a name="see-also"></a>Vea también  
 [DROP SERVER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-server-role-transact-sql.md)   
 [Entidades de seguridad &#40;motor de base de datos&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [sp_addrolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)   
 [sys.database_role_members &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-role-members-transact-sql.md)   
 [sys.database_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)   
 [Introducción a los permisos de los motores de bases de datos](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)  
  
  
