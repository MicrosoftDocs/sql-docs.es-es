---
description: GRANT (permisos de tipo de Transact-SQL)
title: GRANT (permisos de tipo de Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- permissions [SQL Server], types
- granting permissions [SQL Server], types
- GRANT statement, types
- type permissions [SQL Server]
ms.assetid: 14bd2fb3-1446-49c0-be87-c6a670317ed0
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: c562aac8a5b0922c235ec8c24d170fdf19c4d088
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180156"
---
# <a name="grant-type-permissions-transact-sql"></a>GRANT (permisos de tipo de Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Concede permisos para un tipo.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
GRANT permission  [ ,...n ] ON TYPE :: [ schema_name . ] type_name  
    TO <database_principal> [ ,...n ]  
    [ WITH GRANT OPTION ]  
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
 Especifica un permiso que se puede conceder para un tipo. Para obtener una lista de permisos, vea la sección Comentarios que se muestra posteriormente en este tema.  
  
 ON TYPE **::** [ _schema_name_ **.** ] *type_name*  
 Especifica el tipo en el que se va a conceder el permiso. El calificador de ámbito ( **::** ) es obligatorio. Si no se especifica *schema_name*, se usa el esquema predeterminado. Si se especifica *schema_name*, se necesita el calificador de ámbito de esquema ( **.** ).  
  
 TO \<database_principal> Especifica la entidad de seguridad para la que se concede el permiso.  
  
 WITH GRANT OPTION  
 Indica que la entidad de seguridad también podrá conceder el permiso especificado a otras entidades de seguridad.  
  
 AS \<database_principal> Especifica una entidad de seguridad desde la que la entidad de seguridad que ejecuta esta consulta deriva su derecho de conceder el permiso.  
  
 *Database_user*  
 Especifica un usuario de base de datos.  
  
 *Database_role*  
 Especifica un rol de base de datos.  
  
 *Application_role*  
**Se aplica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]
  
 Especifica un rol de aplicación.  
  
 *Database_user_mapped_to_Windows_User*  
**Válido para** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.
  
 Especifica un usuario de base de datos asignado a un usuario de Windows.  
  
 *Database_user_mapped_to_Windows_Group*  
**Válido para** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.
  
 Especifica un usuario de base de datos asignado a un grupo de Windows.  
  
 *Database_user_mapped_to_certificate*  
**Válido para** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.
  
 Especifica un usuario de base de datos asignado a un certificado.  
  
 *Database_user_mapped_to_asymmetric_key*  
**Válido para** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.
  
 Especifica un usuario de base de datos asignado a una clave asimétrica.  
  
 *Database_user_with_no_login*  
 Especifica un usuario de base de datos sin entidad de seguridad de servidor correspondiente.  
  
## <a name="remarks"></a>Observaciones  
 Un tipo es un elemento protegible de nivel de esquema que contiene el esquema que es su entidad primaria en la jerarquía de permisos.  
  
> [!IMPORTANT]  
>  Los permisos **GRANT**, **DENY** y **REVOKE** no se aplican a los tipos de sistemas. Se pueden conceder permisos a los tipos definidos por el usuario. Para más información sobre los tipos definidos por el usuario, vea [Working with User-Defined Types in SQL Server](../../relational-databases/clr-integration-database-objects-user-defined-types/working-with-user-defined-types-in-sql-server.md) (Trabajar con tipos definidos por el usuario en SQL Server).  
  
 La mayoría de permisos limitados y específicos que se pueden conceder para un tipo se muestran en la siguiente tabla, junto con permisos más generales que los incluyen por implicación.  
  
|Permiso de tipo|Implicado por el permiso de tipo|Implícito en el permiso del esquema|  
|---------------------|--------------------------------|----------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|Ejecute|CONTROL|Ejecute|  
|REFERENCES|CONTROL|REFERENCES|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>Permisos  
 El otorgante del permiso (o la entidad de seguridad especificada con la opción AS) debe tener el permiso con GRANT OPTION, o un permiso superior que implique el permiso que se va a conceder.  
  
 Si utiliza la opción AS, se aplican los siguientes requisitos adicionales.  
  
|AS|Permiso adicional necesario|  
|--------|------------------------------------|  
|Usuario de la base de datos|Permiso IMPERSONATE en el usuario, pertenencia al rol fijo de base de datos **db_securityadmin**, pertenencia al rol fijo de base de datos **db_owner** o pertenencia al rol fijo de servidor **sysadmin**.|  
|Usuario de la base de datos asignado a un inicio de sesión de Windows|Permiso IMPERSONATE en el usuario, pertenencia al rol fijo de base de datos **db_securityadmin**, pertenencia al rol fijo de base de datos **db_owner** o pertenencia al rol fijo de servidor **sysadmin**.|  
|Usuario de la base de datos asignado a un grupo de Windows|Pertenencia al grupo de Windows, pertenencia al rol fijo de base de datos **db_securityadmin**, pertenencia al rol fijo de base de datos **db_owner** o pertenencia al rol fijo de servidor **sysadmin**.|  
|Usuario de la base de datos asignado a un certificado|Pertenencia al rol fijo de base de datos **db_securityadmin**, pertenencia al rol fijo de base de datos **db_owner** o pertenencia al rol fijo de servidor **sysadmin**.|  
|Usuario de la base de datos asignado a una clave asimétrica|Pertenencia al rol fijo de base de datos **db_securityadmin**, pertenencia al rol fijo de base de datos **db_owner** o pertenencia al rol fijo de servidor **sysadmin**.|  
|Usuario de la base de datos no asignado a una entidad de seguridad del servidor|Permiso IMPERSONATE en el usuario, pertenencia al rol fijo de base de datos **db_securityadmin**, pertenencia al rol fijo de base de datos **db_owner** o pertenencia al rol fijo de servidor **sysadmin**.|  
|Rol de base de datos|Permiso ALTER para el rol, pertenencia al rol fijo de base de datos **db_securityadmin**, pertenencia al rol fijo de base de datos **db_owner** o pertenencia al rol fijo de servidor **sysadmin**.|  
|Rol de aplicación|Permiso ALTER para el rol, pertenencia al rol fijo de base de datos **db_securityadmin**, pertenencia al rol fijo de base de datos **db_owner** o pertenencia al rol fijo de servidor **sysadmin**.|  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se concede el permiso `VIEW DEFINITION` con `GRANT OPTION` sobre el tipo definido por el usuario `PhoneNumber` al usuario `KhalidR`. `PhoneNumber` se encuentra en el esquema `Telemarketing`.  
  
```sql  
GRANT VIEW DEFINITION ON TYPE::Telemarketing.PhoneNumber   
    TO KhalidR WITH GRANT OPTION;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [DENY &#40;permisos de tipo de Transact-SQL&#41;](../../t-sql/statements/deny-type-permissions-transact-sql.md)   
 [REVOKE &#40;permisos de tipo de Transact-SQL&#41;](../../t-sql/statements/revoke-type-permissions-transact-sql.md)   
 [CREATE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/create-type-transact-sql.md)   
 [Permisos &#40;motor de base de datos&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Securables](../../relational-databases/security/securables.md)   
 [Entidades de seguridad &#40;motor de base de datos&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  

