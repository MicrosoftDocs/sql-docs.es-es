---
description: ALTER LOGIN (Transact-SQL)
title: ALTER LOGIN (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/10/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_LOGIN_TSQL
- ALTER LOGIN
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER LOGIN statement
- change password
- mapping logins [SQL Server]
- logins [SQL Server], modifying
- passwords [SQL Server], modifying
- names [SQL Server], logins
- modifying login accounts
ms.assetid: e247b84e-c99e-4af8-8b50-57586e1cb1c5
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6e15a0c6e1f0e52f530f6c0178f8c6c55cd3d650
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204123"
---
# <a name="alter-login-transact-sql"></a>ALTER LOGIN (Transact-SQL)

Cambia las propiedades de una cuenta de inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [SQL Database](alter-login-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Instancia administrada de SQL](alter-login-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-login-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](alter-login-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server"></a>SQL Server

## <a name="syntax"></a>Sintaxis

```syntaxsql
-- Syntax for SQL Server

ALTER LOGIN login_name
    {
    <status_option>
    | WITH <set_option> [ ,... ]
    | <cryptographic_credential_option>
    }
[;]

<status_option> ::=
        ENABLE | DISABLE

<set_option> ::=
    PASSWORD = 'password' | hashed_password HASHED
    [
      OLD_PASSWORD = 'oldpassword'
      | <password_option> [<password_option> ]
    ]
    | DEFAULT_DATABASE = database
    | DEFAULT_LANGUAGE = language
    | NAME = login_name
    | CHECK_POLICY = { ON | OFF }
    | CHECK_EXPIRATION = { ON | OFF }
    | CREDENTIAL = credential_name
    | NO CREDENTIAL

<password_option> ::=
    MUST_CHANGE | UNLOCK
  
<cryptographic_credentials_option> ::=
    ADD CREDENTIAL credential_name
  | DROP CREDENTIAL credential_name
```

## <a name="arguments"></a>Argumentos

*login_name* Especifica el nombre del inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que se está cambiando. Los inicios de sesión del dominio se deben incluir entre corchetes con el formato [dominio\usuario].

ENABLE | DISABLE Habilita o deshabilita este inicio de sesión. Deshabilitar un inicio de sesión no afecta al comportamiento de los inicios de sesión que ya están conectados. (Use la instrucción `KILL` para finalizar las conexiones existentes). Los inicios de sesión deshabilitados conservan sus permisos y pueden seguir siendo suplantados.

PASSWORD **='** _password_ **'** Se aplica solo a los inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Especifica la contraseña del inicio de sesión que se está cambiando. En las contraseñas se distingue entre mayúsculas y minúsculas.

PASSWORD **=** _hashed\_password_ Se aplica solo a la palabra clave HASHED. Especifica el valor con hash de la contraseña para el inicio de sesión que se está creando.

> [!IMPORTANT]
> Cuando un inicio de sesión (o un usuario de base de datos independiente) se conecta y se autentica, la conexión almacena en memoria caché información de identidad del inicio de sesión. Para un inicio de sesión con autenticación de Windows, esto incluye la información sobre la pertenencia a grupos de Windows. La identidad del inicio de sesión permanecerá autenticada mientras dure la conexión. Para aplicar cambios en la identidad, como un cambio o restablecimiento de contraseña de la pertenencia al grupo de Windows, el inicio de sesión debe cerrar sesión en la entidad de autenticación (Windows o [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]) y volver a iniciar sesión. Los miembros del rol fijo de servidor **sysadmin** o cualquier inicio de sesión con el permiso **ALTER ANY CONNECTION** puede usar el comando **KILL** para finalizar una conexión y hacer que el inicio de sesión se vuelva a conectar. [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] puede reutilizar la información de conexión al abrir varias conexiones en las ventanas del Explorador de objetos y del Editor de consultas. Cierre todas las conexiones para forzar una reconexión.

HASHED Solo se aplica a inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Especifica que la contraseña especificada después del argumento PASSWORD ya tiene aplicado el algoritmo hash. Si no se selecciona esta opción, se aplica un algoritmo hash a la contraseña antes de almacenarla en la base de datos. Esta opción solo se debería utilizar para la sincronización del inicio de sesión entre dos servidores. No utilice la opción HASHED para cambiar las contraseñas de forma habitual.

OLD_PASSWORD **='** _oldpassword_ **'** Se aplica solo a los inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La contraseña actual del inicio de sesión al que se va a asignar una contraseña nueva. En las contraseñas se distingue entre mayúsculas y minúsculas.

MUST_CHANGE Solo se aplica a inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si se incluye esta opción, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pedirá la contraseña actualizada la primera vez que se utilice el inicio de sesión modificado.

DEFAULT_DATABASE **=** _database_ Especifica una base de datos predeterminada que debe asignarse al inicio de sesión.

DEFAULT_LANGUAGE **=** _language_ Especifica el idioma predeterminado que debe asignarse al inicio de sesión. El idioma predeterminado para todos los inicios de sesión de SQL Database es el inglés y no se puede cambiar. El idioma predeterminado del inicio de sesión de `sa` en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en Linux es el inglés, pero se puede cambiar.

NAME = *login_name* El nombre nuevo del inicio de sesión al que se está cambiando el nombre. Si se trata de un inicio de sesión de Windows, el SID de la entidad de seguridad de Windows correspondiente al nombre nuevo debe coincidir con el SID asociado al inicio de sesión en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. El nombre nuevo de un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no puede contener un carácter de barra diagonal inversa (\\).

CHECK_EXPIRATION = { ON | **OFF** } Se aplica solo a los inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Especifica si debe aplicarse la directiva de caducidad de contraseñas en este inicio de sesión. El valor predeterminado es OFF.

CHECK_POLICY **=** { **ON** | OFF } Se aplica solo a los inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Especifica que se deben aplicar las directivas de contraseñas de Windows en el equipo que ejecuta [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para este inicio de sesión. El valor predeterminado es ON.

CREDENTIAL = *credential_name* El nombre de una credencial que debe asignarse a un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La credencial debe existir en la base de datos. Para más información, vea [Credenciales](../../relational-databases/security/authentication-access/credentials-database-engine.md). Las credenciales no se pueden asignar al inicio de sesión sa.

NO CREDENTIAL Elimina cualquier asignación existente del inicio de sesión a una credencial de servidor. Para más información, vea [Credenciales](../../relational-databases/security/authentication-access/credentials-database-engine.md).

UNLOCK Solo se aplica a inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Especifica que un inicio de sesión bloqueado debe desbloquearse.

ADD CREDENTIAL Agrega una credencial del proveedor de Administración extensible de claves (EKM) al inicio de sesión. Para más información, vea [Administración extensible de claves (EKM)](../../relational-databases/security/encryption/extensible-key-management-ekm.md).

DROP CREDENTIAL Quita una credencial del proveedor de Administración extensible de claves (EKM) del inicio de sesión. Para más información, vea [Administración extensible de claves (EKM)] (../.. /relational-databases/security/encryption/extensible-key-management-ekm.md).

## <a name="remarks"></a>Observaciones

Cuando CHECK_POLICY se establece en ON, no puede utilizarse el argumento HASHED.

Cuando el valor de CHECK_POLICY se cambia a ON, ocurre lo siguiente:

- El historial de contraseñas se inicializa con el valor del hash de contraseña actual.

  Cuando el valor de CHECK_POLICY se cambia a OFF, ocurre lo siguiente:

- La opción CHECK_EXPIRATION también se cambia a OFF.
- Se borra el historial de contraseñas.
- El valor de *lockout_time* se restablece.

Si se especifica MUST_CHANGE, CHECK_EXPIRATION y CHECK_POLICY, deben establecerse en ON. Si no es así, la instrucción producirá un error.

Si CHECK_POLICY se establece en OFF, CHECK_EXPIRATION no puede establecerse en ON. Una instrucción ALTER LOGIN con esta combinación de opciones dará error.

No puede usar ALTER LOGIN con el argumento DISABLE para denegar el acceso a un grupo de Windows. Por ejemplo, ALTER LOGIN [*domain\group*] DISABLE devolverá el mensaje de error siguiente:

`"Msg 15151, Level 16, State 1, Line 1
"Cannot alter the login '*Domain\Group*', because it does not exist or you do not have permission."`

es así por diseño.
  
En [!INCLUDE[ssSDS](../../includes/sssds-md.md)], los datos de inicio de sesión necesarios para autenticar una conexión y reglas de firewall de nivel de servidor se almacenan temporalmente en caché en cada base de datos. Esta caché se actualiza regularmente. Para forzar una actualización de la caché de autenticación y garantizar que una base de datos tenga la versión más reciente de la tabla de inicios de sesión, ejecute [DBCC FLUSHAUTHCACHE](../../t-sql/database-console-commands/dbcc-flushauthcache-transact-sql.md).

## <a name="permissions"></a>Permisos

Requiere el permiso ALTER ANY LOGIN.

Si se utiliza la opción CREDENTIAL, también será necesario el permiso ALTER ANY CREDENTIAL.

Si el inicio de sesión que se está cambiado es un miembro del rol fijo de servidor **sysadmin** o tiene concedido el permiso CONTROL SERVER, también será necesario el permiso CONTROL SERVER para realizar los cambios siguientes:

- Restablecer la contraseña sin proporcionar la antigua.
- Habilitar MUST_CHANGE, CHECK_POLICY o CHECK_EXPIRATION.
- Cambiar el nombre de inicio de sesión.
- Habilitar o deshabilitar el inicio de sesión.
- Asignar el inicio de sesión a una credencial diferente.

Una entidad de seguridad puede cambiar la contraseña, el idioma predeterminado y la base de datos predeterminada para su propio inicio de sesión.

## <a name="examples"></a>Ejemplos

### <a name="a-enabling-a-disabled-login"></a>A. Habilitar un inicio de sesión deshabilitado

En el ejemplo siguiente se habilita el inicio de sesión `Mary5`.

```sql
ALTER LOGIN Mary5 ENABLE;
```

### <a name="b-changing-the-password-of-a-login"></a>B. Cambiar la contraseña de un inicio de sesión

En el ejemplo siguiente se cambia la contraseña del inicio de sesión `Mary5` a una contraseña segura.

```sql
ALTER LOGIN Mary5 WITH PASSWORD = '<enterStrongPasswordHere>';
```

### <a name="c-changing-the-password-of-a-login-when-logged-in-as-the-login"></a>C. Cambio de la contraseña de un inicio de sesión al iniciar sesión con el inicio de sesión

Si intenta cambiar la contraseña del inicio de sesión con el que ha iniciado sesión actualmente y no tiene el permiso `ALTER ANY LOGIN`, debe especificar la opción `OLD_PASSWORD`.

```sql
ALTER LOGIN Mary5 WITH PASSWORD = '<enterStrongPasswordHere>' OLD_PASSWORD = '<oldWeakPasswordHere>';
```

### <a name="d-changing-the-name-of-a-login"></a>D. Cambiar el nombre de un inicio de sesión

En el ejemplo siguiente se cambia el nombre del inicio de sesión `Mary5` a `John2`.

```sql
ALTER LOGIN Mary5 WITH NAME = John2;
```

### <a name="e-mapping-a-login-to-a-credential"></a>E. Asignar un inicio de sesión a una credencial

En el ejemplo siguiente se asigna el inicio de sesión `John2` a la credencial `Custodian04`.

```sql
ALTER LOGIN John2 WITH CREDENTIAL = Custodian04;
```

### <a name="f-mapping-a-login-to-an-extensible-key-management-credential"></a>F. Asignar un inicio de sesión a una credencial de Administración de claves extensible

En el ejemplo siguiente se asigna el inicio de sesión `Mary5` a la credencial EKM `EKMProvider1`.

```sql
ALTER LOGIN Mary5
ADD CREDENTIAL EKMProvider1;
GO
```

### <a name="f-unlocking-a-login"></a>F. Desbloquear un inicio de sesión

Para desbloquear un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ejecute la instrucción siguiente y sustituya \*\*\*\* por la contraseña de cuenta deseada.

```sql
ALTER LOGIN [Mary5] WITH PASSWORD = '****' UNLOCK ;

GO
```

Para desbloquear un inicio de sesión sin cambiar la contraseña, desactive la directiva de comprobación y, a continuación, inicie sesión de nuevo.

```sql
ALTER LOGIN [Mary5] WITH CHECK_POLICY = OFF;
ALTER LOGIN [Mary5] WITH CHECK_POLICY = ON;
GO
```

### <a name="g-changing-the-password-of-a-login-using-hashed"></a>G. Cambiar la contraseña de un inicio de sesión utilizando HASHED

En el ejemplo siguiente se cambia la contraseña de inicio de sesión de `TestUser` a un valor que ya tiene aplicado hash.

```sql
ALTER LOGIN TestUser WITH
PASSWORD = 0x01000CF35567C60BFB41EBDE4CF700A985A13D773D6B45B90900 HASHED ;
GO
```

## <a name="see-also"></a>Consulte también

- [Credenciales](../../relational-databases/security/authentication-access/credentials-database-engine.md)
- [CREATE LOGIN](../../t-sql/statements/create-login-transact-sql.md)
- [DROP LOGIN](../../t-sql/statements/drop-login-transact-sql.md)
- [CREATE CREDENTIAL](../../t-sql/statements/create-credential-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [Administración extensible de claves (EKM)](../../relational-databases/security/encryption/extensible-key-management-ekm.md)

::: moniker-end
::: moniker range="=azuresqldb-current"

:::row:::
    :::column:::
        [SQL Server](alter-login-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* SQL Database \*_**
    :::column-end:::
    :::column:::
        [Instancia administrada de SQL](alter-login-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-login-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](alter-login-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-database"></a>SQL Database

## <a name="sql-server"></a>SQL Server

## <a name="syntax"></a>Sintaxis

```syntaxsql
-- Syntax for Azure SQL Database

ALTER LOGIN login_name
  {
      <status_option>
    | WITH <set_option> [ ,.. .n ]
  }
[;]

<status_option> ::=
    ENABLE | DISABLE

<set_option> ::=
    PASSWORD ='password'
    [
      OLD_PASSWORD ='oldpassword'
    ]
    | NAME = login_name
```

## <a name="arguments"></a>Argumentos

*login_name* Especifica el nombre del inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que se está cambiando. Los inicios de sesión del dominio se deben incluir entre corchetes con el formato [dominio\usuario].

ENABLE | DISABLE Habilita o deshabilita este inicio de sesión. Deshabilitar un inicio de sesión no afecta al comportamiento de los inicios de sesión que ya están conectados. (Use la instrucción `KILL` para finalizar las conexiones existentes). Los inicios de sesión deshabilitados conservan sus permisos y pueden seguir siendo suplantados.

PASSWORD **='** _password_ **'** Se aplica solo a los inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Especifica la contraseña del inicio de sesión que se está cambiando. En las contraseñas se distingue entre mayúsculas y minúsculas.

Las conexiones continuamente activas a SQL Database requieren una reautorización (realizada por el Motor de base de datos) como mínimo cada 10 horas. El Motor de base de datos intenta la reautorización con la contraseña enviada originalmente y no se requiere la intervención del usuario. Por motivos de rendimiento, cuando una contraseña se restablece en SQL Database, la conexión no se volverá a autenticar, incluso si se restablece la conexión debido a la agrupación de conexiones. Esto es diferente del comportamiento de SQL Database local. Si la contraseña se ha cambiado desde que se autorizó inicialmente la conexión, es necesario terminar la conexión y establecer una nueva conexión con la nueva contraseña. Un usuario con el permiso KILL DATABASE CONNECTION puede terminar explícitamente una conexión con SQL Database mediante el comando KILL. Para obtener más información, vea [KILL](../../t-sql/language-elements/kill-transact-sql.md).

> [!IMPORTANT]
> Cuando un inicio de sesión (o un usuario de base de datos independiente) se conecta y se autentica, la conexión almacena en memoria caché información de identidad del inicio de sesión. Para un inicio de sesión con autenticación de Windows, esto incluye la información sobre la pertenencia a grupos de Windows. La identidad del inicio de sesión permanecerá autenticada mientras dure la conexión. Para aplicar cambios en la identidad, como un cambio o restablecimiento de contraseña de la pertenencia al grupo de Windows, el inicio de sesión debe cerrar sesión en la entidad de autenticación (Windows o [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]) y volver a iniciar sesión. Los miembros del rol fijo de servidor **sysadmin** o cualquier inicio de sesión con el permiso **ALTER ANY CONNECTION** puede usar el comando **KILL** para finalizar una conexión y hacer que el inicio de sesión se vuelva a conectar. [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] puede reutilizar la información de conexión al abrir varias conexiones en las ventanas del Explorador de objetos y del Editor de consultas. Cierre todas las conexiones para forzar una reconexión.

OLD_PASSWORD **='** _oldpassword_ **'** Se aplica solo a los inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La contraseña actual del inicio de sesión al que se va a asignar una contraseña nueva. En las contraseñas se distingue entre mayúsculas y minúsculas.

NAME = *login_name* El nombre nuevo del inicio de sesión al que se está cambiando el nombre. Si se trata de un inicio de sesión de Windows, el SID de la entidad de seguridad de Windows correspondiente al nombre nuevo debe coincidir con el SID asociado al inicio de sesión en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. El nombre nuevo de un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no puede contener un carácter de barra diagonal inversa (\\).

## <a name="remarks"></a>Observaciones

En [!INCLUDE[ssSDS](../../includes/sssds-md.md)], los datos de inicio de sesión necesarios para autenticar una conexión y reglas de firewall de nivel de servidor se almacenan temporalmente en caché en cada base de datos. Esta caché se actualiza regularmente. Para forzar una actualización de la caché de autenticación y garantizar que una base de datos tenga la versión más reciente de la tabla de inicios de sesión, ejecute [DBCC FLUSHAUTHCACHE](../../t-sql/database-console-commands/dbcc-flushauthcache-transact-sql.md).

## <a name="permissions"></a>Permisos

Requiere el permiso ALTER ANY LOGIN.

Si el inicio de sesión que se está cambiado es un miembro del rol fijo de servidor **sysadmin** o tiene concedido el permiso CONTROL SERVER, también será necesario el permiso CONTROL SERVER para realizar los cambios siguientes:

- Restablecer la contraseña sin proporcionar la antigua.
- Cambiar el nombre de inicio de sesión.
- Habilitar o deshabilitar el inicio de sesión.
- Asignar el inicio de sesión a una credencial diferente.

Una entidad de seguridad puede cambiar la contraseña de su propio inicio de sesión.

## <a name="examples"></a>Ejemplos

En estos ejemplos también se incluyen ejemplos de uso de otros productos SQL. Más arriba puede ver los argumentos que se admiten.

### <a name="a-enabling-a-disabled-login"></a>A. Habilitar un inicio de sesión deshabilitado

En el ejemplo siguiente se habilita el inicio de sesión `Mary5`.

```sql
ALTER LOGIN Mary5 ENABLE;
```

### <a name="b-changing-the-password-of-a-login"></a>B. Cambiar la contraseña de un inicio de sesión

En el ejemplo siguiente se cambia la contraseña del inicio de sesión `Mary5` a una contraseña segura.

```sql
ALTER LOGIN Mary5 WITH PASSWORD = '<enterStrongPasswordHere>';
```

### <a name="c-changing-the-name-of-a-login"></a>C. Cambiar el nombre de un inicio de sesión

En el ejemplo siguiente se cambia el nombre del inicio de sesión `Mary5` a `John2`.

```sql
ALTER LOGIN Mary5 WITH NAME = John2;
```

### <a name="d-mapping-a-login-to-a-credential"></a>D. Asignar un inicio de sesión a una credencial

En el ejemplo siguiente se asigna el inicio de sesión `John2` a la credencial `Custodian04`.

```sql
ALTER LOGIN John2 WITH CREDENTIAL = Custodian04;
```

### <a name="e-mapping-a-login-to-an-extensible-key-management-credential"></a>E. Asignar un inicio de sesión a una credencial de Administración de claves extensible

En el ejemplo siguiente se asigna el inicio de sesión `Mary5` a la credencial EKM `EKMProvider1`.


**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.

```sql
ALTER LOGIN Mary5
ADD CREDENTIAL EKMProvider1;
GO
```

### <a name="f-unlocking-a-login"></a>F. Desbloquear un inicio de sesión

Para desbloquear un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ejecute la instrucción siguiente y sustituya \*\*\*\* por la contraseña de cuenta deseada.

```sql
ALTER LOGIN [Mary5] WITH PASSWORD = '****' UNLOCK ;

GO
```

Para desbloquear un inicio de sesión sin cambiar la contraseña, desactive la directiva de comprobación y, a continuación, inicie sesión de nuevo.

```sql
ALTER LOGIN [Mary5] WITH CHECK_POLICY = OFF;
ALTER LOGIN [Mary5] WITH CHECK_POLICY = ON;
GO
```

### <a name="g-changing-the-password-of-a-login-using-hashed"></a>G. Cambiar la contraseña de un inicio de sesión utilizando HASHED

En el ejemplo siguiente se cambia la contraseña de inicio de sesión de `TestUser` a un valor que ya tiene aplicado hash.

**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.

```sql
ALTER LOGIN TestUser WITH
PASSWORD = 0x01000CF35567C60BFB41EBDE4CF700A985A13D773D6B45B90900 HASHED ;
GO
```

## <a name="see-also"></a>Consulte también

- [Credenciales](../../relational-databases/security/authentication-access/credentials-database-engine.md)
- [CREATE LOGIN](../../t-sql/statements/create-login-transact-sql.md)
- [DROP LOGIN](../../t-sql/statements/drop-login-transact-sql.md)
- [CREATE CREDENTIAL](../../t-sql/statements/create-credential-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [Administración extensible de claves (EKM)](../../relational-databases/security/encryption/extensible-key-management-ekm.md)

::: moniker-end
::: moniker range="=azuresqldb-mi-current"

:::row:::
    :::column:::
        [SQL Server](alter-login-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL Database](alter-login-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* SQL Managed Instance \*_**
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-login-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](alter-login-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-sql-managed-instance"></a>Azure SQL Managed Instance

## <a name="syntax"></a>Sintaxis

```syntaxsql
-- Syntax for SQL Server and Azure SQL Managed Instance

ALTER LOGIN login_name
    {
    <status_option>
    | WITH <set_option> [ ,... ]
    | <cryptographic_credential_option>
    }
[;]

<status_option> ::=
        ENABLE | DISABLE
  
<set_option> ::=
    PASSWORD = 'password' | hashed_password HASHED
    [
      OLD_PASSWORD = 'oldpassword'
      | <password_option> [<password_option> ]
    ]
    | DEFAULT_DATABASE = database
    | DEFAULT_LANGUAGE = language
    | NAME = login_name
    | CHECK_POLICY = { ON | OFF }
    | CHECK_EXPIRATION = { ON | OFF }
    | CREDENTIAL = credential_name
    | NO CREDENTIAL

<password_option> ::=
    MUST_CHANGE | UNLOCK

<cryptographic_credentials_option> ::=
    ADD CREDENTIAL credential_name
  | DROP CREDENTIAL credential_name
```

> [!NOTE]
> La funcionalidad de administrador de Azure AD para instancia administrada después de la creación ha cambiado. Para obtener más información, consulte [Nueva funcionalidad de administrador de Azure AD para MI](/azure/sql-database/sql-database-aad-authentication-configure#new-azure-ad-admin-functionality-for-mi).

```syntaxsql
-- Syntax for Azure SQL Managed Instance using Azure AD logins

ALTER LOGIN login_name
  {
      <status_option>
    | WITH <set_option> [ ,.. .n ]
  }
[;]

<status_option> ::=
    ENABLE | DISABLE

<set_option> ::=
     DEFAULT_DATABASE = database
   | DEFAULT_LANGUAGE = language
```

## <a name="arguments"></a>Argumentos

### <a name="arguments-applicable-to-sql-and-azure-ad-logins"></a>Argumentos que se aplican a los inicios de sesión de SQL y Azure AD

*login_name* Especifica el nombre del inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que se está cambiando. Los inicios de sesión de Azure AD se deben especificar como user@domain. Por ejemplo, john.smith@contoso.com, o bien como el nombre del grupo o la aplicación de Azure AD. Para los inicios de sesión de Azure AD, *nombre_de_inicio_de_sesión* se debe corresponder a un inicio de sesión de Azure AD existente creado en la base de datos principal.

ENABLE | DISABLE Habilita o deshabilita este inicio de sesión. Deshabilitar un inicio de sesión no afecta al comportamiento de los inicios de sesión que ya están conectados. (Use la instrucción `KILL` para finalizar una conexión existente). Los inicios de sesión deshabilitados conservan sus permisos y pueden seguir siendo suplantados.

DEFAULT_DATABASE **=** _database_ Especifica una base de datos predeterminada que debe asignarse al inicio de sesión.

DEFAULT_LANGUAGE **=** _language_ Especifica el idioma predeterminado que debe asignarse al inicio de sesión. El idioma predeterminado para todos los inicios de sesión de SQL Database es el inglés y no se puede cambiar. El idioma predeterminado del inicio de sesión de `sa` en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en Linux es el inglés, pero se puede cambiar.

### <a name="arguments-applicable-only-to-sql-logins"></a>Argumentos que solo se aplican a los inicios de sesión SQL

PASSWORD **='** _password_ **'** Se aplica solo a los inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Especifica la contraseña del inicio de sesión que se está cambiando. En las contraseñas se distingue entre mayúsculas y minúsculas. Las contraseñas tampoco se aplican cuando se usan con inicios de sesión externos, como los de Azure AD.

Las conexiones continuamente activas a SQL Database requieren una reautorización (realizada por el Motor de base de datos) como mínimo cada 10 horas. El Motor de base de datos intenta la reautorización con la contraseña enviada originalmente y no se requiere la intervención del usuario. Por motivos de rendimiento, cuando una contraseña se restablece en SQL Database, la conexión no se volverá a autenticar, incluso si se restablece la conexión debido a la agrupación de conexiones. Esto es diferente del comportamiento de SQL Database local. Si la contraseña se ha cambiado desde que se autorizó inicialmente la conexión, es necesario terminar la conexión y establecer una nueva conexión con la nueva contraseña. Un usuario con el permiso KILL DATABASE CONNECTION puede terminar explícitamente una conexión con SQL Database mediante el comando KILL. Para obtener más información, vea [KILL](../../t-sql/language-elements/kill-transact-sql.md).

PASSWORD **=** _hashed\_password_ Se aplica solo a la palabra clave HASHED. Especifica el valor con hash de la contraseña para el inicio de sesión que se está creando.

HASHED Solo se aplica a inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Especifica que la contraseña especificada después del argumento PASSWORD ya tiene aplicado el algoritmo hash. Si no se selecciona esta opción, se aplica un algoritmo hash a la contraseña antes de almacenarla en la base de datos. Esta opción solo se debería utilizar para la sincronización del inicio de sesión entre dos servidores. No utilice la opción HASHED para cambiar las contraseñas de forma habitual.

OLD_PASSWORD **='** _oldpassword_ **'** Se aplica solo a los inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La contraseña actual del inicio de sesión al que se va a asignar una contraseña nueva. En las contraseñas se distingue entre mayúsculas y minúsculas.

MUST_CHANGE<br>
Solo se aplica a inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si se incluye esta opción, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pedirá la contraseña actualizada la primera vez que se utilice el inicio de sesión modificado.

NAME = *login_name* El nombre nuevo del inicio de sesión al que se está cambiando el nombre. Si se trata de un inicio de sesión de Windows, el SID de la entidad de seguridad de Windows correspondiente al nombre nuevo debe coincidir con el SID asociado al inicio de sesión en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. El nombre nuevo de un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no puede contener un carácter de barra diagonal inversa (\\).

CHECK_EXPIRATION = { ON | **OFF** } Se aplica solo a los inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Especifica si debe aplicarse la directiva de caducidad de contraseñas en este inicio de sesión. El valor predeterminado es OFF.

CHECK_POLICY **=** { **ON** | OFF } Se aplica solo a los inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Especifica que se deben aplicar las directivas de contraseñas de Windows en el equipo que ejecuta [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para este inicio de sesión. El valor predeterminado es ON.

CREDENTIAL = *credential_name* El nombre de una credencial que debe asignarse a un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La credencial debe existir en la base de datos. Para más información, vea [Credenciales](../../relational-databases/security/authentication-access/credentials-database-engine.md). Las credenciales no se pueden asignar al inicio de sesión sa.

NO CREDENTIAL Elimina cualquier asignación existente del inicio de sesión a una credencial de servidor. Para más información, vea [Credenciales](../../relational-databases/security/authentication-access/credentials-database-engine.md).

UNLOCK Solo se aplica a inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Especifica que un inicio de sesión bloqueado debe desbloquearse.

ADD CREDENTIAL Agrega una credencial del proveedor de Administración extensible de claves (EKM) al inicio de sesión. Para más información, vea [Administración extensible de claves (EKM)](../../relational-databases/security/encryption/extensible-key-management-ekm.md).

DROP CREDENTIAL Quita una credencial del proveedor de Administración extensible de claves (EKM) del inicio de sesión. Para más información, vea [Administración extensible de claves (EKM)](../../relational-databases/security/encryption/extensible-key-management-ekm.md).

## <a name="remarks"></a>Observaciones

Cuando CHECK_POLICY se establece en ON, no puede utilizarse el argumento HASHED.

Cuando el valor de CHECK_POLICY se cambia a ON, ocurre lo siguiente:

- El historial de contraseñas se inicializa con el valor del hash de contraseña actual.

  Cuando el valor de CHECK_POLICY se cambia a OFF, ocurre lo siguiente:

- La opción CHECK_EXPIRATION también se cambia a OFF.
- Se borra el historial de contraseñas.
- El valor de *lockout_time* se restablece.

Si se especifica MUST_CHANGE, CHECK_EXPIRATION y CHECK_POLICY, deben establecerse en ON. Si no es así, la instrucción producirá un error.

Si CHECK_POLICY se establece en OFF, CHECK_EXPIRATION no puede establecerse en ON. Una instrucción ALTER LOGIN con esta combinación de opciones dará error.

No puede usar ALTER_LOGIN con el argumento DISABLE para denegar el acceso a un grupo de Windows. es así por diseño. Por ejemplo, ALTER_LOGIN [*domain\group*] DISABLE devolverá el siguiente mensaje de error:

`"Msg 15151, Level 16, State 1, Line 1
"Cannot alter the login '*Domain\Group*', because it does not exist or you do not have permission."`

En [!INCLUDE[ssSDS](../../includes/sssds-md.md)], los datos de inicio de sesión necesarios para autenticar una conexión y reglas de firewall de nivel de servidor se almacenan temporalmente en caché en cada base de datos. Esta caché se actualiza regularmente. Para forzar una actualización de la caché de autenticación y garantizar que una base de datos tenga la versión más reciente de la tabla de inicios de sesión, ejecute [DBCC FLUSHAUTHCACHE](../../t-sql/database-console-commands/dbcc-flushauthcache-transact-sql.md).

## <a name="permissions"></a>Permisos

Requiere el permiso ALTER ANY LOGIN.

Si se utiliza la opción CREDENTIAL, también será necesario el permiso ALTER ANY CREDENTIAL.

Si el inicio de sesión que se está cambiado es un miembro del rol fijo de servidor **sysadmin** o tiene concedido el permiso CONTROL SERVER, también será necesario el permiso CONTROL SERVER para realizar los cambios siguientes:

- Restablecer la contraseña sin proporcionar la antigua.
- Habilitar MUST_CHANGE, CHECK_POLICY o CHECK_EXPIRATION.
- Cambiar el nombre de inicio de sesión.
- Habilitar o deshabilitar el inicio de sesión.
- Asignar el inicio de sesión a una credencial diferente.

Una entidad de seguridad puede cambiar la contraseña, el idioma predeterminado y la base de datos predeterminada para su propio inicio de sesión.

Solo una entidad de seguridad de SQL con privilegios `sysadmin` puede ejecutar un comando ALTER LOGIN en un inicio de sesión de Azure AD.

## <a name="examples"></a>Ejemplos

En estos ejemplos también se incluyen ejemplos de uso de otros productos SQL. Más arriba puede ver los argumentos que se admiten.

### <a name="a-enabling-a-disabled-login"></a>A. Habilitar un inicio de sesión deshabilitado

En el ejemplo siguiente se habilita el inicio de sesión `Mary5`.

```sql
ALTER LOGIN Mary5 ENABLE;
```

### <a name="b-changing-the-password-of-a-login"></a>B. Cambiar la contraseña de un inicio de sesión

En el ejemplo siguiente se cambia la contraseña del inicio de sesión `Mary5` a una contraseña segura.

```sql
ALTER LOGIN Mary5 WITH PASSWORD = '<enterStrongPasswordHere>';
```

### <a name="c-changing-the-name-of-a-login"></a>C. Cambiar el nombre de un inicio de sesión

En el ejemplo siguiente se cambia el nombre del inicio de sesión `Mary5` a `John2`.

```sql
ALTER LOGIN Mary5 WITH NAME = John2;
```

### <a name="d-mapping-a-login-to-a-credential"></a>D. Asignar un inicio de sesión a una credencial

En el ejemplo siguiente se asigna el inicio de sesión `John2` a la credencial `Custodian04`.

```sql
ALTER LOGIN John2 WITH CREDENTIAL = Custodian04;
```

### <a name="e-mapping-a-login-to-an-extensible-key-management-credential"></a>E. Asignar un inicio de sesión a una credencial de Administración de claves extensible

En el ejemplo siguiente se asigna el inicio de sesión `Mary5` a la credencial EKM `EKMProvider1`.

**Se aplica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores, y a Azure SQL Managed Instance.

```sql
ALTER LOGIN Mary5
ADD CREDENTIAL EKMProvider1;
GO
```

### <a name="f-unlocking-a-login"></a>F. Desbloquear un inicio de sesión

Para desbloquear un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ejecute la instrucción siguiente y sustituya \*\*\*\* por la contraseña de cuenta deseada.

```sql
ALTER LOGIN [Mary5] WITH PASSWORD = '****' UNLOCK ;

GO
```

Para desbloquear un inicio de sesión sin cambiar la contraseña, desactive la directiva de comprobación y, a continuación, inicie sesión de nuevo.

```sql
ALTER LOGIN [Mary5] WITH CHECK_POLICY = OFF;
ALTER LOGIN [Mary5] WITH CHECK_POLICY = ON;
GO
```

### <a name="g-changing-the-password-of-a-login-using-hashed"></a>G. Cambiar la contraseña de un inicio de sesión utilizando HASHED

En el ejemplo siguiente se cambia la contraseña de inicio de sesión de `TestUser` a un valor que ya tiene aplicado hash.

**Se aplica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores, y a Azure SQL Managed Instance.

```sql
ALTER LOGIN TestUser WITH
PASSWORD = 0x01000CF35567C60BFB41EBDE4CF700A985A13D773D6B45B90900 HASHED ;
GO
```

### <a name="h-disabling-the-login-of-an-azure-ad-user"></a>H. Deshabilitar el inicio de sesión de un usuario de Azure AD

En el ejemplo siguiente se deshabilita el inicio de sesión de un usuario de Azure AD, joe@contoso.com.

```sql
ALTER LOGIN [joe@contoso.com] DISABLE
```

## <a name="see-also"></a>Consulte también

- [Credenciales](../../relational-databases/security/authentication-access/credentials-database-engine.md)
- [CREATE LOGIN](../../t-sql/statements/create-login-transact-sql.md)
- [DROP LOGIN](../../t-sql/statements/drop-login-transact-sql.md)
- [CREATE CREDENTIAL](../../t-sql/statements/create-credential-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [Administración extensible de claves (EKM)](../../relational-databases/security/encryption/extensible-key-management-ekm.md)

::: moniker-end
::: moniker range="=azure-sqldw-latest"

:::row:::
    :::column:::
        [SQL Server](alter-login-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL Database](alter-login-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Instancia administrada de SQL](alter-login-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Azure Synapse<br />Analytics \*_**
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](alter-login-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-synapse-analytics"></a>Azure Synapse Analytics

## <a name="syntax"></a>Sintaxis

```syntaxsql
-- Syntax for Azure Synapse

ALTER LOGIN login_name
  {
      <status_option>
    | WITH <set_option> [ ,.. .n ]
  }
[;]

<status_option> ::=
    ENABLE | DISABLE
  
<set_option> ::=
    PASSWORD ='password'
    [
      OLD_PASSWORD ='oldpassword'
    ]
    | NAME = login_name
```

## <a name="arguments"></a>Argumentos

*login_name* Especifica el nombre del inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que se está cambiando. Los inicios de sesión del dominio se deben incluir entre corchetes con el formato [dominio\usuario].

ENABLE | DISABLE Habilita o deshabilita este inicio de sesión. Deshabilitar un inicio de sesión no afecta al comportamiento de los inicios de sesión que ya están conectados. (Use la instrucción `KILL` para finalizar las conexiones existentes). Los inicios de sesión deshabilitados conservan sus permisos y pueden seguir siendo suplantados.

PASSWORD **='** _password_ **'** Se aplica solo a los inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Especifica la contraseña del inicio de sesión que se está cambiando. En las contraseñas se distingue entre mayúsculas y minúsculas.

Las conexiones continuamente activas a SQL Database requieren una reautorización (realizada por el Motor de base de datos) como mínimo cada 10 horas. El Motor de base de datos intenta la reautorización con la contraseña enviada originalmente y no se requiere la intervención del usuario. Por motivos de rendimiento, cuando una contraseña se restablece en SQL Database, la conexión no se volverá a autenticar, incluso si se restablece la conexión debido a la agrupación de conexiones. Esto es diferente del comportamiento de SQL Database local. Si la contraseña se ha cambiado desde que se autorizó inicialmente la conexión, es necesario terminar la conexión y establecer una nueva conexión con la nueva contraseña. Un usuario con el permiso KILL DATABASE CONNECTION puede terminar explícitamente una conexión con SQL Database mediante el comando KILL. Para obtener más información, vea [KILL](../../t-sql/language-elements/kill-transact-sql.md).

> [!IMPORTANT]
> Cuando un inicio de sesión (o un usuario de base de datos independiente) se conecta y se autentica, la conexión almacena en memoria caché información de identidad del inicio de sesión. Para un inicio de sesión con autenticación de Windows, esto incluye la información sobre la pertenencia a grupos de Windows. La identidad del inicio de sesión permanecerá autenticada mientras dure la conexión. Para aplicar cambios en la identidad, como un cambio o restablecimiento de contraseña de la pertenencia al grupo de Windows, el inicio de sesión debe cerrar sesión en la entidad de autenticación (Windows o [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]) y volver a iniciar sesión. Los miembros del rol fijo de servidor **sysadmin** o cualquier inicio de sesión con el permiso **ALTER ANY CONNECTION** puede usar el comando **KILL** para finalizar una conexión y hacer que el inicio de sesión se vuelva a conectar. [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] puede reutilizar la información de conexión al abrir varias conexiones en las ventanas del Explorador de objetos y del Editor de consultas. Cierre todas las conexiones para forzar una reconexión.

OLD_PASSWORD **='** _oldpassword_ **'** Se aplica solo a los inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La contraseña actual del inicio de sesión al que se va a asignar una contraseña nueva. En las contraseñas se distingue entre mayúsculas y minúsculas.

NAME = *login_name* El nombre nuevo del inicio de sesión al que se está cambiando el nombre. Si se trata de un inicio de sesión de Windows, el SID de la entidad de seguridad de Windows correspondiente al nombre nuevo debe coincidir con el SID asociado al inicio de sesión en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. El nombre nuevo de un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no puede contener un carácter de barra diagonal inversa (\\).

## <a name="remarks"></a>Observaciones

En [!INCLUDE[ssSDS](../../includes/sssds-md.md)], los datos de inicio de sesión necesarios para autenticar una conexión y reglas de firewall de nivel de servidor se almacenan temporalmente en caché en cada base de datos. Esta caché se actualiza regularmente. Para forzar una actualización de la caché de autenticación y garantizar que una base de datos tenga la versión más reciente de la tabla de inicios de sesión, ejecute [DBCC FLUSHAUTHCACHE](../../t-sql/database-console-commands/dbcc-flushauthcache-transact-sql.md).

## <a name="permissions"></a>Permisos

Requiere el permiso ALTER ANY LOGIN.

Si el inicio de sesión que se está cambiado es un miembro del rol fijo de servidor **sysadmin** o tiene concedido el permiso CONTROL SERVER, también será necesario el permiso CONTROL SERVER para realizar los cambios siguientes:

- Restablecer la contraseña sin proporcionar la antigua.
- Cambiar el nombre de inicio de sesión.
- Habilitar o deshabilitar el inicio de sesión.
- Asignar el inicio de sesión a una credencial diferente.

Una entidad de seguridad puede cambiar la contraseña de su propio inicio de sesión.

## <a name="examples"></a>Ejemplos

En estos ejemplos también se incluyen ejemplos de uso de otros productos SQL. Más arriba puede ver los argumentos que se admiten.

### <a name="a-enabling-a-disabled-login"></a>A. Habilitar un inicio de sesión deshabilitado

En el ejemplo siguiente se habilita el inicio de sesión `Mary5`.

```sql
ALTER LOGIN Mary5 ENABLE;
```

### <a name="b-changing-the-password-of-a-login"></a>B. Cambiar la contraseña de un inicio de sesión

En el ejemplo siguiente se cambia la contraseña del inicio de sesión `Mary5` a una contraseña segura.

```sql
ALTER LOGIN Mary5 WITH PASSWORD = '<enterStrongPasswordHere>';
```

### <a name="c-changing-the-name-of-a-login"></a>C. Cambiar el nombre de un inicio de sesión

En el ejemplo siguiente se cambia el nombre del inicio de sesión `Mary5` a `John2`.

```sql
ALTER LOGIN Mary5 WITH NAME = John2;
```

### <a name="d-mapping-a-login-to-a-credential"></a>D. Asignar un inicio de sesión a una credencial

En el ejemplo siguiente se asigna el inicio de sesión `John2` a la credencial `Custodian04`.

```sql
ALTER LOGIN John2 WITH CREDENTIAL = Custodian04;
```

### <a name="e-mapping-a-login-to-an-extensible-key-management-credential"></a>E. Asignar un inicio de sesión a una credencial de Administración de claves extensible

En el ejemplo siguiente se asigna el inicio de sesión `Mary5` a la credencial EKM `EKMProvider1`.

**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.

```sql
ALTER LOGIN Mary5
ADD CREDENTIAL EKMProvider1;
GO
```

### <a name="f-unlocking-a-login"></a>F. Desbloquear un inicio de sesión

Para desbloquear un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ejecute la instrucción siguiente y sustituya \*\*\*\* por la contraseña de cuenta deseada.

```sql
ALTER LOGIN [Mary5] WITH PASSWORD = '****' UNLOCK ;

GO
```

Para desbloquear un inicio de sesión sin cambiar la contraseña, desactive la directiva de comprobación y, a continuación, inicie sesión de nuevo.

```sql
ALTER LOGIN [Mary5] WITH CHECK_POLICY = OFF;
ALTER LOGIN [Mary5] WITH CHECK_POLICY = ON;
GO
```

### <a name="g-changing-the-password-of-a-login-using-hashed"></a>G. Cambiar la contraseña de un inicio de sesión utilizando HASHED

En el ejemplo siguiente se cambia la contraseña de inicio de sesión de `TestUser` a un valor que ya tiene aplicado hash.

**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.

```sql
ALTER LOGIN TestUser WITH
PASSWORD = 0x01000CF35567C60BFB41EBDE4CF700A985A13D773D6B45B90900 HASHED ;
GO
```

## <a name="see-also"></a>Consulte también

- [Credenciales](../../relational-databases/security/authentication-access/credentials-database-engine.md)
- [CREATE LOGIN](../../t-sql/statements/create-login-transact-sql.md)
- [DROP LOGIN](../../t-sql/statements/drop-login-transact-sql.md)
- [CREATE CREDENTIAL](../../t-sql/statements/create-credential-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [Administración extensible de claves (EKM)](../../relational-databases/security/encryption/extensible-key-management-ekm.md)

::: moniker-end
::: moniker range=">=aps-pdw-2016"

:::row:::
    :::column:::
        [SQL Server](alter-login-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL Database](alter-login-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Instancia administrada de SQL](alter-login-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-login-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
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

ALTER LOGIN login_name
    {
    <status_option>
    | WITH <set_option> [ ,... ]
    }

<status_option> ::=ENABLE | DISABLE

<set_option> ::=
    PASSWORD ='password'
    [
      OLD_PASSWORD ='oldpassword'
      | <password_option> [<password_option> ]
    ]
    | NAME = login_name
    | CHECK_POLICY = { ON | OFF }
    | CHECK_EXPIRATION = { ON | OFF }

<password_option> ::=
    MUST_CHANGE | UNLOCK
```

## <a name="arguments"></a>Argumentos

*login_name* Especifica el nombre del inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que se está cambiando. Los inicios de sesión del dominio se deben incluir entre corchetes con el formato [dominio\usuario].

ENABLE | DISABLE Habilita o deshabilita este inicio de sesión. Deshabilitar un inicio de sesión no afecta al comportamiento de los inicios de sesión que ya están conectados. (Use la instrucción `KILL` para finalizar una conexión existente). Los inicios de sesión deshabilitados conservan sus permisos y pueden seguir siendo suplantados.

PASSWORD **='** _password_ **'** Se aplica solo a los inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Especifica la contraseña del inicio de sesión que se está cambiando. En las contraseñas se distingue entre mayúsculas y minúsculas.

> [!IMPORTANT]
> Cuando un inicio de sesión (o un usuario de base de datos independiente) se conecta y se autentica, la conexión almacena en memoria caché información de identidad del inicio de sesión. Para un inicio de sesión con autenticación de Windows, esto incluye la información sobre la pertenencia a grupos de Windows. La identidad del inicio de sesión permanecerá autenticada mientras dure la conexión. Para aplicar cambios en la identidad, como un cambio o restablecimiento de contraseña de la pertenencia al grupo de Windows, el inicio de sesión debe cerrar sesión en la entidad de autenticación (Windows o [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]) y volver a iniciar sesión. Los miembros del rol fijo de servidor **sysadmin** o cualquier inicio de sesión con el permiso **ALTER ANY CONNECTION** puede usar el comando **KILL** para finalizar una conexión y hacer que el inicio de sesión se vuelva a conectar. [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] puede reutilizar la información de conexión al abrir varias conexiones en las ventanas del Explorador de objetos y del Editor de consultas. Cierre todas las conexiones para forzar una reconexión.

OLD_PASSWORD **='** _oldpassword_ **'** Se aplica solo a los inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La contraseña actual del inicio de sesión al que se va a asignar una contraseña nueva. En las contraseñas se distingue entre mayúsculas y minúsculas.

MUST_CHANGE Solo se aplica a inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si se incluye esta opción, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pedirá la contraseña actualizada la primera vez que se utilice el inicio de sesión modificado.

NAME = *login_name* El nombre nuevo del inicio de sesión al que se está cambiando el nombre. Si se trata de un inicio de sesión de Windows, el SID de la entidad de seguridad de Windows correspondiente al nombre nuevo debe coincidir con el SID asociado al inicio de sesión en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. El nombre nuevo de un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no puede contener un carácter de barra diagonal inversa (\\).

CHECK_EXPIRATION = { ON | **OFF** } Se aplica solo a los inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Especifica si debe aplicarse la directiva de caducidad de contraseñas en este inicio de sesión. El valor predeterminado es OFF.

CHECK_POLICY **=** { **ON** | OFF } Se aplica solo a los inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Especifica que se deben aplicar las directivas de contraseñas de Windows en el equipo que ejecuta [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para este inicio de sesión. El valor predeterminado es ON.

UNLOCK Solo se aplica a inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Especifica que un inicio de sesión bloqueado debe desbloquearse.

## <a name="remarks"></a>Observaciones

Cuando CHECK_POLICY se establece en ON, no puede utilizarse el argumento HASHED.

Cuando el valor de CHECK_POLICY se cambia a ON, ocurre lo siguiente:

- El historial de contraseñas se inicializa con el valor del hash de contraseña actual.

  Cuando el valor de CHECK_POLICY se cambia a OFF, ocurre lo siguiente:

- La opción CHECK_EXPIRATION también se cambia a OFF.
- Se borra el historial de contraseñas.
- El valor de *lockout_time* se restablece.

Si se especifica MUST_CHANGE, CHECK_EXPIRATION y CHECK_POLICY, deben establecerse en ON. Si no es así, la instrucción producirá un error.

Si CHECK_POLICY se establece en OFF, CHECK_EXPIRATION no puede establecerse en ON. Una instrucción ALTER LOGIN con esta combinación de opciones dará error.

No puede usar ALTER_LOGIN con el argumento DISABLE para denegar el acceso a un grupo de Windows. es así por diseño. Por ejemplo, ALTER_LOGIN [*domain\group*] DISABLE devolverá el siguiente mensaje de error:

`"Msg 15151, Level 16, State 1, Line 1
"Cannot alter the login '*Domain\Group*', because it does not exist or you do not have permission."`

En [!INCLUDE[ssSDS](../../includes/sssds-md.md)], los datos de inicio de sesión necesarios para autenticar una conexión y reglas de firewall de nivel de servidor se almacenan temporalmente en caché en cada base de datos. Esta caché se actualiza regularmente. Para forzar una actualización de la caché de autenticación y garantizar que una base de datos tenga la versión más reciente de la tabla de inicios de sesión, ejecute [DBCC FLUSHAUTHCACHE](../../t-sql/database-console-commands/dbcc-flushauthcache-transact-sql.md).

## <a name="permissions"></a>Permisos

Requiere el permiso ALTER ANY LOGIN.

Si se utiliza la opción CREDENTIAL, también será necesario el permiso ALTER ANY CREDENTIAL.

Si el inicio de sesión que se está cambiado es un miembro del rol fijo de servidor **sysadmin** o tiene concedido el permiso CONTROL SERVER, también será necesario el permiso CONTROL SERVER para realizar los cambios siguientes:

- Restablecer la contraseña sin proporcionar la antigua.
- Habilitar MUST_CHANGE, CHECK_POLICY o CHECK_EXPIRATION.
- Cambiar el nombre de inicio de sesión.
- Habilitar o deshabilitar el inicio de sesión.
- Asignar el inicio de sesión a una credencial diferente.

Una entidad de seguridad puede cambiar la contraseña, el idioma predeterminado y la base de datos predeterminada para su propio inicio de sesión.

## <a name="examples"></a>Ejemplos

En estos ejemplos también se incluyen ejemplos de uso de otros productos SQL. Más arriba puede ver los argumentos que se admiten.

### <a name="a-enabling-a-disabled-login"></a>A. Habilitar un inicio de sesión deshabilitado

En el ejemplo siguiente se habilita el inicio de sesión `Mary5`.

```sql
ALTER LOGIN Mary5 ENABLE;
```

### <a name="b-changing-the-password-of-a-login"></a>B. Cambiar la contraseña de un inicio de sesión

En el ejemplo siguiente se cambia la contraseña del inicio de sesión `Mary5` a una contraseña segura.

```sql
ALTER LOGIN Mary5 WITH PASSWORD = '<enterStrongPasswordHere>';
```

### <a name="c-changing-the-name-of-a-login"></a>C. Cambiar el nombre de un inicio de sesión

En el ejemplo siguiente se cambia el nombre del inicio de sesión `Mary5` a `John2`.

```sql
ALTER LOGIN Mary5 WITH NAME = John2;
```

### <a name="d-mapping-a-login-to-a-credential"></a>D. Asignar un inicio de sesión a una credencial

En el ejemplo siguiente se asigna el inicio de sesión `John2` a la credencial `Custodian04`.

```sql
ALTER LOGIN John2 WITH CREDENTIAL = Custodian04;
```

### <a name="e-mapping-a-login-to-an-extensible-key-management-credential"></a>E. Asignar un inicio de sesión a una credencial de Administración de claves extensible

En el ejemplo siguiente se asigna el inicio de sesión `Mary5` a la credencial EKM `EKMProvider1`.

**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.

```sql
ALTER LOGIN Mary5
ADD CREDENTIAL EKMProvider1;
GO
```

### <a name="f-unlocking-a-login"></a>F. Desbloquear un inicio de sesión

Para desbloquear un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ejecute la instrucción siguiente y sustituya \*\*\*\* por la contraseña de cuenta deseada.

```sql
ALTER LOGIN [Mary5] WITH PASSWORD = '****' UNLOCK ;

GO
```

Para desbloquear un inicio de sesión sin cambiar la contraseña, desactive la directiva de comprobación y, a continuación, inicie sesión de nuevo.

```sql
ALTER LOGIN [Mary5] WITH CHECK_POLICY = OFF;
ALTER LOGIN [Mary5] WITH CHECK_POLICY = ON;
GO
```

### <a name="g-changing-the-password-of-a-login-using-hashed"></a>G. Cambiar la contraseña de un inicio de sesión utilizando HASHED

En el ejemplo siguiente se cambia la contraseña de inicio de sesión de `TestUser` a un valor que ya tiene aplicado hash.

**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.

```sql
ALTER LOGIN TestUser WITH
PASSWORD = 0x01000CF35567C60BFB41EBDE4CF700A985A13D773D6B45B90900 HASHED ;
GO
```

## <a name="see-also"></a>Consulte también

- [Credenciales](../../relational-databases/security/authentication-access/credentials-database-engine.md)
- [CREATE LOGIN](../../t-sql/statements/create-login-transact-sql.md)
- [DROP LOGIN](../../t-sql/statements/drop-login-transact-sql.md)
- [CREATE CREDENTIAL](../../t-sql/statements/create-credential-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [Administración extensible de claves (EKM)](../../relational-databases/security/encryption/extensible-key-management-ekm.md)

::: moniker-end
