---
description: CREATE ROLE (Transact-SQL)
title: CREATE ROLE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE ROLE
- DATABASE ROLE
- ROLE_TSQL
- DATABASE_ROLE_TSQL
- CREATE_ROLE_TSQL
- CREATE DATABASE ROLE
- ROLE
- CREATE_DATABASE_ROLE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- database roles [SQL Server], creating
- CREATE DATABASE ROLE statement
- roles [SQL Server], creating
- CREATE ROLE statement
ms.assetid: b0cd54ad-e81d-4d71-acec-8a6d7261ca08
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 63f7f7157afe656f38160b37f99059e47aab9484
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192712"
---
# <a name="create-role-transact-sql"></a>CREATE ROLE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Crea un rol de base de datos nuevo en la base de datos actual.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql  
CREATE ROLE role_name [ AUTHORIZATION owner_name ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *role_name*  
 Es el nombre del rol que se va a crear.  
  
 AUTHORIZATION *owner_name*  
 Es el usuario o el rol de base de datos que será propietario del nuevo rol. Si no se especifica un usuario, el rol será propiedad del usuario que ejecute CREATE ROLE. El propietario del rol, o cualquier miembro de un rol propietario, puede agregar o quitar miembros del rol.
  
## <a name="remarks"></a>Observaciones  
 Los roles son elementos protegibles de nivel de base de datos. Después de crear un rol, configure los permisos de nivel de base de datos del rol con GRANT, DENY y REVOKE. Para agregar miembros a un rol de base de datos, use [ALTER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-role-transact-sql.md). Para más información, vea [Roles de nivel de base de datos](../../relational-databases/security/authentication-access/database-level-roles.md).  
  
 Los roles de base de datos se pueden ver en las vistas de catálogos sys.database_role_members y sys.database_principals.  
  
 Para más información acerca de cómo diseñar un sistema de permisos, consulte [Getting Started with Database Engine Permissions](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md).  
  
> [!CAUTION]  
>  [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso **CREATE ROLE** en la base de datos o la pertenencia al rol fijo de base de datos **db_securityadmin**. Cuando usa la opción **AUTHORIZATION**, también se necesitan los siguientes permisos:  
  
-   Asignar la propiedad de un rol a otro usuario requiere el permiso IMPERSONATE para ese usuario.  
  
-   Asignar la propiedad de un rol a otro rol requiere la pertenencia al rol receptor o el permiso ALTER para ese rol.  
  
-   Asignar la propiedad de un rol a un rol de aplicación requiere el permiso ALTER para el rol de aplicación.  
  
## <a name="examples"></a>Ejemplos  
En todos estos ejemplos se usa la base de datos AdventureWorks.   

### <a name="a-creating-a-database-role-that-is-owned-by-a-database-user"></a>A. Crear un rol de base de datos propiedad de un usuario de la base de datos  
 En el siguiente ejemplo se crea el rol de base de datos `buyers` que es propiedad del usuario `BenMiller`.  
  
```sql  
CREATE ROLE buyers AUTHORIZATION BenMiller;  
GO  
```  
  
### <a name="b-creating-a-database-role-that-is-owned-by-a-fixed-database-role"></a>B. Crear un rol de base de datos que es propiedad de un rol fijo de base de datos  
 En el siguiente ejemplo se crea el rol de base de datos `auditors` que es propiedad del rol fijo de base de datos `db_securityadmin`.  
  
```sql  
CREATE ROLE auditors AUTHORIZATION db_securityadmin;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Entidades de seguridad &#40;motor de base de datos&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [ALTER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-role-transact-sql.md)   
 [DROP ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-role-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [sp_addrolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)   
 [sys.database_role_members &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-role-members-transact-sql.md)   
 [sys.database_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)   
 [Introducción a los permisos de los motores de bases de datos](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)  
  
  


