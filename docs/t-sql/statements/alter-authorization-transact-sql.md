---
description: ALTER AUTHORIZATION (Transact-SQL)
title: ALTER AUTHORIZATION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/01/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_AUTHORIZATION_TSQL
- ALTER AUTHORIZATION
dev_langs:
- TSQL
helpviewer_keywords:
- owners [SQL Server], transferring
- modifying entity ownership
- full-text search [SQL Server], permissions
- owners [SQL Server], entities
- ALTER AUTHORIZATION statement
- entity ownership [SQL Server]
- transferring ownership
- search property lists [SQL Server], permissions
- TAKE OWNERSHIP
ms.assetid: 8c805ae2-91ed-4133-96f6-9835c908f373
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 946698640e480634db7bcd4431f55012426e1a90
ms.sourcegitcommit: fa63019cbde76dd981b0c5a97c8e4d57e8d5ca4e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/03/2021
ms.locfileid: "99495793"
---
# <a name="alter-authorization-transact-sql"></a>ALTER AUTHORIZATION (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Cambia la propiedad de un elemento protegible.

 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Sintaxis

```syntaxsql
-- Syntax for SQL Server
ALTER AUTHORIZATION
    ON [ <class_type>:: ] entity_name
    TO { principal_name | SCHEMA OWNER }
    [;]

<class_type> ::=
     {
      OBJECT | ASSEMBLY | ASYMMETRIC KEY | AVAILABILITY GROUP | CERTIFICATE
    | CONTRACT | TYPE | DATABASE | ENDPOINT | FULLTEXT CATALOG
    | FULLTEXT STOPLIST | MESSAGE TYPE | REMOTE SERVICE BINDING
    | ROLE | ROUTE | SCHEMA | SEARCH PROPERTY LIST | SERVER ROLE
    | SERVICE | SYMMETRIC KEY | XML SCHEMA COLLECTION
     }
```

```syntaxsql
-- Syntax for SQL Database

ALTER AUTHORIZATION
    ON [ <class_type>:: ] entity_name
    TO { principal_name | SCHEMA OWNER }
    [;]

<class_type> ::=
     {
    OBJECT | ASSEMBLY | ASYMMETRIC KEY | CERTIFICATE
     | TYPE | DATABASE | FULLTEXT CATALOG
     | FULLTEXT STOPLIST
     | ROLE | SCHEMA | SEARCH PROPERTY LIST
     | SYMMETRIC KEY | XML SCHEMA COLLECTION
     }
```

```syntaxsql
-- Syntax for Azure Synapse Analytics

ALTER AUTHORIZATION ON
     [ <class_type> :: ] <entity_name>
     TO { principal_name | SCHEMA OWNER }
    [;]

    <class_type> ::= {
    SCHEMA
     | OBJECT
    }

    <entity_name> ::=
    {
    schema_name
     | [ schema_name. ] object_name
    }
```

```syntaxsql
-- Syntax for Parallel Data Warehouse

ALTER AUTHORIZATION ON
     [ <class_type> :: ] <entity_name>
     TO { principal_name | SCHEMA OWNER }
    [;]

<class_type> ::= {
    DATABASE
     | SCHEMA
     | OBJECT
    }

<entity_name> ::=
    {
    database_name
     | schema_name
     | [ schema_name. ] object_name
    }
```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos

\<class_type> Es la clase protegible de la entidad para la que se va a cambiar el propietario. El valor predeterminado es OBJECT.

|Clase|Producto|
|-|-|
|OBJECT|**SE APLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].|
|ASSEMBLY|**SE APLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|
|ASYMMETRIC KEY|**SE APLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|
|AVAILABILITY GROUP |**SE APLICA A**: SQL Server 2012 y versiones posteriores|
|CERTIFICADO|**SE APLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|
|CONTRACT|**SE APLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores|
|DATABASE|**SE APLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] Para más información, consulte [ALTER AUTHORIZATION para bases de datos](#alter-authorization-for-databases).|
|ENDPOINT|**SE APLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores|
|FULLTEXT CATALOG|**SE APLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|
|FULLTEXT STOPLIST|**SE APLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|
|MESSAGE TYPE|**SE APLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores|
|REMOTE SERVICE BINDING|**SE APLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores|
|ROLE|**SE APLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|
|ROUTE|**SE APLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores|
|SCHEMA|**SE APLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].|
|SEARCH PROPERTY LIST|**SE APLICA A**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|
|SERVER ROLE|**SE APLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores|
|SERVICE|**SE APLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores|
|SYMMETRIC KEY|**SE APLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|
|TYPE|**SE APLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|
|XML SCHEMA COLLECTION|**SE APLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|

*entity_name* es el nombre de la entidad.

*principal_name* | SCHEMA OWNER: nombre del propietario del esquema de la entidad de seguridad que será propietaria de la entidad. Los objetos de base de datos deben tener una entidad de base de datos como propietaria; un usuario o rol de base de datos. Los objetos de servidor (como bases de datos) deben tener una entidad de servidor (un inicio de sesión) como propietaria. Indique **SCHEMA OWNER** como *principal_name para indicar que el objeto debe tener como propietaria la entidad que posee el esquema del objeto.

## <a name="remarks"></a>Observaciones

Se puede usar ALTER AUTHORIZATION para cambiar la propiedad de cualquier entidad que tenga propietario. La propiedad de las entidades que contienen bases de datos se puede transferir a cualquier entidad de seguridad de nivel de base de datos. La propiedad de las entidades de nivel de servidor solo se puede transferir a entidades de seguridad de nivel de servidor.

> [!IMPORTANT]
> A partir de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], un usuario puede ser propietario de un objeto o un tipo contenido en un esquema de otro usuario de la base de datos. Es un cambio de comportamiento con respecto a versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener más información, vea [OBJECTPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/objectproperty-transact-sql.md) y [TYPEPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/typeproperty-transact-sql.md).

Se puede transferir la propiedad de las siguientes entidades contenidas en esquemas de tipo "objeto": tablas, vistas, funciones, procedimientos, colas y sinónimos.

No se puede transferir la propiedad de las siguientes entidades: servidores vinculados, estadísticas, restricciones, reglas, valores predeterminados, desencadenadores, colas de [!INCLUDE[ssSB](../../includes/sssb-md.md)], credenciales, funciones de partición, esquemas de partición, claves maestras de bases de datos, clave maestra de servicios y notificaciones de eventos.

No se puede transferir la propiedad de los miembros de las siguientes clases de elementos protegibles: servidor, inicio de sesión, usuario, rol de aplicación y columna.

La opción SCHEMA OWNER solo es válida cuando se transfiere la propiedad de una entidad que contiene esquemas. SCHEMA OWNER transferirá la propiedad de la entidad al propietario del esquema en el que reside. Solo las entidades de clase OBJECT, TYPE o XML SCHEMA COLLECTION contienen esquemas.

Si la entidad de destino no es una base de datos y la entidad se va a transferir a un nuevo propietario, se quitarán todos los permisos del destino.

> [!CAUTION]
> En [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], el comportamiento de los esquemas es distinto del de las versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si en el código se supone que los esquemas son equivalentes a usuarios de base de datos, los resultados obtenidos podrían ser incorrectos. No se deben usar vistas de catálogo antiguas, como sysobjects, en una base de datos en la que nunca se haya usado ninguna de las instrucciones DDL siguientes: CREATE SCHEMA, ALTER SCHEMA, DROP SCHEMA, CREATE USER, ALTER USER, DROP USER, CREATE ROLE, ALTER ROLE, DROP ROLE, CREATE APPROLE, ALTER APPROLE, DROP APPROLE, ALTER AUTHORIZATION. En una base de datos en la que se ha usado alguna de estas instrucciones, deben usarse las nuevas vistas de catálogo. En las nuevas vistas de catálogo se tiene en cuenta la separación de entidades de seguridad y esquemas que se presentó en [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. Para obtener más información sobre las vistas de catálogo, vea [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md).

 Tenga en cuenta los siguientes aspectos:

> [!IMPORTANT]
> La única manera confiable de localizar al propietario de un objeto consiste en consultar la vista de catálogo **sys.objects**. La única manera confiable de localizar al propietario de un tipo consiste en utilizar la función TYPEPROPERTY.

## <a name="special-cases-and-conditions"></a>Condiciones y casos especiales

En la siguiente tabla se enumeran los casos especiales, las excepciones y las condiciones que se aplican para modificar la autorización.

|Clase|Condición|
|-----------|---------------|
|OBJECT|No se puede cambiar la propiedad de desencadenadores, restricciones, reglas, valores predeterminados, estadísticas, objetos del sistema, colas, vistas indizadas o tablas con vistas indizadas.|
|SCHEMA|Cuando se transfiere la propiedad, se quitarán los permisos en objetos contenidos en esquemas que no tengan propietarios explícitos. No se puede cambiar el propietario de sys, dbo o information_schema.|
|TYPE|No se puede cambiar la propiedad de un TYPE que pertenece a sys o information_schema.|
|CONTRACT, MESSAGE TYPE o SERVICE|No se puede cambiar la propiedad de entidades del sistema.|
|SYMMETRIC KEY|No se puede cambiar la propiedad de claves globales temporales.|
|CERTIFICATE o ASYMMETRIC KEY|No se puede transferir la propiedad de estas entidades a un rol o un grupo.|
|ENDPOINT|La entidad de seguridad debe ser un inicio de sesión.|

## <a name="alter-authorization-for-databases"></a> ALTER AUTHORIZATION para bases de datos

### <a name="for-sql-server"></a>Para SQL Server

**Requisitos del nuevo propietario:** La nueva entidad propietaria debe ser una de las siguientes:

- Un inicio de sesión para la autenticación de SQL Server.
- Un inicio de sesión para la autenticación de Windows que represente un usuario de Windows (no un grupo).
- Un usuario de Windows que se autentica a través de un inicio de sesión de autenticación de Windows que representa un grupo de Windows.

**Requisitos de la persona que ejecuta la instrucción ALTER AUTHORIZATION:** Si no es un miembro del rol fijo de servidor **sysadmin**, debe tener al menos el permiso TAKE OWNERSHIP en la base de datos y el permiso IMPERSONATE en el nuevo inicio de sesión de propietario.

### <a name="for-azure-sql-database"></a>En Azure SQL Database

**Requisitos del nuevo propietario:** La nueva entidad propietaria debe ser una de las siguientes:

- Un inicio de sesión para la autenticación de SQL Server.
- Un usuario federado (no un grupo) presente en Azure AD.
- Un usuario administrado (no un grupo) o una aplicación presente en Azure AD.

Si el nuevo propietario es un usuario de Azure Active Directory, no puede existir como usuario en la base de datos cuyo nuevo propietario se convertirá en el nuevo propietario de la base de datos. Primero hay que quitar a este usuario de Azure AD de la base de datos para poder ejecutar la instrucción ALTER AUTHORIZATION cambiando la propiedad de base de datos al nuevo usuario. Para obtener más información sobre cómo configurar usuarios de Azure Active Directory con SQL Database, consulte [Conexión a SQL Database o [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] mediante la autenticación de Azure Active Directory](https://azure.microsoft.com/documentation/articles/sql-database-aad-authentication/).

**Requisitos de la persona que ejecuta la instrucción ALTER AUTHORIZATION:** Debe conectarse a la base de datos de destino para cambiar el propietario de dicha base de datos.

El propietario de una base de datos se puede cambiar en los siguientes tipos de cuentas.

- El inicio de sesión de la entidad de seguridad a nivel de servicio. (El administrador de SQL Azure proporcionado cuando se creó el servidor de SQL Database).
- El administrador de Active Directory para Azure SQL Server.
- El propietario actual de la base de datos.

En la tabla siguiente se resumen los requisitos:

Ejecutor  |Destino  |Resultado
---------|---------|---------
Inicio de sesión para la autenticación de SQL Server  |Inicio de sesión para la autenticación de SQL Server|Correcto
Inicio de sesión para la autenticación de SQL Server  |Usuario de Azure AD|Incorrecto
Usuario de Azure AD  |Inicio de sesión para la autenticación de SQL Server|Correcto
Usuario de Azure AD  |Usuario de Azure AD|Correcto

Para verificar un propietario de Azure AD de la base de datos, ejecute el siguiente comando Transact-SQL en una base de datos de usuario (en este ejemplo, `testdb`).

```sql
SELECT CAST(owner_sid as uniqueidentifier) AS Owner_SID
FROM sys.databases
WHERE name = 'testdb';
```

La salida será un identificador (por ejemplo, 6D8B81F6-7C79-444C-8858-4AF896C03C67) que corresponde al elemento ObjectID de Azure AD asignado a `richel@cqclinic.onmicrosoft.com`. Cuando un usuario con inicio de sesión de autenticación de SQL Server sea el propietario de la base de datos, ejecute la siguiente instrucción en la base de datos maestra para comprobar el propietario de la base de datos:

```sql
SELECT d.name, d.owner_sid, sl.name
FROM sys.databases AS d
JOIN sys.sql_logins AS sl
ON d.owner_sid = sl.sid;

```

### <a name="best-practice"></a>Procedimiento recomendado

En lugar de utilizar a los usuarios de Azure AD como propietarios individuales de la base de datos, utilice un grupo de Azure AD como miembro del rol fijo de base de datos **db_owner**. En los pasos siguientes se muestra cómo configurar un inicio de sesión deshabilitado como propietario de la base de datos y cómo convertir un grupo de Azure Active Directory (`mydbogroup`) en un miembro del rol **db_owner**.

1. Inicie sesión en SQL Server como administrador de Azure AD y cambie el propietario de la base de datos a un inicio de sesión deshabilitado para la autenticación de SQL Server. Por ejemplo, desde la base de datos de usuario, ejecute:

   ```sql
   ALTER AUTHORIZATION ON database::testdb TO DisabledLogin;
   ```

1. Cree un grupo de Azure AD que posea la base de datos y agréguelo como usuario a la base de datos de usuario. Por ejemplo:

   ```sql
   CREATE USER [mydbogroup] FROM EXTERNAL PROVIDER;
   ```

1. En la base de datos de usuario, agregue el usuario que representa el grupo de Azure AD al rol fijo de base de datos **db_owner**. Por ejemplo:

   ```sql
   ALTER ROLE db_owner ADD MEMBER mydbogroup;
   ```

Ahora, los miembros `mydbogroup` pueden administrar centralmente la base de datos como miembros del rol **db_owner**.

- Cuando los miembros de este grupo se quitan del grupo de Azure AD, pierden automáticamente los permisos de propietarios para esta base de datos.
- Asimismo, si se agregan nuevos miembros al grupo de Azure AD `mydbogroup`, estos obtienen automáticamente acceso de propietarios para esta base de datos.

Para comprobar si un usuario específico tiene el permiso de propietario de base de datos en vigor, pídale que ejecute la instrucción siguiente:

```sql
SELECT IS_MEMBER ('db_owner');
```

Si el valor devuelto es 1, significa que el usuario es un miembro del rol.

## <a name="permissions"></a>Permisos

Requiere el permiso TAKE OWNERSHIP en la entidad. Si el propietario no es el usuario que ejecuta esta instrucción, también se necesita, 1) el permiso IMPERSONATE en el nuevo propietario si es un usuario o inicio de sesión; o 2) si el nuevo propietario es un rol, debe pertenecer al rol, o el permiso ALTER en el rol; o 3) si el nuevo propietario es un rol de aplicación, el permiso ALTER en el rol de aplicación.

## <a name="examples"></a>Ejemplos

### <a name="a-transfer-ownership-of-a-table"></a>A. Transferir la propiedad de una tabla

En el siguiente ejemplo se transfiere la propiedad de la tabla `Sprockets` al usuario `MichikoOsada`. La tabla se encuentra dentro del esquema `Parts`.

```sql
ALTER AUTHORIZATION ON OBJECT::Parts.Sprockets TO MichikoOsada;
GO
```

La consulta también podría ser similar a la siguiente:

```sql
ALTER AUTHORIZATION ON Parts.Sprockets TO MichikoOsada;
GO
```

Si el esquema de los objetos no se incluye como parte de la instrucción, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] buscará el objeto en el esquema predeterminado de los usuarios. Por ejemplo:

```sql
ALTER AUTHORIZATION ON Sprockets TO MichikoOsada;
ALTER AUTHORIZATION ON OBJECT::Sprockets TO MichikoOsada;
```

### <a name="b-transfer-ownership-of-a-view-to-the-schema-owner"></a>B. Transferir la propiedad de una vista al propietario del esquema

En el siguiente ejemplo se transfiere la propiedad de la vista `ProductionView06` al propietario del esquema que la contiene. La vista se encuentra dentro del esquema `Production`.

```sql
ALTER AUTHORIZATION ON OBJECT::Production.ProductionView06 TO SCHEMA OWNER;
GO
```

### <a name="c-transfer-ownership-of-a-schema-to-a-user"></a>C. Transferir la propiedad de un esquema a un usuario

En el siguiente ejemplo se transfiere la propiedad del esquema `SeattleProduction11` al usuario `SandraAlayo`.

```sql
ALTER AUTHORIZATION ON SCHEMA::SeattleProduction11 TO SandraAlayo;
GO
```

### <a name="d-transfer-ownership-of-an-endpoint-to-a-sql-server-login"></a>D. Transferir la propiedad de un extremo a un inicio de sesión de SQL Server

En el siguiente ejemplo se transfiere la propiedad del extremo `CantabSalesServer1` a `JaePak`. Puesto que el extremo es un elemento protegible de nivel de servidor, el extremo solo puede transferirse a una entidad de seguridad de nivel de servidor.

**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.

```sql
ALTER AUTHORIZATION ON ENDPOINT::CantabSalesServer1 TO JaePak;
GO
```

### <a name="e-changing-the-owner-of-a-table"></a>E. Cambiar el propietario de una tabla

En todos los ejemplos siguientes se cambia el propietario de la tabla `Sprockets` en la base de datos `Parts` por el usuario de base de datos `MichikoOsada`.

```sql
ALTER AUTHORIZATION ON Sprockets TO MichikoOsada;
ALTER AUTHORIZATION ON dbo.Sprockets TO MichikoOsada;
ALTER AUTHORIZATION ON OBJECT::Sprockets TO MichikoOsada;
ALTER AUTHORIZATION ON OBJECT::dbo.Sprockets TO MichikoOsada;
```

### <a name="f-changing-the-owner-of-a-database"></a>F. Cambiar el propietario de una base de datos

 **SE APLICA A**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores, [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]

 En el ejemplo siguiente, se cambia el propietario de la base de datos `Parts` por el inicio de sesión `MichikoOsada`.

```sql
ALTER AUTHORIZATION ON DATABASE::Parts TO MichikoOsada;
```

### <a name="g-changing-the-owner-of-a-sql-database-to-an-azure-ad-user"></a>G. Cambiar el propietario de SQL Database por un usuario de Azure AD

En el ejemplo siguiente, un administrador de Azure Active Directory para SQL Server en una organización con un Active Directory denominado `cqclinic.onmicrosoft.com` puede cambiar la propiedad actual de una base de datos `targetDB` y convertir a un usuario de AAD `richel@cqclinic.onmicorsoft.com` en el nuevo propietario de la base de datos con el comando siguiente:

```sql
ALTER AUTHORIZATION ON database::targetDB TO [rachel@cqclinic.onmicrosoft.com];
```

En Azure AD, el nombre de usuario debe incluirse entre corchetes `[]`.

## <a name="see-also"></a>Consulte también
 [OBJECTPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/objectproperty-transact-sql.md) [TYPEPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/typeproperty-transact-sql.md) [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)

