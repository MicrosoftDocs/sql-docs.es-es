---
title: Permisos de entidad de seguridad de base de datos DENY
description: Deniega permisos a un usuario de base de datos, un rol de base de datos o un rol de aplicación.
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 05/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- database roles [SQL Server], permissions
- denying permissions [SQL Server], database roles
- denying permissions [SQL Server], database users
- permissions [SQL Server], database roles
- DENY statement, database roles
- database user permissions [SQL Server]
- permissions [SQL Server], application roles
- permissions [SQL Server], database users
- database permissions [SQL Server], denying
- DENY statement, application roles
- DENY statement, database users
- denying permissions [SQL Server], application roles
- application roles [SQL Server], permissions
ms.assetid: e2429a5d-e9be-4c05-be20-414d1038a63a
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: ff2c2feec318fb8bc53d3218ca81adcc6a3aaf5a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177737"
---
# <a name="deny-database-principal-permissions-transact-sql"></a>DENY (permisos de entidad de seguridad de base de datos de Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Deniega los permisos concedidos para un usuario de base de datos, un rol de base de datos o un rol de aplicación en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
DENY permission [ ,...n ]    
    ON   
    {  [ USER :: database_user ]  
     | [ ROLE :: database_role ]  
     | [ APPLICATION ROLE :: application_role ]  
    }  
    TO <database_principal> [ ,...n ]  
      [ CASCADE ]  
      [ AS <database_principal> ]  
  
<database_principal> ::=  
    Database_user   
  | Database_role   
  | Application_role   
  | Database_user_mapped_to_Windows_User   
  | Database_user_mapped_to_Windows_Group   
  | Database_user_mapped_to_certificate   
  | Database_user_mapped_to_asymmetric_key   
  | Database_user_with_no_login   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *permission*  
 Especifica un permiso que se puede denegar en la entidad de seguridad de base de datos. Para obtener una lista de permisos, vea la sección Comentarios que se muestra posteriormente en este tema.  
  
 USER ::*database_user*  
 Especifica la clase y nombre del usuario para el que se deniega el permiso. El calificador de ámbito ( **::** ) es obligatorio.  
  
 ROLE ::*database_role*  
 Especifica la clase y el nombre del rol para el que se deniega el permiso. El calificador de ámbito ( **::** ) es obligatorio.  
  
 APPLICATION ROLE ::*application_role*  
 **Se aplica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]  
  
 Especifica la clase y el nombre del rol de aplicación para el que se deniega el permiso. El calificador de ámbito ( **::** ) es obligatorio.  
  
 CASCADE  
 Indica que el permiso que se va a denegar también se denegará a otras entidades de seguridad a las que esta entidad de seguridad ha concedido permisos.  
  
 AS \<database_principal>.  
 Especifica una entidad de seguridad de la que la entidad de seguridad que ejecuta esta consulta deriva su derecho de revocar el permiso.  
  
 *Database_user*  
 Especifica un usuario de base de datos.  
  
 *Database_role*  
 Especifica un rol de base de datos.  
  
 *Application_role*  
 **Se aplica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]  
  
 Especifica un rol de aplicación.  
  
 *Database_user_mapped_to_Windows_User*  
 Especifica un usuario de base de datos asignado a un usuario de Windows.  
  
 *Database_user_mapped_to_Windows_Group*  
 Especifica un usuario de base de datos asignado a un grupo de Windows.  
  
 *Database_user_mapped_to_certificate*  
  Especifica un usuario de base de datos asignado a un certificado.  
  
 *Database_user_mapped_to_asymmetric_key*  
  Especifica un usuario de base de datos asignado a una clave asimétrica.  
  
 *Database_user_with_no_login*  
 Especifica un usuario de base de datos sin entidad de seguridad de servidor correspondiente.  
  
## <a name="remarks"></a>Observaciones  
  
## <a name="database-user-permissions"></a>Permisos de usuario de base de datos  
 Un usuario de base de datos es un elemento protegible de nivel de base de datos que contiene la base de datos que es su entidad primaria en la jerarquía de permisos. La mayoría de permisos limitados y específicos que se pueden denegar en un usuario de base de datos se muestran en la siguiente tabla, junto con permisos más generales que los incluyen por implicación.  
  
|Permiso de usuario de base de datos|Implícito en el permiso de usuario de base de datos|Implícito en el permiso de base de datos|  
|------------------------------|-----------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|IMPERSONATE|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY USER|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="database-role-permissions"></a>Permisos de rol de base de datos  
 Un rol de base de datos es un elemento protegible de nivel de base de datos contenido en la base de datos que es su entidad primaria en la jerarquía de permisos. Los permisos más específicos y limitados que se pueden denegar para un rol de base de datos se muestran en la siguiente tabla, junto con permisos más generales que los incluyen por implicación.  
  
|Permiso de rol de base de datos|Implícito en el permiso de rol de base de datos|Implícito en el permiso de base de datos|  
|------------------------------|-----------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY ROLE|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="application-role-permissions"></a>Permisos de rol de aplicación  
 Un rol de aplicación es un elemento protegible de nivel de base de datos que contiene la base de datos que es su entidad primaria en la jerarquía de permisos. Los permisos más específicos y limitados que se pueden denegar en un rol de aplicación se muestran en la siguiente tabla, junto con permisos más generales que los incluyen por implicación.  
  
|Permiso de rol de aplicación|Implícito en el permiso de rol de aplicación|Implícito en el permiso de base de datos|  
|---------------------------------|--------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY APPLICATION ROLE|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso CONTROL en la entidad de seguridad especificada o un permiso superior que implique el permiso CONTROL.  
  
 Los receptores del permiso CONTROL para una base de datos, como los miembros del rol fijo de base de datos db_owner, pueden denegar los permisos para cualquier elemento protegible en la base de datos.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-denying-control-permission-on-a-user-to-another-user"></a>A. Denegar el permiso CONTROL en un usuario para otro usuario  
 En el siguiente ejemplo se deniega el permiso `CONTROL` para el usuario [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] de `Wanida` al usuario `RolandX`.  
  
```sql 
USE AdventureWorks2012;  
DENY CONTROL ON USER::Wanida TO RolandX;  
GO  
```  
  
### <a name="b-denying-view-definition-permission-on-a-role-to-a-user-to-which-it-was-granted-with-grant-option"></a>B. Denegar el permiso VIEW DEFINITION para un rol a un usuario para el que se concedió con GRANT OPTION  
 En el siguiente ejemplo se deniega el permiso `VIEW DEFINITION` para el rol [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] de `SammamishParking` al usuario de base de datos `JinghaoLiu`. Se especifica la opción `CASCADE` porque al usuario `JinghaoLiu` se le concedió el permiso VIEW DEFINITION con WITH GRANT OPTION.  
  
```sql  
USE AdventureWorks2012;  
DENY VIEW DEFINITION ON ROLE::SammamishParking   
    TO JinghaoLiu CASCADE;  
GO  
```  
  
### <a name="c-denying-impersonate-permission-on-a-user-to-an-application-role"></a>C. Denegar el permiso IMPERSONATE para un usuario a un rol de aplicación  
 En el ejemplo siguiente se deniega el permiso `IMPERSONATE` para el usuario `HamithaL` al rol de aplicación `AccountsPayable17` de [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
**Se aplica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]  
  
```sql  
USE AdventureWorks2012;  
DENY IMPERSONATE ON USER::HamithaL TO AccountsPayable17;  
GO    
```  
  
## <a name="see-also"></a>Consulte también  
 [GRANT &#40;permisos de entidad de seguridad de base de datos de Transact-SQL&#41;](../../t-sql/statements/grant-database-principal-permissions-transact-sql.md)   
 [REVOKE &#40;permisos de entidad de seguridad de base de datos de Transact-SQL&#41;](../../t-sql/statements/revoke-database-principal-permissions-transact-sql.md)   
 [sys.database_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)   
 [sys.database_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md)   
 [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)   
 [CREATE APPLICATION ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-application-role-transact-sql.md)   
 [CREATE ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-role-transact-sql.md)   
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [Permisos &#40;motor de base de datos&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Entidades de seguridad &#40;motor de base de datos&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  
