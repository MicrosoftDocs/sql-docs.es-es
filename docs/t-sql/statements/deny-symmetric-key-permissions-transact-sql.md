---
description: DENY (permisos de clave simétrica de Transact-SQL)
title: DENY (permisos de clave simétrica de Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/09/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- denying permissions [SQL Server], symmetric keys
- symmetric keys [SQL Server], permissions
- permissions [SQL Server], symmetric keys
- DENY statement, symmetric keys
- encryption [SQL Server], symmetric keys
- cryptography [SQL Server], symmetric keys
ms.assetid: 52d4b12d-17be-4cbd-aa78-65332a4883b0
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: a23279a7878955f7419fe3f74feb298b22a0efa1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198641"
---
# <a name="deny-symmetric-key-permissions-transact-sql"></a>DENY (permisos de clave simétrica de Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Deniega permisos en una clave simétrica.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
DENY permission [ ,...n ]    
    ON SYMMETRIC KEY :: symmetric_key_name   
        TO <database_principal> [ ,...n ] [ CASCADE ]  
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
 Especifica un permiso que se puede denegar en una clave simétrica. Para obtener una lista de permisos, vea la sección Comentarios que se muestra posteriormente en este tema.  
  
 ON SYMMETRIC KEY ::*asymmetric_key_name*  
 Especifica la clave simétrica en la que se va a denegar el permiso. Se requiere el calificador de ámbito (::).  
  
 TO \<*database_principal*>  
 Especifica la entidad de seguridad desde la que se revoca el permiso.  
  
 CASCADE  
 Indica que el permiso que se va a denegar también se denegará a otras entidades de seguridad a las que esta entidad de seguridad ha concedido permisos.  
  
 AS \<database_principal>.  
 Especifica una entidad de seguridad de la que la entidad de seguridad que ejecuta esta consulta deriva su derecho de denegar el permiso.  
  
 *Database_user*  
 Especifica un usuario de base de datos.  
  
 *Database_role*  
 Especifica un rol de base de datos.  
  
 *Application_role*  
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
 Puede ver información sobre claves simétricas en la vista de catálogo [sys.symmetric_keys](../../relational-databases/system-catalog-views/sys-symmetric-keys-transact-sql.md).  
  
 Una clave simétrica es un elemento protegible de nivel de base de datos que contiene la base de datos que es su entidad primaria en la jerarquía de permisos. La mayoría de permisos limitados y específicos que se pueden denegar en una clave simétrica se muestran en la siguiente tabla, junto con permisos más generales que los incluyen por implicación.  
  
|Permiso de clave simétrica|Implícito en el permiso de clave simétrica|Implícito en el permiso de base de datos|  
|------------------------------|-----------------------------------------|------------------------------------|  
|ALTER|CONTROL|ALTER ANY SYMMETRIC KEY|  
|CONTROL|CONTROL|CONTROL|  
|REFERENCES|CONTROL|REFERENCES|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso CONTROL en la clave simétrica o el permiso ALTER ANY SYMMETRIC KEY en la base de datos. Si utiliza la opción AS, la entidad de seguridad especificada debe ser propietaria de la clave simétrica.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se deniega el permiso `ALTER` para la clave simétrica `SamInventory42` al usuario de base de datos `HamidS`.  
  
```sql  
USE AdventureWorks2012;  
DENY ALTER ON SYMMETRIC KEY::SamInventory42 TO HamidS;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [sys.symmetric_keys &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-symmetric-keys-transact-sql.md)   
 [GRANT &#40;permisos de clave simétrica de Transact-SQL&#41;](../../t-sql/statements/grant-symmetric-key-permissions-transact-sql.md)   
 [REVOKE &#40;permisos de clave simétrica de Transact-SQL&#41;](../../t-sql/statements/revoke-symmetric-key-permissions-transact-sql.md)   
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)   
 [Permisos &#40;motor de base de datos&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Entidades de seguridad &#40;motor de base de datos&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [Jerarquía de cifrado](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  
