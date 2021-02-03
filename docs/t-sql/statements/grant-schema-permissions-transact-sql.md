---
description: GRANT (permisos de esquema de Transact-SQL)
title: GRANT (permisos de esquema de Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/19/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- schemas [SQL Server], permissions
- permissions [SQL Server], schemas
- GRANT statement, schemas
- granting permissions [SQL Server], schemas
ms.assetid: b2aa1fc8-e7af-45d2-9f80-737543c8aa95
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: bfe31bbd312b2df347eadf33d0c8b53ec327db2d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161228"
---
# <a name="grant-schema-permissions-transact-sql"></a>GRANT (permisos de esquema de Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Concede permisos para un esquema.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
GRANT permission  [ ,...n ] ON SCHEMA :: schema_name  
    TO database_principal [ ,...n ]  
    [ WITH GRANT OPTION ]  
    [ AS granting_principal ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *permission*  
 Especifica un permiso que se puede conceder en un esquema. Para obtener una lista de permisos, vea la sección Comentarios más adelante en este tema.  
  
 ON SCHEMA **::** schema *_name*  
 Especifica el esquema en el que se va a conceder el permiso. El calificador de ámbito **::** es obligatorio.  
  
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
  
> [!IMPORTANT]  
>  En algunos casos, una combinación de los permisos ALTER y REFERENCE podría permitir al receptor ver datos o ejecutar funciones no autorizadas. Por ejemplo: Un usuario con el permiso ALTER en una tabla y el permiso REFERENCE en una función puede crear una columna calculada sobre una función y hacer que se ejecute. En este caso, el usuario también debe disponer del permiso SELECT en la columna calculada.  
  
 Un esquema es un elemento protegible en el nivel de base de datos que contiene la base de datos que es su entidad primaria en la jerarquía de permisos. La mayoría de permisos limitados y específicos que se pueden conceder para un esquema se muestran a continuación, junto con permisos más generales que los incluyen por implicación.  
  
|Permiso del esquema|Implícito en el permiso del esquema|Implícito en el permiso de base de datos|  
|-----------------------|----------------------------------|------------------------------------|  
|ALTER|CONTROL|ALTER ANY SCHEMA|  
|CONTROL|CONTROL|CONTROL|  
|CREATE SEQUENCE|ALTER|ALTER ANY SCHEMA|  
|Delete|CONTROL|Delete|  
|Ejecute|CONTROL|Ejecute|  
|INSERT|CONTROL|INSERT|  
|REFERENCES|CONTROL|REFERENCES|  
|SELECT|CONTROL|SELECT|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|UPDATE|CONTROL|UPDATE|  
|VIEW CHANGE TRACKING|CONTROL|CONTROL|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
> [!CAUTION]  
>  Un usuario con permiso ALTER en un esquema puede utilizar encadenamiento de propiedad para tener acceso a elementos protegibles de otros esquemas, incluidos aquellos para los que se haya denegado explícitamente el acceso a ese usuario. Esto se debe a que el encadenamiento de propiedad omite las comprobaciones de permisos en los objetos a los que se hace referencia cuando pertenecen al servidor principal propietario de los objetos que hacen referencia a los mismos. Un usuario con permiso ALTER en un esquema puede crear procedimientos, sinónimos y vistas que son propiedad del propietario del esquema. Esos objetos tendrán acceso (mediante encadenamiento de propiedad) a información de otros esquemas que son propiedad del propietario del esquema. Si es posible, se debe evitar conceder permiso ALTER para un esquema si el propietario de dicho esquema también es propietario de otros esquemas.  
  
 Por ejemplo, este problema puede suceder en los siguientes escenarios. En ellos se supone que un usuario, denominado U1, tiene permiso ALTER en el esquema S1. Se deniega al usuario U1 acceso a un objeto tabla, denominado T1, en el esquema S2. Los esquemas S1 y S2 son propiedad del mismo propietario.  
  
 El usuario U1 tiene permiso CREATE PROCEDURE en la base de datos y permiso EXECUTE en el esquema S1. Por tanto, el usuario U1 puede crear un procedimiento almacenado y después tener acceso al objeto T1 denegado en el procedimiento almacenado.  
  
 El usuario U1 tiene permiso CREATE SYNONYM en la base de datos y permiso SELECT en el esquema S1. Por tanto, el usuario U1 puede crear un sinónimo en el esquema S1 para el objeto T1 denegado y después tener acceso a dicho objeto mediante el sinónimo.  
  
 El usuario U1 tiene permiso CREATE VIEW en la base de datos y permiso SELECT en el esquema S1. Por tanto, el usuario U1 puede crear una vista en el esquema S1 para consultar datos del objeto T1 denegado y después tener acceso a dicho objeto mediante la vista.
  
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
  
### <a name="a-granting-insert-permission-on-schema-humanresources-to-guest"></a>A. Conceder a un invitado el permiso INSERT en el esquema HumanResources  
  
```sql  
GRANT INSERT ON SCHEMA :: HumanResources TO guest;  
```  
  
### <a name="b-granting-select-permission-on-schema-person-to-database-user-wiljo"></a>B. Conceder al usuario de base de datos WilJo el permiso SELECT en el esquema Person  
  
```sql  
GRANT SELECT ON SCHEMA :: Person TO WilJo WITH GRANT OPTION;  
```  
  
## <a name="see-also"></a>Consulte también  
 [DENY &#40;permisos de esquema de Transact-SQL&#41;](../../t-sql/statements/deny-schema-permissions-transact-sql.md)   
 [REVOKE &#40;permisos de esquema de Transact-SQL&#41;](../../t-sql/statements/revoke-schema-permissions-transact-sql.md)   
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [Permisos &#40;motor de base de datos&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Entidades de seguridad &#40;motor de base de datos&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)   
 [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)   
 [CREATE APPLICATION ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-application-role-transact-sql.md)   
 [Jerarquía de cifrado](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [sys.fn_builtin_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [sys.fn_my_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md)   
 [HAS_PERMS_BY_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/has-perms-by-name-transact-sql.md)  
  
  
