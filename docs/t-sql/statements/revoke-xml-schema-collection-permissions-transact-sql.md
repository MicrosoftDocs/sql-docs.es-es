---
title: Permisos de colección de esquemas XML REVOKE
description: Use Transact-SQL para aplicar REVOKE a permisos de colección de esquemas XML.
ms.custom: seo-lt-2019
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- REVOKE statement, XML schema collections
- XML schema collections [SQL Server], permissions
- schema collections [SQL Server], permissions
ms.assetid: 8ca0973c-30b2-4633-a165-c09b13cc81ae
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5404b1d743f32e07244f54f86e039087e05f6ab9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198430"
---
# <a name="revoke-xml-schema-collection-permissions-transact-sql"></a>REVOKE (permisos de colección de esquemas XML de Transact-SQL)
[!INCLUDE [sql-asdbmi-pdw](../../includes/applies-to-version/sql-asdbmi-pdw.md)]

  Revoca los permisos concedidos o denegados para una colección de esquemas XML.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
REVOKE [ GRANT OPTION FOR ] permission [ ,...n ] ON   
    XML SCHEMA COLLECTION :: [ schema_name . ]  
    XML_schema_collection_name  
    { TO | FROM } <database_principal> [ ,...n ]  
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
 Especifica un permiso que se puede revocar en una colección de esquemas XML. Para obtener una lista de permisos, vea la sección Comentarios que se muestra posteriormente en este tema.  
  
 ON XML SCHEMA COLLECTION :: [ _schema_name_ **.** ] *XML_schema_collection_name*  
 Especifica la colección de esquemas XML en la que se va a revocar el permiso. Se requiere el calificador de ámbito (::). Si no se especifica *schema_name*, se usa el esquema predeterminado. Si se especifica *schema_name*, se necesita el calificador de ámbito de esquema (.).  
  
 GRANT OPTION  
 Indica que se revocará el derecho de conceder el permiso especificado a otras entidades de seguridad. No se revocará el permiso.  
  
> [!IMPORTANT]  
>  Si la entidad de seguridad dispone del permiso especificado sin la opción GRANT, se revocará el permiso.  
  
 CASCADE  
 Indica que el permiso que se va a revocar también se revocará de otras entidades de seguridad a las que esta entidad de seguridad ha concedido o denegado permisos.  
  
> [!CAUTION]  
>  Una revocación en cascada de un permiso concedido WITH GRANT OPTION revocará tanto GRANT como DENY de dicho permiso.  
  
 { TO | FROM } \<*database_principal*>  
 Especifica la entidad de seguridad desde la que se revoca el permiso.  
  
 AS \<database_principal> Especifica una entidad de seguridad desde la que la entidad de seguridad que ejecuta esta consulta deriva su derecho de revocar el permiso.  
  
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
  
 Se produce un error en la instrucción si no se especifica CASCADE al revocar un permiso de una entidad de seguridad que tenía concedido ese permiso con GRANT OPTION.  
  
 Una colección de esquemas XML es un elemento protegible de nivel de esquema que contiene el esquema que es su entidad primaria en la jerarquía de permisos. La mayoría de permisos limitados y específicos que se pueden revocar en una colección de esquemas XML se muestran en la siguiente tabla, junto con permisos más generales que los incluyen por implicación.  
  
|Permiso de colección de esquemas XML|Implicado por el permiso de colección de esquemas XML|Implícito en el permiso del esquema|  
|--------------------------------------|-------------------------------------------------|----------------------------------|  
|ALTER|CONTROL|ALTER|  
|CONTROL|CONTROL|CONTROL|  
|Ejecute|CONTROL|Ejecute|  
|REFERENCES|CONTROL|REFERENCES|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso CONTROL en la colección de esquemas XML. Si utiliza la opción AS, la entidad de seguridad especificada debe ser propietaria de la colección de esquemas XML.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se revoca el permiso `EXECUTE` para la colección de esquemas XML `Invoices4` al usuario `Wanida`. La colección de esquemas XML `Invoices4` se ubica dentro del esquema `Sales` de la base de datos `AdventureWorks2012`.  
  
 ```sql
 USE AdventureWorks2012;  
 REVOKE EXECUTE ON XML SCHEMA COLLECTION::Sales.Invoices4 FROM Wanida;  
 GO
 ```  
  
## <a name="see-also"></a>Consulte también  
 [GRANT &#40;permisos de colección de esquemas XML de Transact-SQL&#41;](../../t-sql/statements/grant-xml-schema-collection-permissions-transact-sql.md)   
 [DENY &#40;permisos de colección de esquemas XML de Transact-SQL&#41;](../../t-sql/statements/deny-xml-schema-collection-permissions-transact-sql.md)   
 [sys.xml_schema_collections &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-xml-schema-collections-transact-sql.md)   
 [CREATE XML SCHEMA COLLECTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-xml-schema-collection-transact-sql.md)   
 [Permisos &#40;motor de base de datos&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Entidades de seguridad &#40;motor de base de datos&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  

