---
description: Credencial de ámbito de base de datos DENY (Transact-SQL)
title: Credencial de ámbito de base de datos DENY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/16/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: conceptual
f1_keywords:
- DENY DATABASE SCOPED CREDENTIAL
- DENY_DATABASE_SCOPED_CREDENTIAL_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- DENY statement, database scoped credentials
- denying permissions [SQL Server], database scoped credential
ms.assetid: c508b1c9-169e-4e7a-9a49-7ddf2ca8f848
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 5851644e5ae982f94b55af1ef527262f2169fae6
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97464020"
---
# <a name="deny-database-scoped-credential-transact-sql"></a>Credencial de ámbito de base de datos DENY (Transact-SQL)
[!INCLUDE[sqlserver2017-asdb](../../includes/applies-to-version/sqlserver2017-asdb.md)]

  Deniega permisos en una credencial de ámbito de base de datos.  

  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
  
DENY permission  [ ,...n ]   
    ON DATABASE SCOPED CREDENTIAL :: credential_name   
    TO principal [ ,...n ]  
    [ CASCADE ]  
    [ AS denying_principal ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *permission*  
 Especifica un permiso que puede denegarse en una credencial de ámbito de base de datos. Se muestra a continuación.  
  
 ON DATABASE SCOPED CREDENTIAL **::** _credential_name_  
 Especifica la credencial de ámbito de bases de dados en la que se va a denegar el permiso. Es preciso utilizar el calificador de ámbito "::".  
  
 *database_principal*  
 Especifica la entidad de seguridad a la que se deniega el permiso. Uno de los siguientes:  
  
-   usuario de base de datos  
  
-   rol de base de datos  
  
-   rol de aplicación  
  
-   usuario de base de datos asignado a un inicio de sesión de Windows  
  
-   usuario de base de datos asignado a un grupo de Windows  
  
-   usuario de base de datos asignado a un certificado  
  
-   usuario de base de datos asignado a una clave asimétrica  
  
-   usuario de base de datos no asignado a una entidad de seguridad del servidor  
  
 CASCADE  
 Indica que el permiso que se va a denegar también se denegará a otras entidades de seguridad a las que esta entidad de seguridad ha concedido permisos.  
  
 *denying_principal*  
 Especifica una entidad de seguridad de la que la entidad de seguridad que ejecuta esta consulta deriva su derecho de denegar el permiso. Uno de los siguientes:  
  
-   usuario de base de datos  
  
-   rol de base de datos  
  
-   rol de aplicación  
  
-   usuario de base de datos asignado a un inicio de sesión de Windows  
  
-   usuario de base de datos asignado a un grupo de Windows  
  
-   usuario de base de datos asignado a un certificado  
  
-   usuario de base de datos asignado a una clave asimétrica  
  
-   usuario de base de datos no asignado a una entidad de seguridad del servidor  
  
## <a name="remarks"></a>Observaciones  
 Una credencial de ámbito de base de datos es un elemento protegible de nivel de base de datos que contiene la base de datos que es su entidad primaria en la jerarquía de permisos. La mayoría de permisos limitados y específicos que se pueden denegar en una credencial de ámbito de base de datos se muestran a continuación, junto con permisos más generales que los incluyen por implicación.  
  
|Permiso de credencial de ámbito de base de datos|Implícito en el permiso de credencial de ámbito de base de datos|Implícito en el permiso de base de datos|  
|----------------------------|---------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|CONTROL|  
|REFERENCES|CONTROL|REFERENCES|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>Permisos  
 Necesita el permiso CONTROL en la credencial de ámbito de base de datos. Si se utiliza la cláusula AS, la entidad de seguridad especificada debe poseer la credencial de ámbito de base de datos.  
  
## <a name="see-also"></a>Consulte también  
 [DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-transact-sql.md)   
 [Credencial de ámbito de base de datos GRANT (Transact-SQL)](../../t-sql/statements/grant-database-scoped-credential-transact-sql.md)   
 [Credencial de ámbito de base de datos REVOKE (Transact-SQL)](../../t-sql/statements/revoke-database-scoped-credential-transact-sql.md)   
 [Permisos &#40;motor de base de datos&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Entidades de seguridad &#40;motor de base de datos&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [Jerarquía de cifrado](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  
