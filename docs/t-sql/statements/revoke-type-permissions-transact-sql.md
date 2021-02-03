---
description: REVOKE (permisos de tipo de Transact-SQL)
title: REVOKE (permisos de tipo de Transact-SQL) | Microsoft Docs
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
- REVOKE statement, types
- permissions [SQL Server], types
- type permissions [SQL Server]
ms.assetid: 3969c7e9-ca10-4c67-971b-25d2dfccf650
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 2291704df7fd73dc54ebd353b32e11700175aba7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193373"
---
# <a name="revoke-type-permissions-transact-sql"></a>REVOKE (permisos de tipo de Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Revoca permisos en un tipo.  
  
  ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
REVOKE [ GRANT OPTION FOR ] permission [ ,...n ]   
    ON TYPE :: [ schema_name ]. type_name   
    { FROM | TO } <database_principal> [ ,...n ]   
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
 Especifica un permiso que se puede revocar en un tipo. Para obtener una lista de permisos, vea la sección Comentarios que se muestra posteriormente en este tema.  
  
 ON TYPE **::** [ *schema_name* ] **.** *type_name*  
 Especifica el tipo en el que se va a revocar el permiso. El calificador de ámbito ( **::** ) es obligatorio. Si no se especifica *schema_name*, se usa el esquema predeterminado. Si se especifica *schema_name*, se necesita el calificador de ámbito de esquema ( **.** ).  
  
 { FROM | TO } \<database_principal> Especifica la entidad de seguridad desde la que se revoca el permiso.  
  
 GRANT OPTION  
 Indica que se revocará el derecho de conceder el permiso especificado a otras entidades de seguridad. No se revocará el permiso.  
  
> [!IMPORTANT]  
>  Si la entidad de seguridad dispone del permiso especificado sin la opción GRANT, se revocará el permiso.  
  
 CASCADE  
 Indica que el permiso que se va a revocar también se revocará de otras entidades de seguridad a las que esta entidad de seguridad ha concedido o denegado permisos.  
  
> [!CAUTION]  
>  Una revocación en cascada de un permiso concedido WITH GRANT OPTION revocará tanto GRANT como DENY de dicho permiso.  
  
 AS \<database_principal> Especifica una entidad de seguridad de la que la entidad de seguridad que ejecuta esta consulta deriva su derecho de revocar el permiso.  
  
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
  
 La mayoría de permisos limitados y específicos que se pueden revocar en un tipo se muestran en la siguiente tabla, junto con permisos más generales que los incluyen por implicación.  
  
|Permiso de tipo|Implicado por el permiso de tipo|Implícito en el permiso del esquema|  
|---------------------|--------------------------------|----------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|Ejecute|CONTROL|Ejecute|  
|REFERENCES|CONTROL|REFERENCES|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso CONTROL en el tipo. Si utiliza la cláusula AS, la entidad de seguridad especificada debe ser propietaria del tipo.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se revoca el permiso `VIEW DEFINITION` para el tipo definido por el usuario `PhoneNumber` del usuario `KhalidR`. La opción `CASCADE` indica que el permiso `VIEW DEFINITION` también se revocará a las entidades de seguridad a las que `KhalidR` lo concedió. `PhoneNumber` se encuentra en el esquema `Telemarketing`.  
  
```sql  
REVOKE VIEW DEFINITION ON TYPE::Telemarketing.PhoneNumber   
    FROM KhalidR CASCADE;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [GRANT &#40;permisos de tipo de Transact-SQL&#41;](../../t-sql/statements/grant-type-permissions-transact-sql.md)   
 [DENY &#40;permisos de tipo de Transact-SQL&#41;](../../t-sql/statements/deny-type-permissions-transact-sql.md)   
 [CREATE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/create-type-transact-sql.md)   
 [Permisos &#40;motor de base de datos&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Entidades de seguridad &#40;motor de base de datos&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [Elementos protegibles](../../relational-databases/security/securables.md)  
  
  

