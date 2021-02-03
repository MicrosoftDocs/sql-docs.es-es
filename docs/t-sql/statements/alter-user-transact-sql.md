---
description: ALTER USER (Transact-SQL)
title: ALTER USER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/10/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_USER_TSQL
- ALTER USER
dev_langs:
- TSQL
helpviewer_keywords:
- modifying database users
- ALTER USER statement
- renaming databases
- schemas [SQL Server], default
- database user names [SQL Server]
- names [SQL Server], database users
- default schemas
- modifying default schemas
ms.assetid: 344fc6ce-a008-47c8-a02e-47fae66cc590
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 9f10913c23d8ab4b395bea53d1aadb7fbd8d3009
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237023"
---
# <a name="alter-user-transact-sql"></a>ALTER USER (Transact-SQL)

Cambia el nombre de un usuario de base de datos o cambia su esquema predeterminado.

 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [SQL Database](alter-user-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Instancia administrada de SQL](alter-user-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-user-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](alter-user-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server"></a>SQL Server

## <a name="syntax"></a>Sintaxis

```syntaxsql
-- Syntax for SQL Server

ALTER USER userName
 WITH <set_item> [ ,...n ]
[;]

<set_item> ::=
NAME = newUserName
| DEFAULT_SCHEMA = { schemaName | NULL }
| LOGIN = loginName
| PASSWORD = 'password' [ OLD_PASSWORD = 'oldpassword' ]
| DEFAULT_LANGUAGE = { NONE | <lcid> | <language name> | <language alias> }
| ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | OFF ]
```

## <a name="arguments"></a>Argumentos

 *userName* especifica el nombre por el que se identifica al usuario en esta base de datos.

 LOGIN **=** _loginName_ reasigna un usuario a otro inicio de sesión cambiando el identificador de seguridad (SID) del usuario para que coincida con el SID de inicio de sesión.

 NAME **=** _newUserName_ especifica el nuevo nombre de este usuario. *newUserName* no debe existir en la base de datos actual.

 DEFAULT_SCHEMA **=** { *schemaName* | NULL } especifica el primer esquema donde buscará el servidor cuando resuelva los nombres de objetos de este usuario. El establecimiento del esquema predeterminado en NULL quita un esquema predeterminado de un grupo de Windows. La opción NULL no se puede utilizar con un usuario de Windows.

 PASSWORD **=** '*password*'  **Se aplica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y posterior, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].

 Especifica la contraseña del usuario que se está cambiando. En las contraseñas se distingue entre mayúsculas y minúsculas.

> [!NOTE]
> Esta opción solo está disponible para los usuarios contenidos. Para más información, vea [Bases de datos independientes](../../relational-databases/databases/contained-databases.md) y [sp_migrate_user_to_contained &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md).

 OLD_PASSWORD **=** _'oldpassword'_ **Se aplica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y posterior, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].

 La contraseña de usuario actual que se reemplazará por '*password*'. En las contraseñas se distingue entre mayúsculas y minúsculas. Para cambiar una contraseña se pide *OLD_PASSWORD*, a menos que tenga el permiso **ALTER ANY USER**. Al pedir que se especifique *OLD_PASSWORD*, se impide que los usuarios con el permiso **IMPERSONATION** puedan cambiar la contraseña.

> [!NOTE]
> Esta opción solo está disponible para los usuarios contenidos.

 DEFAULT_LANGUAGE **=** _{ NONE \| \<lcid> \| \<language name> \| \<language alias> }_ **Se aplica a** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.

 Especifica el idioma predeterminado que debe asignarse al usuario. Si esta opción se establece en NONE, el idioma predeterminado se establece en el de la base de datos. Si el idioma predeterminado de la base de datos se cambia más tarde, el idioma predeterminado del usuario no se modificará. *DEFAULT_LANGUAGE* puede ser el identificador local (lcid), el nombre del idioma o el alias del idioma.

> [!NOTE]
> Esta opción solo se puede especificar en una base de datos independiente y solo para los usuarios independientes.

 ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | **OFF** ]  **Se aplica a**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y posterior, [!INCLUDE[ssSDS](../../includes/sssds-md.md)].

 Suprime las comprobaciones de metadatos criptográficos en el servidor en operaciones de copia masiva. De esta manera, el usuario puede copiar los datos de forma masiva entre tablas o bases de datos, sin descifrar los datos. El valor predeterminado es OFF.

> [!WARNING]
> Si esta opción no se utiliza adecuadamente, pueden dañarse los datos. Para obtener más información, vea [Migración de datos confidenciales protegidos mediante Always Encrypted](../../relational-databases/security/encryption/migrate-sensitive-data-protected-by-always-encrypted.md).

## <a name="remarks"></a>Observaciones

 El esquema predeterminado será el primer esquema donde buscará el servidor cuando resuelva los nombres de objetos de este usuario de base de datos. A menos que se especifique lo contrario, el esquema predeterminado será el propietario de los objetos creados por este usuario de base de datos.

 Si el usuario tiene un esquema predeterminado, se utilizará dicho esquema. Si el usuario no tiene un esquema predeterminado, pero es miembro de un grupo con un esquema predeterminado, se utilizará el esquema predeterminado del grupo. Si el usuario no tiene un esquema predeterminado y es miembro de varios grupos, el esquema predeterminado para el usuario será el del grupo de Windows con el principal_id mínimo y un esquema predeterminado establecido explícitamente. Si no se puede determinar ningún esquema predeterminado para un usuario, se utilizará el esquema **dbo**.

 DEFAULT_SCHEMA puede establecerse en un esquema que no existe actualmente en la base de datos. Por tanto, puede asignar un DEFAULT_SCHEMA a un usuario antes de crear el esquema.

 No se puede especificar DEFAULT_SCHEMA para un usuario asignado a un certificado o una clave asimétrica.

> [!IMPORTANT]
> El valor de DEFAULT_SCHEMA se omite si el usuario es un miembro del rol fijo de servidor **sysadmin**. Todos los miembros del rol fijo de servidor **sysadmin** tienen un esquema predeterminado `dbo`.

 Solo puede cambiar el nombre de un usuario que está asignado a un grupo o inicio de sesión de Windows cuando el SID del nuevo nombre de usuario coincide con el SID registrado en la base de datos. Esta comprobación ayuda a evitar la suplantación de inicios de sesión de Windows en la base de datos.

 La cláusula WITH LOGIN habilita la reasignación de un usuario a un inicio de sesión diferente. Los usuarios sin inicio de sesión, los usuarios asignados a un certificado y los usuarios asignados a una clave asimétrica no se pueden reasignar con esta cláusula. Solo se pueden reasignar usuarios de SQL y usuarios (o grupos) de Windows. La cláusula WITH LOGIN no se puede utilizar para cambiar el tipo de usuario, como cambiar una cuenta de Windows a un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

 El nombre del usuario se cambiará automáticamente por el nombre de inicio de sesión si se cumplen las condiciones siguientes.

- El usuario es un usuario de Windows.

- El nombre es un nombre de Windows (contiene una barra diagonal inversa).

- No se ha especificado ningún nombre nuevo.

- El nombre actual difiere del nombre de inicio de sesión.

 En caso contrario, no se cambiará el nombre del usuario a menos que el autor de las llamadas invoque también la cláusula NAME.

El nombre de un usuario asignado a un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un certificado o una clave asimétrica no puede contener el carácter de la barra diagonal inversa (\\).

> [!CAUTION]
> [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]

## <a name="security"></a>Seguridad

> [!NOTE]
> Un usuario que tenga permiso **ALTER ANY USER** puede cambiar el esquema predeterminado de cualquier usuario. Un usuario que tenga un esquema modificado podría seleccionar datos de la tabla incorrecta o ejecutar código del esquema incorrecto sin saberlo.

### <a name="permissions"></a>Permisos

 Para cambiar el nombre de un usuario se necesita el permiso **ALTER ANY USER** en la base de datos.

 Para cambiar la información de inicio de sesión de destino de un usuario, es necesario contar con el permiso **CONTROL** en la base de datos.

 Para cambiar el nombre de usuario de un usuario con el permiso **CONTROL** en la base de datos, se necesita el permiso **CONTROL** en la base de datos.

 Para cambiar el idioma o el esquema predeterminado, se necesita el permiso **ALTER** en el usuario. Los usuarios pueden cambiar el idioma y el esquema predeterminados.

## <a name="examples"></a>Ejemplos

Todos los ejemplos se ejecutan en una base de datos de usuario.

### <a name="a-changing-the-name-of-a-database-user"></a>A. Cambiar el nombre de usuario de una base de datos

 En el ejemplo siguiente se cambia el nombre del usuario de la base de datos `Mary5` a `Mary51`.

```sql
ALTER USER Mary5 WITH NAME = Mary51;
GO
```

### <a name="b-changing-the-default-schema-of-a-user"></a>B. Cambiar el esquema predeterminado de un usuario

 En el ejemplo siguiente se cambia el esquema predeterminado del usuario de `Mary51` a `Purchasing`.

```sql
ALTER USER Mary51 WITH DEFAULT_SCHEMA = Purchasing;
GO
```

### <a name="c-changing-several-options-at-once"></a>C. Cambiar varias opciones a la vez

 En el siguiente ejemplo se cambian varias opciones para un usuario de base de datos independiente en una instrucción.

**Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.

```sql
ALTER USER Philip
WITH NAME = Philipe
, DEFAULT_SCHEMA = Development
, PASSWORD = 'W1r77TT98%ab@#' OLD_PASSWORD = 'New Devel0per'
, DEFAULT_LANGUAGE= French ;
GO
```

## <a name="see-also"></a>Consulte también

- [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)
- [DROP USER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-user-transact-sql.md)
- [Bases de datos independientes](../../relational-databases/databases/contained-databases.md)
- [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)
- [sp_migrate_user_to_contained &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md)

::: moniker-end
::: moniker range="=azuresqldb-current"

:::row:::
    :::column:::
        [SQL Server](alter-user-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* SQL Database \*_**
    :::column-end:::
    :::column:::
        [Instancia administrada de SQL](alter-user-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-user-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](alter-user-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-database"></a>SQL Database

## <a name="syntax"></a>Sintaxis

```syntaxsql
-- Syntax for Azure SQL Database

ALTER USER userName
 WITH <set_item> [ ,...n ]

<set_item> ::=
NAME = newUserName
| DEFAULT_SCHEMA = schemaName
| LOGIN = loginName
| ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | OFF ]
[;]

-- Azure SQL Database Update Syntax
ALTER USER userName
 WITH <set_item> [ ,...n ]
[;]

<set_item> ::=
NAME = newUserName
| DEFAULT_SCHEMA = { schemaName | NULL }
| LOGIN = loginName
| PASSWORD = 'password' [ OLD_PASSWORD = 'oldpassword' ]
| ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | OFF ]

-- SQL Database syntax when connected to a federation member
ALTER USER userName
 WITH <set_item> [ ,... n ]
[;]

<set_item> ::=
 NAME = newUserName
```

## <a name="arguments"></a>Argumentos

 *userName* especifica el nombre por el que se identifica al usuario en esta base de datos.

 LOGIN **=** _loginName_ reasigna un usuario a otro inicio de sesión cambiando el identificador de seguridad (SID) del usuario para que coincida con el SID de inicio de sesión.

 Si la instrucción ALTER USER es la única instrucción en un lote SQL, Azure SQL Database admite la cláusula WITH LOGIN. Si la instrucción ALTER USER no es la única instrucción en un lote SQL ni se ejecuta en SQL dinámico, la cláusula WITH LOGIN no se admite.

 NAME **=** _newUserName_ especifica el nuevo nombre de este usuario. *newUserName* no debe existir en la base de datos actual.

 DEFAULT_SCHEMA **=** { *schemaName* | NULL } especifica el primer esquema donde buscará el servidor cuando resuelva los nombres de objetos de este usuario. El establecimiento del esquema predeterminado en NULL quita un esquema predeterminado de un grupo de Windows. La opción NULL no se puede utilizar con un usuario de Windows.

 PASSWORD **=** '*password*'  **Se aplica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y posterior, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].

 Especifica la contraseña del usuario que se está cambiando. En las contraseñas se distingue entre mayúsculas y minúsculas.

> [!NOTE]
> Esta opción solo está disponible para los usuarios contenidos. Para más información, vea [Bases de datos independientes](../../relational-databases/databases/contained-databases.md) y [sp_migrate_user_to_contained &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md).

 OLD_PASSWORD **=** _'oldpassword'_ **Se aplica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y posterior, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].

 La contraseña de usuario actual que se reemplazará por '*password*'. En las contraseñas se distingue entre mayúsculas y minúsculas. Para cambiar una contraseña se pide *OLD_PASSWORD*, a menos que tenga el permiso **ALTER ANY USER**. Al pedir que se especifique *OLD_PASSWORD*, se impide que los usuarios con el permiso **IMPERSONATION** puedan cambiar la contraseña.

> [!NOTE]
> Esta opción solo está disponible para los usuarios contenidos.

 ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | **OFF** ]  **Se aplica a**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y posterior, [!INCLUDE[ssSDS](../../includes/sssds-md.md)].

 Suprime las comprobaciones de metadatos criptográficos en el servidor en operaciones de copia masiva. De esta manera, el usuario puede copiar los datos de forma masiva entre tablas o bases de datos, sin descifrar los datos. El valor predeterminado es OFF.

> [!WARNING]
> Si esta opción no se utiliza adecuadamente, pueden dañarse los datos. Para obtener más información, vea [Migración de datos confidenciales protegidos mediante Always Encrypted](../../relational-databases/security/encryption/migrate-sensitive-data-protected-by-always-encrypted.md).

## <a name="remarks"></a>Observaciones

 El esquema predeterminado será el primer esquema donde buscará el servidor cuando resuelva los nombres de objetos de este usuario de base de datos. A menos que se especifique lo contrario, el esquema predeterminado será el propietario de los objetos creados por este usuario de base de datos.

 Si el usuario tiene un esquema predeterminado, se utilizará dicho esquema. Si el usuario no tiene un esquema predeterminado, pero es miembro de un grupo con un esquema predeterminado, se utilizará el esquema predeterminado del grupo. Si el usuario no tiene un esquema predeterminado y es miembro de varios grupos, el esquema predeterminado para el usuario será el del grupo de Windows con el principal_id mínimo y un esquema predeterminado establecido explícitamente. Si no se puede determinar ningún esquema predeterminado para un usuario, se utilizará el esquema **dbo**.

 DEFAULT_SCHEMA puede establecerse en un esquema que no existe actualmente en la base de datos. Por tanto, puede asignar un DEFAULT_SCHEMA a un usuario antes de crear el esquema.

 No se puede especificar DEFAULT_SCHEMA para un usuario asignado a un certificado o una clave asimétrica.

> [!IMPORTANT]
> El valor de DEFAULT_SCHEMA se omite si el usuario es un miembro del rol fijo de servidor **sysadmin**. Todos los miembros del rol fijo de servidor **sysadmin** tienen un esquema predeterminado `dbo`.

 Solo puede cambiar el nombre de un usuario que está asignado a un grupo o inicio de sesión de Windows cuando el SID del nuevo nombre de usuario coincide con el SID registrado en la base de datos. Esta comprobación ayuda a evitar la suplantación de inicios de sesión de Windows en la base de datos.

 La cláusula WITH LOGIN habilita la reasignación de un usuario a un inicio de sesión diferente. Los usuarios sin inicio de sesión, los usuarios asignados a un certificado y los usuarios asignados a una clave asimétrica no se pueden reasignar con esta cláusula. Solo se pueden reasignar usuarios de SQL y usuarios (o grupos) de Windows. La cláusula WITH LOGIN no se puede utilizar para cambiar el tipo de usuario, como cambiar una cuenta de Windows a un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

 El nombre del usuario se cambiará automáticamente por el nombre de inicio de sesión si se cumplen las condiciones siguientes.

- El usuario es un usuario de Windows.

- El nombre es un nombre de Windows (contiene una barra diagonal inversa).

- No se ha especificado ningún nombre nuevo.

- El nombre actual difiere del nombre de inicio de sesión.

 En caso contrario, no se cambiará el nombre del usuario a menos que el autor de las llamadas invoque también la cláusula NAME.

El nombre de un usuario asignado a un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un certificado o una clave asimétrica no puede contener el carácter de la barra diagonal inversa (\\).

> [!CAUTION]
> [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]

## <a name="security"></a>Seguridad

> [!NOTE]
> Un usuario que tenga permiso **ALTER ANY USER** puede cambiar el esquema predeterminado de cualquier usuario. Un usuario que tenga un esquema modificado podría seleccionar datos de la tabla incorrecta o ejecutar código del esquema incorrecto sin saberlo.

### <a name="permissions"></a>Permisos

 Para cambiar el nombre de un usuario se necesita el permiso **ALTER ANY USER** en la base de datos.

 Para cambiar la información de inicio de sesión de destino de un usuario, es necesario contar con el permiso **CONTROL** en la base de datos.

 Para cambiar el nombre de usuario de un usuario con el permiso **CONTROL** en la base de datos, se necesita el permiso **CONTROL** en la base de datos.

 Para cambiar el idioma o el esquema predeterminado, se necesita el permiso **ALTER** en el usuario. Los usuarios pueden cambiar el idioma y el esquema predeterminados.

## <a name="examples"></a>Ejemplos

Todos los ejemplos se ejecutan en una base de datos de usuario.

### <a name="a-changing-the-name-of-a-database-user"></a>A. Cambiar el nombre de usuario de una base de datos

 En el ejemplo siguiente se cambia el nombre del usuario de la base de datos `Mary5` a `Mary51`.

```sql
ALTER USER Mary5 WITH NAME = Mary51;
GO
```

### <a name="b-changing-the-default-schema-of-a-user"></a>B. Cambiar el esquema predeterminado de un usuario

 En el ejemplo siguiente se cambia el esquema predeterminado del usuario de `Mary51` a `Purchasing`.

```sql
ALTER USER Mary51 WITH DEFAULT_SCHEMA = Purchasing;
GO
```

### <a name="c-changing-several-options-at-once"></a>C. Cambiar varias opciones a la vez

 En el siguiente ejemplo se cambian varias opciones para un usuario de base de datos independiente en una instrucción.

```sql
ALTER USER Philip
WITHNAME = Philipe
, DEFAULT_SCHEMA = Development
, PASSWORD = 'W1r77TT98%ab@#' OLD_PASSWORD = 'New Devel0per';
GO
```

## <a name="see-also"></a>Consulte también

- [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)
- [DROP USER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-user-transact-sql.md)
- [Bases de datos independientes](../../relational-databases/databases/contained-databases.md)
- [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)
- [sp_migrate_user_to_contained &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md)

::: moniker-end
::: moniker range="=azuresqldb-mi-current"

:::row:::
    :::column:::
        [SQL Server](alter-user-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL Database](alter-user-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* SQL Managed Instance \*_**
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-user-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](alter-user-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-sql-managed-instance"></a>Azure SQL Managed Instance

## <a name="syntax"></a>Sintaxis

> [!IMPORTANT]
> Solo se admiten las opciones siguientes para Azure SQL Managed Instance cuando se aplica a usuarios con inicios de sesión de Azure AD: `DEFAULT_SCHEMA = { schemaName | NULL }` y `DEFAULT_LANGUAGE = { NONE | lcid | language name | language alias }`.
> </br> </br> Hay una nueva extensión de sintaxis que se ha agregado para facilitar la reasignación de usuarios en una base de datos migrada a la Azure SQL Managed Instance. La sintaxis de ALTER USER ayuda a asignar usuarios de base de datos en un dominio federado y sincronizado con Azure AD a inicios de sesión de Azure AD.

```syntaxsql
-- Syntax for SQL Managed Instance
ALTER USER userName
 { WITH <set_item> [ ,...n ] | FROM EXTERNAL PROVIDER }
[;]

<set_item> ::=
NAME = newUserName
| DEFAULT_SCHEMA = { schemaName | NULL }
| LOGIN = loginName
| PASSWORD = 'password' [ OLD_PASSWORD = 'oldpassword' ]
| DEFAULT_LANGUAGE = { NONE | <lcid> | <language name> | <language alias> }
| ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | OFF ]

-- Users or groups that are migrated as federated and synchronized with Azure AD have the following syntax:

/** Applies to Windows users that were migrated and have the following user names:
- Windows user <domain\user>
- Windows group <domain\MyWindowsGroup>
- Windows alias <MyWindowsAlias>
**/

ALTER USER userName
 { WITH <set_item> [ ,...n ] | FROM EXTERNAL PROVIDER }
[;]

<set_item> ::=
 NAME = newUserName
| DEFAULT_SCHEMA = { schemaName | NULL }
| LOGIN = loginName
| DEFAULT_LANGUAGE = { NONE | <lcid> | <language name> | <language alias> }
```

## <a name="arguments"></a>Argumentos

 *userName* especifica el nombre por el que se identifica al usuario en esta base de datos.

 LOGIN **=** _loginName_ reasigna un usuario a otro inicio de sesión cambiando el identificador de seguridad (SID) del usuario para que coincida con el SID de inicio de sesión.

 Si la instrucción ALTER USER es la única instrucción en un lote SQL, Azure SQL Database admite la cláusula WITH LOGIN. Si la instrucción ALTER USER no es la única instrucción en un lote SQL ni se ejecuta en SQL dinámico, la cláusula WITH LOGIN no se admite.

 NAME **=** _newUserName_ especifica el nuevo nombre de este usuario. *newUserName* no debe existir en la base de datos actual.

 DEFAULT_SCHEMA **=** { *schemaName* | NULL } especifica el primer esquema donde buscará el servidor cuando resuelva los nombres de objetos de este usuario. El establecimiento del esquema predeterminado en NULL quita un esquema predeterminado de un grupo de Windows. La opción NULL no se puede utilizar con un usuario de Windows.

 PASSWORD **=** '*password*'

 Especifica la contraseña del usuario que se está cambiando. En las contraseñas se distingue entre mayúsculas y minúsculas.

> [!NOTE]
> Esta opción solo está disponible para los usuarios contenidos. Para más información, vea [Bases de datos independientes](../../relational-databases/databases/contained-databases.md) y [sp_migrate_user_to_contained &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md).

 OLD_PASSWORD **=** _'oldpassword'_

 La contraseña de usuario actual que se reemplazará por '*password*'. En las contraseñas se distingue entre mayúsculas y minúsculas. Para cambiar una contraseña se pide *OLD_PASSWORD*, a menos que tenga el permiso **ALTER ANY USER**. Al pedir que se especifique *OLD_PASSWORD*, se impide que los usuarios con el permiso **IMPERSONATION** puedan cambiar la contraseña.

> [!NOTE]
> Esta opción solo está disponible para los usuarios contenidos.

 DEFAULT_LANGUAGE **=** _{ NONE | \<lcid> | \<language name> | \<language alias> }_

 Especifica el idioma predeterminado que debe asignarse al usuario. Si esta opción se establece en NONE, el idioma predeterminado se establece en el de la base de datos. Si el idioma predeterminado de la base de datos se cambia más tarde, el idioma predeterminado del usuario no se modificará. *DEFAULT_LANGUAGE* puede ser el identificador local (lcid), el nombre del idioma o el alias del idioma.

> [!NOTE]
> Esta opción solo se puede especificar en una base de datos independiente y solo para los usuarios independientes.

 ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | **OFF** ]

 Suprime las comprobaciones de metadatos criptográficos en el servidor en operaciones de copia masiva. De esta manera, el usuario puede copiar los datos de forma masiva entre tablas o bases de datos, sin descifrar los datos. El valor predeterminado es OFF.

> [!WARNING]
> Si esta opción no se utiliza adecuadamente, pueden dañarse los datos. Para obtener más información, vea [Migración de datos confidenciales protegidos mediante Always Encrypted](../../relational-databases/security/encryption/migrate-sensitive-data-protected-by-always-encrypted.md).

## <a name="remarks"></a>Observaciones

 El esquema predeterminado será el primer esquema donde buscará el servidor cuando resuelva los nombres de objetos de este usuario de base de datos. A menos que se especifique lo contrario, el esquema predeterminado será el propietario de los objetos creados por este usuario de base de datos.

 Si el usuario tiene un esquema predeterminado, se utilizará dicho esquema. Si el usuario no tiene un esquema predeterminado, pero es miembro de un grupo con un esquema predeterminado, se utilizará el esquema predeterminado del grupo. Si el usuario no tiene un esquema predeterminado y es miembro de varios grupos, el esquema predeterminado para el usuario será el del grupo de Windows con el principal_id mínimo y un esquema predeterminado establecido explícitamente. Si no se puede determinar ningún esquema predeterminado para un usuario, se utilizará el esquema **dbo**.

 DEFAULT_SCHEMA puede establecerse en un esquema que no existe actualmente en la base de datos. Por tanto, puede asignar un DEFAULT_SCHEMA a un usuario antes de crear el esquema.

 No se puede especificar DEFAULT_SCHEMA para un usuario asignado a un certificado o una clave asimétrica.

> [!IMPORTANT]
> El valor de DEFAULT_SCHEMA se omite si el usuario es un miembro del rol fijo de servidor **sysadmin**. Todos los miembros del rol fijo de servidor **sysadmin** tienen un esquema predeterminado `dbo`.

 Solo puede cambiar el nombre de un usuario que está asignado a un grupo o inicio de sesión de Windows cuando el SID del nuevo nombre de usuario coincide con el SID registrado en la base de datos. Esta comprobación ayuda a evitar la suplantación de inicios de sesión de Windows en la base de datos.

 La cláusula WITH LOGIN habilita la reasignación de un usuario a un inicio de sesión diferente. Los usuarios sin inicio de sesión, los usuarios asignados a un certificado y los usuarios asignados a una clave asimétrica no se pueden reasignar con esta cláusula. Solo se pueden reasignar usuarios de SQL y usuarios (o grupos) de Windows. La cláusula WITH LOGIN no se puede utilizar para cambiar el tipo de usuario, como cambiar una cuenta de Windows a un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La única excepción es cuando se cambia un usuario de Windows a un usuario de Azure AD.

> [!NOTE]
> Las siguientes reglas no se aplican a los usuarios de Windows en Azure SQL Managed Instance, ya que no se admite la creación de inicios de sesión de Windows en dicho servicio. La opción WITH LOGIN solo se puede usar si existen inicios de sesión de Azure AD.

 El nombre del usuario se cambiará automáticamente por el nombre de inicio de sesión si se cumplen las condiciones siguientes.

- El usuario es un usuario de Windows.

- El nombre es un nombre de Windows (contiene una barra diagonal inversa).

- No se ha especificado ningún nombre nuevo.

- El nombre actual difiere del nombre de inicio de sesión.

 En caso contrario, no se cambiará el nombre del usuario a menos que el autor de las llamadas invoque también la cláusula NAME.

El nombre de un usuario asignado a un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un certificado o una clave asimétrica no puede contener el carácter de la barra diagonal inversa (\\).

> [!CAUTION]
> [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]

### <a name="remarks-for-windows-users-in-sql-on-premises-migrated-to-azure-sql-managed-instance"></a>Comentarios para usuarios de Windows en SQL en el entorno local migrados a Azure SQL Managed Instance

Estas notas se aplican a la autenticación como usuarios de Windows que se han federado y sincronizado con Azure AD.

> [!NOTE]
> La funcionalidad de administrador de Azure AD para Azure SQL Managed Instance después de la creación ha cambiado. Para obtener más información, consulte [Nueva funcionalidad de administrador de Azure AD para MI](/azure/sql-database/sql-database-aad-authentication-configure#new-azure-ad-admin-functionality-for-mi).

- La validación de los usuarios o grupos de Windows que se asignan a Azure AD se realiza de forma predeterminada a través de Graph API en todas las versiones de la sintaxis de ALTER USER usada para la migración.
- Los usuarios locales con alias (use un nombre diferente de la cuenta de Windows original) conservarán el nombre con alias.
- Para la autenticación de Azure AD, el parámetro LOGIN solo se aplica a Azure SQL Managed Instance y no se puede usar con SQL Database.
- Para ver los inicios de sesión de entidades de seguridad de Azure AD, use el siguiente comando: `select * from sys.server_principals`.
- Compruebe que el tipo indicado del inicio de sesión sea `E` o `X`.
- La opción PASSWORD no se puede usar en usuarios de Azure AD.
- En todos los casos de migración, los roles y permisos de usuarios o grupos de Windows se transferirán automáticamente a los nuevos usuarios o grupos de Azure AD.
- Una nueva extensión de sintaxis, **FROM EXTERNAL PROVIDER**, está disponible para modificar los usuarios y grupos de Windows de SQL en el entorno local a usuarios y grupos de Azure AD. El dominio de Windows debe estar federado con Azure AD y todos los miembros del dominio de Windows deben estar en Azure AD cuando se use esa extensión. La sintaxis de **FROM EXTERNAL PROVIDER** se aplica a Azure SQL Managed Instance y debe usarse en caso de que los usuarios de Windows no dispongan de inicios de sesión en la instancia de SQL original y deban asignarse a usuarios de base de datos de Azure AD independientes.
- En este caso, el valor userName permitido puede ser:
- Un usuario de Windows (_domain\user_).
- Un grupo de Windows (_MyWidnowsGroup_).
- Un alias de Windows (_MyWindowsAlias_).
- El resultado del comando ALTER reemplaza el valor userName anterior por el nombre correspondiente que se encuentra en Azure AD en función del SID original del valor userName anterior. El nombre modificado se sustituye y almacena en los metadatos de la base de datos:
- (_domain\user_) se reemplazará por Azure AD user@domain.com.
- (_domain\\MyWidnowsGroup_) se reemplazará por el grupo de Azure AD.
- (_MyWindowsAlias_) permanecerá sin cambios, pero se comprobará el SID de este usuario en Azure AD.

> [!NOTE]
> Si no se encuentra el SID del usuario original convertido en objectID en Azure AD, se producirá un error en el comando ALTER USER.

- Para ver los usuarios modificados, use el siguiente comando: `select * from sys.database_principals`.
- Compruebe el tipo indicado por el usuario `E` o `X`.
- Cuando se usa NAME para migrar usuarios de Windows a usuarios de Azure AD, se aplican las restricciones siguientes:
- Debe especificarse un LOGIN válido.
- Se comprobará NAME en Azure AD y solo podrá ser:
- El nombre de LOGIN.
- Un alias: el nombre no puede existir en Azure AD.
- En todos los demás casos, se producirá un error en la sintaxis.

## <a name="security"></a>Seguridad

> [!NOTE]
> Un usuario que tenga permiso **ALTER ANY USER** puede cambiar el esquema predeterminado de cualquier usuario. Un usuario que tenga un esquema modificado podría seleccionar datos de la tabla incorrecta o ejecutar código del esquema incorrecto sin saberlo.

### <a name="permissions"></a>Permisos

 Para cambiar el nombre de un usuario se necesita el permiso **ALTER ANY USER** en la base de datos.

 Para cambiar la información de inicio de sesión de destino de un usuario, es necesario contar con el permiso **CONTROL** en la base de datos.

 Para cambiar el nombre de usuario de un usuario con el permiso **CONTROL** en la base de datos, se necesita el permiso **CONTROL** en la base de datos.

 Para cambiar el idioma o el esquema predeterminado, se necesita el permiso **ALTER** en el usuario. Los usuarios pueden cambiar el idioma y el esquema predeterminados.

## <a name="examples"></a>Ejemplos

Todos los ejemplos se ejecutan en una base de datos de usuario.

### <a name="a-changing-the-name-of-a-database-user"></a>A. Cambiar el nombre de usuario de una base de datos

 En el ejemplo siguiente se cambia el nombre del usuario de la base de datos `Mary5` a `Mary51`.

```sql
ALTER USER Mary5 WITH NAME = Mary51;
GO
```

### <a name="b-changing-the-default-schema-of-a-user"></a>B. Cambiar el esquema predeterminado de un usuario

 En el ejemplo siguiente se cambia el esquema predeterminado del usuario de `Mary51` a `Purchasing`.

```sql
ALTER USER Mary51 WITH DEFAULT_SCHEMA = Purchasing;
GO
```

### <a name="c-changing-several-options-at-once"></a>C. Cambiar varias opciones a la vez

 En el siguiente ejemplo se cambian varias opciones para un usuario de base de datos independiente en una instrucción.

```sql
ALTER USER Philip
WITH NAME = Philipe
, DEFAULT_SCHEMA = Development
, PASSWORD = 'W1r77TT98%ab@#' OLD_PASSWORD = 'New Devel0per'
, DEFAULT_LANGUAGE= French ;
GO
```

### <a name="d-map-the-user-in-the-database-to-an-azure-ad-login-after-migration"></a>D. Asignar el usuario en la base de datos a un inicio de sesión de Azure AD después de la migración

En el ejemplo siguiente se reasigna el usuario, `westus/joe` a un usuario de Azure AD, `joe@westus.com`. Este ejemplo es para los inicios de sesión ya existentes en la instancia administrada. Esto debe realizarse después de haber completado la migración de una base de datos a Azure SQL Managed Instance, y si quiere usar el inicio de sesión de Azure AD para autenticarse.

```sql
ALTER USER [westus/joe] WITH LOGIN = joe@westus.com
```

### <a name="e-map-an-old-windows-user-in-the-database-without-a-login-in-azure-sql-managed-instance-to-an-azure-ad-user"></a>E. Asignar un usuario antiguo de Windows en la base de datos sin un inicio de sesión en Azure SQL Managed Instance a un usuario de Azure AD

En el ejemplo siguiente se reasigna el usuario, `westus/joe` sin un inicio de sesión, a un usuario de Azure AD, `joe@westus.com`. El usuario federado debe existir en Azure AD.

```sql
ALTER USER [westus/joe] FROM EXTERNAL PROVIDER
```

### <a name="f-map-the-user-alias-to-an-existing-azure-ad-login"></a>F. Asignar el alias de usuario a un inicio de sesión de Azure AD existente

En el ejemplo siguiente se reasigna el nombre de usuario `westus\joe` a `joe_alias`. En este caso, el inicio de sesión de Azure AD correspondiente es `joe@westus.com`.

```sql
ALTER USER [westus/joe] WITH LOGIN = joe@westus.com, name= joe_alias
```

### <a name="g-map-a-windows-group-that-was-migrated-in-azure-sql-managed-instance-to-an-azure-ad-group"></a>G. Asignar un grupo de Windows que se migró en Azure SQL Managed Instance a un grupo de Azure AD

En el ejemplo siguiente se reasigna el grupo local antiguo `westus\mygroup` a un grupo de Azure AD `mygroup` en la instancia administrada. El grupo debe existir en Azure AD.

```sql
ALTER USER [westus\mygroup] WITH LOGIN = mygroup
```

## <a name="see-also"></a>Consulte también

- [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)
- [DROP USER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-user-transact-sql.md)
- [Bases de datos independientes](../../relational-databases/databases/contained-databases.md)
- [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)
- [sp_migrate_user_to_contained &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md)
- [Tutorial: Migración de usuarios y grupos de Windows de SQL Server en el entorno local a Azure SQL Managed Instance mediante la sintaxis DDL de T-SQL](/azure/sql-database/tutorial-managed-instance-azure-active-directory-migration)

::: moniker-end
::: moniker range="=azure-sqldw-latest"

:::row:::
    :::column:::
        [SQL Server](alter-user-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL Database](alter-user-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Instancia administrada de SQL](alter-user-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Azure Synapse<br />Analytics \*_**
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](alter-user-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-synapse-analytics"></a>Azure Synapse Analytics

## <a name="syntax"></a>Sintaxis

```syntaxsql
-- Syntax for Azure Synapse

ALTER USER userName
 WITH <set_item> [ ,...n ]

<set_item> ::=
 NAME = newUserName
 | LOGIN = loginName
 | DEFAULT_SCHEMA = schema_name
[;]
```

## <a name="arguments"></a>Argumentos

 *userName* especifica el nombre por el que se identifica al usuario en esta base de datos.

 LOGIN **=** _loginName_ reasigna un usuario a otro inicio de sesión cambiando el identificador de seguridad (SID) del usuario para que coincida con el SID de inicio de sesión.

 Si la instrucción ALTER USER es la única instrucción en un lote SQL, Azure SQL Database admite la cláusula WITH LOGIN. Si la instrucción ALTER USER no es la única instrucción en un lote SQL ni se ejecuta en SQL dinámico, la cláusula WITH LOGIN no se admite.

 NAME **=** _newUserName_ especifica el nuevo nombre de este usuario. *newUserName* no debe existir en la base de datos actual.

 DEFAULT_SCHEMA **=** { *schemaName* | NULL } especifica el primer esquema donde buscará el servidor cuando resuelva los nombres de objetos de este usuario. El establecimiento del esquema predeterminado en NULL quita un esquema predeterminado de un grupo de Windows. La opción NULL no se puede utilizar con un usuario de Windows.

## <a name="remarks"></a>Observaciones

 El esquema predeterminado será el primer esquema donde buscará el servidor cuando resuelva los nombres de objetos de este usuario de base de datos. A menos que se especifique lo contrario, el esquema predeterminado será el propietario de los objetos creados por este usuario de base de datos.

 Si el usuario tiene un esquema predeterminado, se utilizará dicho esquema. Si el usuario no tiene un esquema predeterminado, pero es miembro de un grupo con un esquema predeterminado, se utilizará el esquema predeterminado del grupo. Si el usuario no tiene un esquema predeterminado y es miembro de varios grupos, el esquema predeterminado para el usuario será el del grupo de Windows con el principal_id mínimo y un esquema predeterminado establecido explícitamente. Si no se puede determinar ningún esquema predeterminado para un usuario, se utilizará el esquema **dbo**.

 DEFAULT_SCHEMA puede establecerse en un esquema que no existe actualmente en la base de datos. Por tanto, puede asignar un DEFAULT_SCHEMA a un usuario antes de crear el esquema.

 No se puede especificar DEFAULT_SCHEMA para un usuario asignado a un certificado o una clave asimétrica.

> [!IMPORTANT]
> El valor de DEFAULT_SCHEMA se omite si el usuario es un miembro del rol fijo de servidor **sysadmin**. Todos los miembros del rol fijo de servidor **sysadmin** tienen un esquema predeterminado `dbo`.

 La cláusula WITH LOGIN habilita la reasignación de un usuario a un inicio de sesión diferente. Los usuarios sin inicio de sesión, los usuarios asignados a un certificado y los usuarios asignados a una clave asimétrica no se pueden reasignar con esta cláusula. Solo se pueden reasignar usuarios de SQL y usuarios (o grupos) de Windows. La cláusula WITH LOGIN no se puede utilizar para cambiar el tipo de usuario, como cambiar una cuenta de Windows a un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

 El nombre del usuario se cambiará automáticamente por el nombre de inicio de sesión si se cumplen las condiciones siguientes.

- No se ha especificado ningún nombre nuevo.

- El nombre actual difiere del nombre de inicio de sesión.

 En caso contrario, no se cambiará el nombre del usuario a menos que el autor de las llamadas invoque también la cláusula NAME.

El nombre de un usuario asignado a un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un certificado o una clave asimétrica no puede contener el carácter de la barra diagonal inversa (\\).

> [!CAUTION]
> [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]

## <a name="security"></a>Seguridad

> [!NOTE]
> Un usuario que tenga permiso **ALTER ANY USER** puede cambiar el esquema predeterminado de cualquier usuario. Un usuario que tenga un esquema modificado podría seleccionar datos de la tabla incorrecta o ejecutar código del esquema incorrecto sin saberlo.

### <a name="permissions"></a>Permisos

 Para cambiar el nombre de un usuario se necesita el permiso **ALTER ANY USER** en la base de datos.

 Para cambiar la información de inicio de sesión de destino de un usuario, es necesario contar con el permiso **CONTROL** en la base de datos.

 Para cambiar el nombre de usuario de un usuario con el permiso **CONTROL** en la base de datos, se necesita el permiso **CONTROL** en la base de datos.

 Para cambiar el idioma o el esquema predeterminado, se necesita el permiso **ALTER** en el usuario. Los usuarios pueden cambiar el idioma y el esquema predeterminados.

## <a name="examples"></a>Ejemplos

Todos los ejemplos se ejecutan en una base de datos de usuario.

### <a name="a-changing-the-name-of-a-database-user"></a>A. Cambiar el nombre de usuario de una base de datos

 En el ejemplo siguiente se cambia el nombre del usuario de la base de datos `Mary5` a `Mary51`.

```sql
ALTER USER Mary5 WITH NAME = Mary51;
GO
```

### <a name="b-changing-the-default-schema-of-a-user"></a>B. Cambiar el esquema predeterminado de un usuario

En el ejemplo siguiente se cambia el esquema predeterminado del usuario de `Mary51` a `Purchasing`.

```sql
ALTER USER Mary51 WITH DEFAULT_SCHEMA = Purchasing;
GO
```

## <a name="see-also"></a>Consulte también

- [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)
- [DROP USER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-user-transact-sql.md)
- [Bases de datos independientes](../../relational-databases/databases/contained-databases.md)
- [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)
- [sp_migrate_user_to_contained &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md)

::: moniker-end
::: moniker range=">=aps-pdw-2016"

:::row:::
    :::column:::
        [SQL Server](alter-user-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL Database](alter-user-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Instancia administrada de SQL](alter-user-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-user-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Analytics<br />Platform System (PDW) \*_**
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="analytics-platform-system"></a>Sistema de la plataforma de análisis

## <a name="syntax"></a>Sintaxis

```syntaxsql
-- Syntax for Analytics Platform System

ALTER USER userName
 WITH <set_item> [ ,...n ]

<set_item> ::=
 NAME = newUserName
 | LOGIN = loginName
 | DEFAULT_SCHEMA = schema_name
[;]
```

## <a name="arguments"></a>Argumentos

 *userName* especifica el nombre por el que se identifica al usuario en esta base de datos.

 LOGIN **=** _loginName_ reasigna un usuario a otro inicio de sesión cambiando el identificador de seguridad (SID) del usuario para que coincida con el SID de inicio de sesión.

 Si la instrucción ALTER USER es la única instrucción en un lote SQL, Azure SQL Database admite la cláusula WITH LOGIN. Si la instrucción ALTER USER no es la única instrucción en un lote SQL ni se ejecuta en SQL dinámico, la cláusula WITH LOGIN no se admite.

 NAME **=** _newUserName_ especifica el nuevo nombre de este usuario. *newUserName* no debe existir en la base de datos actual.

 DEFAULT_SCHEMA **=** { *schemaName* | NULL } especifica el primer esquema donde buscará el servidor cuando resuelva los nombres de objetos de este usuario. El establecimiento del esquema predeterminado en NULL quita un esquema predeterminado de un grupo de Windows. La opción NULL no se puede utilizar con un usuario de Windows.

## <a name="remarks"></a>Observaciones

 El esquema predeterminado será el primer esquema donde buscará el servidor cuando resuelva los nombres de objetos de este usuario de base de datos. A menos que se especifique lo contrario, el esquema predeterminado será el propietario de los objetos creados por este usuario de base de datos.

 Si el usuario tiene un esquema predeterminado, se utilizará dicho esquema. Si el usuario no tiene un esquema predeterminado, pero es miembro de un grupo con un esquema predeterminado, se utilizará el esquema predeterminado del grupo. Si el usuario no tiene un esquema predeterminado y es miembro de varios grupos, el esquema predeterminado para el usuario será el del grupo de Windows con el principal_id mínimo y un esquema predeterminado establecido explícitamente. Si no se puede determinar ningún esquema predeterminado para un usuario, se utilizará el esquema **dbo**.

 DEFAULT_SCHEMA puede establecerse en un esquema que no existe actualmente en la base de datos. Por tanto, puede asignar un DEFAULT_SCHEMA a un usuario antes de crear el esquema.

 No se puede especificar DEFAULT_SCHEMA para un usuario asignado a un certificado o una clave asimétrica.

> [!IMPORTANT]
> El valor de DEFAULT_SCHEMA se omite si el usuario es un miembro del rol fijo de servidor **sysadmin**. Todos los miembros del rol fijo de servidor **sysadmin** tienen un esquema predeterminado `dbo`.

 La cláusula WITH LOGIN habilita la reasignación de un usuario a un inicio de sesión diferente. Los usuarios sin inicio de sesión, los usuarios asignados a un certificado y los usuarios asignados a una clave asimétrica no se pueden reasignar con esta cláusula. Solo se pueden reasignar usuarios de SQL y usuarios (o grupos) de Windows. La cláusula WITH LOGIN no se puede utilizar para cambiar el tipo de usuario, como cambiar una cuenta de Windows a un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

 El nombre del usuario se cambiará automáticamente por el nombre de inicio de sesión si se cumplen las condiciones siguientes.

- No se ha especificado ningún nombre nuevo.

- El nombre actual difiere del nombre de inicio de sesión.

 En caso contrario, no se cambiará el nombre del usuario a menos que el autor de las llamadas invoque también la cláusula NAME.

El nombre de un usuario asignado a un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un certificado o una clave asimétrica no puede contener el carácter de la barra diagonal inversa (\\).

> [!CAUTION]
> [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]

## <a name="security"></a>Seguridad

> [!NOTE]
> Un usuario que tenga permiso **ALTER ANY USER** puede cambiar el esquema predeterminado de cualquier usuario. Un usuario que tenga un esquema modificado podría seleccionar datos de la tabla incorrecta o ejecutar código del esquema incorrecto sin saberlo.

### <a name="permissions"></a>Permisos

 Para cambiar el nombre de un usuario se necesita el permiso **ALTER ANY USER** en la base de datos.

 Para cambiar la información de inicio de sesión de destino de un usuario, es necesario contar con el permiso **CONTROL** en la base de datos.

 Para cambiar el nombre de usuario de un usuario con el permiso **CONTROL** en la base de datos, se necesita el permiso **CONTROL** en la base de datos.

 Para cambiar el idioma o el esquema predeterminado, se necesita el permiso **ALTER** en el usuario. Los usuarios pueden cambiar el idioma y el esquema predeterminados.

## <a name="examples"></a>Ejemplos

Todos los ejemplos se ejecutan en una base de datos de usuario.

### <a name="a-changing-the-name-of-a-database-user"></a>A. Cambiar el nombre de usuario de una base de datos

 En el ejemplo siguiente se cambia el nombre del usuario de la base de datos `Mary5` a `Mary51`.

```sql
ALTER USER Mary5 WITH NAME = Mary51;
GO
```

### <a name="b-changing-the-default-schema-of-a-user"></a>B. Cambiar el esquema predeterminado de un usuario
 En el ejemplo siguiente se cambia el esquema predeterminado del usuario de `Mary51` a `Purchasing`.

```sql
ALTER USER Mary51 WITH DEFAULT_SCHEMA = Purchasing;
GO
```

## <a name="see-also"></a>Consulte también

- [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)
- [DROP USER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-user-transact-sql.md)
- [Bases de datos independientes](../../relational-databases/databases/contained-databases.md)
- [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)
- [sp_migrate_user_to_contained &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md)

::: moniker-end
