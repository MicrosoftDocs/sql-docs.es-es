---
title: Permisos de colección de esquemas XML DENY
description: Deniegue permisos en una colección de esquemas XML.
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 06/09/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- denying permissions [SQL Server], XML schema collections
- XML schema collections [SQL Server], permissions
- DENY statement, XML schema collections
- schema collections [SQL Server], permissions
ms.assetid: 159969a7-8313-41bc-bb19-c55af76597e6
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 148b3837b069457c1ea144e5cd2bf21b6515e37f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200402"
---
# <a name="deny-xml-schema-collection-permissions-transact-sql"></a>DENY (permisos de colección de esquemas XML de Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Deniega permisos en una colección de esquemas XML.  
  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
DENY permission  [ ,...n ] ON   
    XML SCHEMA COLLECTION :: [ schema_name . ]  
    XML_schema_collection_name  
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
 Especifica un permiso que se puede denegar en una colección de esquemas XML. Para obtener una lista de permisos, vea la sección Comentarios que se muestra posteriormente en este tema.  
  
 ON XML SCHEMA COLLECTION :: [ _schema_name_ **.** ] *XML_schema_collection_name*  
 Especifica la colección de esquemas XML en la que se va a denegar el permiso. Se requiere el calificador de ámbito (::). Si no se especifica *schema_name*, se usa el esquema predeterminado. Si se especifica *schema_name*, se necesita el calificador de ámbito de esquema (.).  
  
 PARA \<database_principal>  
 Especifica la entidad de seguridad a la que se deniega el permiso.  
  
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
 Encontrará información sobre las colecciones de esquemas XML en la vista de catálogo [sys.xml_schema_collections](../../relational-databases/system-catalog-views/sys-xml-schema-collections-transact-sql.md).  
  
 Una colección de esquemas XML es un elemento protegible de nivel de esquema que contiene el esquema que es su entidad primaria en la jerarquía de permisos. La mayoría de permisos limitados y específicos que se pueden denegar en una colección de esquemas XML se muestran en la siguiente tabla, junto con permisos más generales que los incluyen por implicación.  
  
|Permiso de colección de esquemas XML|Implicado por el permiso de colección de esquemas XML|Implícito en el permiso del esquema|  
|--------------------------------------|-------------------------------------------------|----------------------------------|  
|ALTER|CONTROL|ALTER|  
|CONTROL|CONTROL|CONTROL|  
|Ejecute|CONTROL|Ejecute|  
|REFERENCES|CONTROL|REFERENCES|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>Permisos  
 Requiere CONTROL en la colección de esquemas XML. Si utiliza la opción AS, la entidad de seguridad especificada debe ser propietaria de la colección de esquemas XML.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se deniega el permiso `EXECUTE` para la colección de esquemas XML `Invoices4` al usuario `Wanida`. La colección de esquemas XML `Invoices4` se ubica dentro del esquema `Sales` de la base de datos `AdventureWorks2012`.  
  
```sql  
USE AdventureWorks2012;  
DENY EXECUTE ON XML SCHEMA COLLECTION::Sales.Invoices4 TO Wanida;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [GRANT &#40;permisos de colección de esquemas XML de Transact-SQL&#41;](../../t-sql/statements/grant-xml-schema-collection-permissions-transact-sql.md)   
 [REVOKE &#40;permisos de colección de esquemas XML de Transact-SQL&#41;](../../t-sql/statements/revoke-xml-schema-collection-permissions-transact-sql.md)   
 [sys.xml_schema_collections &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-xml-schema-collections-transact-sql.md)   
 [CREATE XML SCHEMA COLLECTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-xml-schema-collection-transact-sql.md)   
 [Permisos &#40;motor de base de datos&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Entidades de seguridad &#40;motor de base de datos&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  
