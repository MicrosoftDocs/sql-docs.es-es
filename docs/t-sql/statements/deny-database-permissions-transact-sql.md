---
description: DENY (permisos de base de datos de Transact-SQL)
title: DENY (permisos de base de datos de Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/21/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- DENY statement, databases
- permissions [SQL Server], databases
- database permissions [SQL Server], denying
- denying permissions [SQL Server], databases
ms.assetid: 36cc4e2c-5a24-4975-9920-9305f12c6e7c
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1287d86ceeab338a1feaedbd7b0be694021a70fe
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "100342547"
---
# <a name="deny-database-permissions-transact-sql"></a>DENY (permisos de base de datos de Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Deniega permisos en una base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Sintaxis

```syntaxsql
DENY <permission> [ ,...n ]
    TO <database_principal> [ ,...n ] [ CASCADE ]
    [ AS <database_principal> ]

<permission> ::=
    permission | ALL [ PRIVILEGES ]

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

*permission* Especifica un permiso que puede denegarse en una base de datos. Para obtener una lista de permisos, vea la sección Comentarios que se muestra posteriormente en este tema.

ALL Esta opción no deniega todos los permisos posibles. Denegar ALL es equivalente a denegar los permisos siguientes: BACKUP DATABASE, BACKUP LOG, CREATE DATABASE, CREATE DEFAULT, CREATE FUNCTION, CREATE PROCEDURE, CREATE RULE, CREATE TABLE y CREATE VIEW.

PRIVILEGES Incluido por compatibilidad con ISO. No cambia el comportamiento de ALL.

CASCADE Indica que el permiso también se denegará a las entidades de seguridad para las que la entidad de seguridad especificada lo concedió.

AS \<database_principal> Especifica una entidad de seguridad desde la que la entidad de seguridad que ejecuta esta consulta deriva su derecho de denegar el permiso.

*Database_user* Especifica un usuario de base de datos.

*Database_role* Especifica un rol de la base de datos.

*Application_role*
**Se aplica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]

Especifica un rol de aplicación.

*Database_user_mapped_to_Windows_User* Especifica un usuario de base de datos asignado a un usuario de Windows.

*Database_user_mapped_to_Windows_Group* Especifica un usuario de base de datos asignado a un grupo de Windows.

*Database_user_mapped_to_certificate* Especifica un usuario de base de datos asignado a un certificado.

*Database_user_mapped_to_asymmetric_key* Especifica un usuario de base de datos asignado a una clave asimétrica.

*Database_user_with_no_login* Especifica un usuario de base de datos sin entidad de seguridad de servidor correspondiente.

## <a name="remarks"></a>Observaciones

Una base de datos es un elemento protegible que contiene el servidor, que es su entidad primaria en la jerarquía de permisos. La mayoría de permisos limitados y específicos que se pueden denegar en una base de datos se muestran en la siguiente tabla, junto con permisos más generales que los incluyen por implicación.

|Permiso de base de datos|Implícito en el permiso de base de datos|Implícito en el permiso de servidor|
|-------------------------|------------------------------------|----------------------------------|
|ADMINISTER DATABASE BULK OPERATIONS<br/>**Se aplica a:** [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)].|CONTROL|CONTROL SERVER|
|ALTER|CONTROL|ALTER ANY DATABASE|
|ALTER ANY APPLICATION ROLE|ALTER|CONTROL SERVER|
|ALTER ANY ASSEMBLY|ALTER|CONTROL SERVER|
|ALTER ANY ASYMMETRIC KEY|ALTER|CONTROL SERVER|
|ALTER ANY CERTIFICATE|ALTER|CONTROL SERVER|
|ALTER ANY COLUMN ENCRYPTION KEY|ALTER|CONTROL SERVER|
|ALTER ANY COLUMN MASTER KEY DEFINITION|ALTER|CONTROL SERVER|
|ALTER ANY CONTRACT|ALTER|CONTROL SERVER|
|ALTER ANY DATABASE AUDIT|ALTER|ALTER ANY SERVER AUDIT|
|ALTER ANY DATABASE DDL TRIGGER|ALTER|CONTROL SERVER|
|ALTER ANY DATABASE EVENT NOTIFICATION|ALTER|ALTER ANY EVENT NOTIFICATION|
|ALTER ANY DATABASE EVENT SESSION<br /> **Se aplica a**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|ALTER|ALTER ANY EVENT SESSION|
|ALTER ANY DATABASE SCOPED CONFIGURATION<br /> **Se aplica a**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores, [!INCLUDE[ssSDS](../../includes/sssds-md.md)]|CONTROL|CONTROL SERVER|
|ALTER ANY DATASPACE|ALTER|CONTROL SERVER|
|ALTER ANY EXTERNAL DATA SOURCE|ALTER|CONTROL SERVER|
|ALTER ANY EXTERNAL FILE FORMAT|ALTER|CONTROL SERVER|
|ALTER ANY EXTERNAL LIBRARY <br /> **Se aplica a**: [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)].|CONTROL|CONTROL SERVER |
|ALTER ANY FULLTEXT CATALOG|ALTER|CONTROL SERVER|
|ALTER ANY MASK|CONTROL|CONTROL SERVER|
|ALTER ANY MESSAGE TYPE|ALTER|CONTROL SERVER|
|ALTER ANY REMOTE SERVICE BINDING|ALTER|CONTROL SERVER|
|ALTER ANY ROLE|ALTER|CONTROL SERVER|
|ALTER ANY ROUTE|ALTER|CONTROL SERVER|
|ALTER ANY SECURITY POLICY<br /> **Se aplica a**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|CONTROL|CONTROL SERVER|
|ALTER ANY SCHEMA|ALTER|CONTROL SERVER|
|ALTER ANY SERVICE|ALTER|CONTROL SERVER|
|ALTER ANY SYMMETRIC KEY|ALTER|CONTROL SERVER|
|ALTER ANY USER|ALTER|CONTROL SERVER|
|AUTHENTICATE|CONTROL|AUTHENTICATE SERVER|
|BACKUP DATABASE|CONTROL|CONTROL SERVER|
|BACKUP LOG|CONTROL|CONTROL SERVER|
|CHECKPOINT|CONTROL|CONTROL SERVER|
|CONNECT|CONNECT REPLICATION|CONTROL SERVER|
|CONNECT REPLICATION|CONTROL|CONTROL SERVER|
|CONTROL|CONTROL|CONTROL SERVER|
|CREATE AGGREGATE|ALTER|CONTROL SERVER|
|CREATE ASSEMBLY|ALTER ANY ASSEMBLY|CONTROL SERVER|
|CREATE ASYMMETRIC KEY|ALTER ANY ASYMMETRIC KEY|CONTROL SERVER|
|CREATE CERTIFICATE|ALTER ANY CERTIFICATE|CONTROL SERVER|
|CREATE CONTRACT|ALTER ANY CONTRACT|CONTROL SERVER|
|CREATE DATABASE|CONTROL|CREATE ANY DATABASE|
|CREATE DATABASE DDL EVENT NOTIFICATION|ALTER ANY DATABASE EVENT NOTIFICATION|CREATE DDL EVENT NOTIFICATION|
|CREATE DEFAULT|ALTER|CONTROL SERVER|
|CREATE FULLTEXT CATALOG|ALTER ANY FULLTEXT CATALOG|CONTROL SERVER|
|CREATE FUNCTION|ALTER|CONTROL SERVER|
|CREATE MESSAGE TYPE|ALTER ANY MESSAGE TYPE|CONTROL SERVER|
|CREATE PROCEDURE|ALTER|CONTROL SERVER|
|CREATE QUEUE|ALTER|CONTROL SERVER|
|CREATE REMOTE SERVICE BINDING|ALTER ANY REMOTE SERVICE BINDING|CONTROL SERVER|
|CREATE ROLE|ALTER ANY ROLE|CONTROL SERVER|
|CREATE ROUTE|ALTER ANY ROUTE|CONTROL SERVER|
|CREATE RULE|ALTER|CONTROL SERVER|
|CREATE SCHEMA|ALTER ANY SCHEMA|CONTROL SERVER|
|CREATE SERVICE|ALTER ANY SERVICE|CONTROL SERVER|
|CREATE SYMMETRIC KEY|ALTER ANY SYMMETRIC KEY|CONTROL SERVER|
|CREATE SYNONYM|ALTER|CONTROL SERVER|
|CREATE TABLE|ALTER|CONTROL SERVER|
|CREATE TYPE|ALTER|CONTROL SERVER|
|CREATE VIEW|ALTER|CONTROL SERVER|
|CREATE XML SCHEMA COLLECTION|ALTER|CONTROL SERVER|
|Delete|CONTROL|CONTROL SERVER|
|Ejecute|CONTROL|CONTROL SERVER|
|EXECUTE ANY EXTERNAL SCRIPT <br /> **Se aplica a**: [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)].|CONTROL|CONTROL SERVER|
|INSERT|CONTROL|CONTROL SERVER|
|KILL DATABASE CONNECTION<br /> **Se aplica a**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|CONTROL|ALTER ANY CONNECTION|
|REFERENCES|CONTROL|CONTROL SERVER|
|SELECT|CONTROL|CONTROL SERVER|
|SHOWPLAN|CONTROL|ALTER TRACE|
|SUBSCRIBE QUERY NOTIFICATIONS|CONTROL|CONTROL SERVER|
|TAKE OWNERSHIP|CONTROL|CONTROL SERVER|
|UNMASK|CONTROL|CONTROL SERVER|
|UPDATE|CONTROL|CONTROL SERVER|
|VIEW ANY COLUMN ENCRYPTION KEY|CONTROL|VIEW ANY DEFINITION|
|VIEW ANY MASTER KEY DEFINITION|CONTROL|VIEW ANY DEFINITION|
|VIEW DATABASE STATE|CONTROL|VIEW SERVER STATE|
|VIEW DEFINITION|CONTROL|VIEW ANY DEFINITION|

## <a name="permissions"></a>Permisos

La entidad de seguridad que ejecuta esta instrucción (o la entidad de seguridad especificada con la opción AS) debe disponer del permiso CONTROL en la base de datos o un permiso superior que implique el permiso CONTROL en la base de datos.

Si utiliza la opción AS, la entidad de seguridad especificada debe ser propietaria de la base de datos.

## <a name="examples"></a>Ejemplos

### <a name="a-denying-permission-to-create-certificates"></a>A. Denegar el permiso para crear certificados

En el siguiente ejemplo se deniega el permiso `CREATE CERTIFICATE` para la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] al usuario `MelanieK`.

```sql
USE AdventureWorks2012;
DENY CREATE CERTIFICATE TO MelanieK;
GO
```

### <a name="b-denying-references-permission-to-an-application-role"></a>B. Denegar el permiso REFERENCES para un rol de aplicación

En el siguiente ejemplo se deniega el permiso `REFERENCES` para la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] al rol de aplicación `AuditMonitor`.

**Se aplica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]

```sql
USE AdventureWorks2012;
DENY REFERENCES TO AuditMonitor;
GO
```

### <a name="c-denying-view-definition-with-cascade"></a>C. Denegar VIEW DEFINITION con CASCADE

En el siguiente ejemplo se deniega el permiso `VIEW DEFINITION` para la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] al usuario `CarmineEs` y a todas las entidades de seguridad a las que `CarmineEs` concedió el permiso `VIEW DEFINITION`.

```sql
USE AdventureWorks2012;
DENY VIEW DEFINITION TO CarmineEs CASCADE;
GO
```

## <a name="see-also"></a>Consulte también

- [sys.database_permissions](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md)
- [sys.database_principals](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)
- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)
- [GRANT](../../t-sql/statements/grant-transact-sql.md)
- [Permisos](../../relational-databases/security/permissions-database-engine.md)
- [Entidades de seguridad](../../relational-databases/security/authentication-access/principals-database-engine.md)
