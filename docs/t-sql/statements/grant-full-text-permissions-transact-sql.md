---
description: GRANT (permisos de texto completo de Transact-SQL)
title: GRANT (permisos de texto completo de Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- granting permissions [SQL Server], full-text
- full-text search [SQL Server], permissions
- full-text catalogs [SQL Server], permissions
- full-text stoplist [SQL Server], permissions
- GRANT statement, full-text permissions
ms.assetid: fdb64e09-222a-47fe-b08b-999264ca261d
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f20a996c6d89f1925a8e2f1317826b6e572d0d01
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161203"
---
# <a name="grant-full-text-permissions-transact-sql"></a>GRANT (permisos de texto completo de Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Concede permisos en un catálogo de texto completo o en una lista de palabras irrelevantes de texto completo.  
  

  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
GRANT permission [ ,...n ] ON  
    FULLTEXT   
        {  
           CATALOG :: full-text_catalog_name  
           |  
           STOPLIST :: full-text_stoplist_name  
        }  
    TO database_principal [ ,...n ]  
    [ WITH GRANT OPTION ]  
    [ AS granting_principal ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *permission*  
 Es el nombre de un permiso. Las asignaciones válidas de permisos a elementos protegibles se describen en la sección "Comentarios", más adelante en este tema.  
  
 ON FULLTEXT CATALOG **::**_nombre_catálogo_de_texto_completo_  
 Especifica el catálogo de texto completo para el que se concede el permiso. El calificador de ámbito **::** es obligatorio.  
  
 ON FULLTEXT STOPLIST **::**_nombre_de_la_lista_de_palabras_irrelevantes_de_texto_completo_  
 Especifica la lista de palabras irrelevantes de texto completo en la que se concede el permiso. El calificador de ámbito **::** es obligatorio.  
  
 *database_principal*  
 Especifica la entidad de seguridad para la que se concede el permiso. Uno de los siguientes:  
  
-   usuario de base de datos  
-   rol de base de datos  
-   rol de aplicación  
-   usuario de base de datos asignado a un inicio de sesión de Windows  
-   usuario de base de datos asignado a un grupo de Windows  
-   usuario de base de datos asignado a un certificado  
-   usuario de base de datos asignado a una clave asimétrica  
-   usuario de base de datos no asignado a una entidad de seguridad del servidor  
  
GRANT OPTION  
 Indica que la entidad de seguridad también podrá conceder el permiso especificado a otras entidades de seguridad.  
  
AS *granting_principal*  
 Especifica una entidad de seguridad de la que la entidad de seguridad que ejecuta esta consulta deriva su derecho de conceder el permiso. Uno de los siguientes:  
  
-   usuario de base de datos  
-   rol de base de datos  
-   rol de aplicación  
-   usuario de base de datos asignado a un inicio de sesión de Windows  
-   usuario de base de datos asignado a un grupo de Windows  
-   usuario de base de datos asignado a un certificado  
-   usuario de base de datos asignado a una clave asimétrica  
-   usuario de base de datos no asignado a una entidad de seguridad del servidor  
  
## <a name="remarks"></a>Observaciones  
  
## <a name="fulltext-catalog-permissions"></a>Permisos FULLTEXT CATALOG  
 Un catálogo de texto completo es un elemento protegible en el nivel de base de datos que contiene la base de datos que es su entidad primaria en la jerarquía de permisos. Los permisos más limitados y específicos que se pueden conceder en un catálogo de texto completo se muestran en la siguiente tabla, junto con permisos más generales que los incluyen por implicación.  
  
|Permiso de catálogo de texto completo|Implícito en el permiso de catálogo de texto completo|Implícito en el permiso de base de datos|  
|-----------------------------------|----------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY FULLTEXT CATALOG|  
|REFERENCES|CONTROL|REFERENCES|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="fulltext-stoplist-permissions"></a>Permisos FULLTEXT STOPLIST  
 Una lista de palabras irrelevantes de texto completo es un elemento protegible en el nivel de base de datos que contiene la base de datos que es su entidad primaria en la jerarquía de permisos. Los permisos más limitados y específicos que se pueden conceder en una lista de palabras irrelevantes de texto completo se muestran en la siguiente tabla, junto con permisos más generales que los incluyen por implicación.  
  
|Permiso de lista de palabras irrelevantes de texto completo|Implícito en el permiso de lista de palabras irrelevantes de texto completo|Implícito en el permiso de base de datos|  
|------------------------------------|-----------------------------------------------|------------------------------------|  
|ALTER|CONTROL|ALTER ANY FULLTEXT CATALOG|  
|CONTROL|CONTROL|CONTROL|  
|REFERENCES|CONTROL|REFERENCES|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>Permisos  
 El otorgante del permiso (o la entidad de seguridad especificada con la opción AS) debe tener el permiso con GRANT OPTION, o un permiso superior que implique el permiso que se va a conceder.  
  
 Si se utiliza la opción AS, debe tener en cuenta los requisitos adicionales siguientes.  
  
|AS *granting_principal*|Permiso adicional necesario|  
|------------------------------|------------------------------------|  
|Usuario de la base de datos|Permiso IMPERSONATE para el usuario, pertenencia al rol fijo de base de datos db_securityadmin, pertenencia al rol fijo de base de datos db_owner o pertenencia al rol fijo de servidor sysadmin.|  
|Usuario de la base de datos asignado a un inicio de sesión de Windows|Permiso IMPERSONATE para el usuario, pertenencia al rol fijo de base de datos db_securityadmin, pertenencia al rol fijo de base de datos db_owner o pertenencia al rol fijo de servidor sysadmin.|  
|Usuario de la base de datos asignado a un grupo de Windows|Pertenencia al grupo de Windows, pertenencia al rol fijo de base de datos db_securityadmin, pertenencia al rol fijo de base de datos db_owner o pertenencia al rol fijo de servidor sysadmin.|  
|Usuario de la base de datos asignado a un certificado|Pertenencia al rol fijo de base de datos db_securityadmin, pertenencia al rol fijo de base de datos db_owner o pertenencia al rol fijo de servidor sysadmin.|  
|Usuario de la base de datos asignado a una clave asimétrica|Pertenencia al rol fijo de base de datos db_securityadmin, pertenencia al rol fijo de base de datos db_owner o pertenencia al rol fijo de servidor sysadmin.|  
|Usuario de la base de datos no asignado a una entidad de seguridad del servidor|Permiso IMPERSONATE para el usuario, pertenencia al rol fijo de base de datos db_securityadmin, pertenencia al rol fijo de base de datos db_owner o pertenencia al rol fijo de servidor sysadmin.|  
|Rol de base de datos|Permiso ALTER para el rol, pertenencia al rol fijo de base de datos db_securityadmin, pertenencia al rol fijo de base de datos db_owner o pertenencia al rol fijo de servidor sysadmin.|  
|Rol de aplicación|Permiso ALTER para el rol, pertenencia al rol fijo de base de datos db_securityadmin, pertenencia al rol fijo de base de datos db_owner o pertenencia al rol fijo de servidor sysadmin.|  
  
 Los propietarios de objetos pueden conceder permisos para los objetos que poseen. Las entidades de seguridad con permiso CONTROL sobre un elemento protegible pueden conceder permisos para ese elemento.  
  
 Los receptores del permiso CONTROL SERVER como los miembros del rol fijo de servidor sysadmin pueden conceder los permisos sobre cualquier elemento protegible en el servidor. Los receptores del permiso CONTROL para una base de datos, como los miembros del rol fijo de base de datos db_owner, pueden conceder los permisos para cualquier elemento protegible en la base de datos. Los receptores del permiso CONTROL en un esquema pueden conceder los permisos en cualquier objeto del esquema.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-granting-permissions-to-a-full-text-catalog"></a>A. Conceder permisos en un catálogo de texto completo  
 En el ejemplo siguiente, se concede a `Ted` el permiso `CONTROL` en el catálogo de texto completo `ProductCatalog`.  
  
```sql  
GRANT CONTROL  
    ON FULLTEXT CATALOG :: ProductCatalog  
    TO Ted ;  
```  
  
### <a name="b-granting-permissions-to-a-stoplist"></a>B. Conceder permisos en una lista de palabras irrelevantes  
 En el ejemplo siguiente, se concede a `Mary` el permiso `VIEW DEFINITION` en la lista de palabras irrelevantes de texto completo `ProductStoplist`.  
  
```sql  
GRANT VIEW DEFINITION  
    ON FULLTEXT STOPLIST :: ProductStoplist  
    TO Mary ;  
```  
  
## <a name="see-also"></a>Consulte también  
 [CREATE APPLICATION ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-application-role-transact-sql.md)   
 [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)   
 [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)   
 [CREATE FULLTEXT CATALOG &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-catalog-transact-sql.md)   
 [CREATE FULLTEXT STOPLIST &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-stoplist-transact-sql.md)   
 [Jerarquía de cifrado](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [sys.fn_my_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md)   
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [HAS_PERMS_BY_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/has-perms-by-name-transact-sql.md)   
 [Permisos &#40;motor de base de datos&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Entidades de seguridad &#40;motor de base de datos&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [sys.fn_builtin_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [sys.fulltext_catalogs &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-catalogs-transact-sql.md)   
 [sys.fulltext_stoplists &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-stoplists-transact-sql.md)  
  
  
