---
description: CREATE USER (Transact-SQL)
title: CREATE USER (Transact-SQL)
ms.custom: ''
ms.date: 11/06/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- WITHOUT_LOGIN_TSQL
- CREATE_USER_TSQL
- SQL13.SWB.DATABASEUSER.OWNEDSCHEMAS.F1
- WITHOUT LOGIN
- CREATE USER
- ALLOW_ENCRYPTED_VALUE_MODIFICATIONS
- ALLOW_ENCRYPTED_VALUE_MODIFICATIONS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- adding users
- WITHOUT LOGIN [SQL Server]
- CREATE USER statement
- database user additions [SQL Server]
- USER WITHOUT LOGIN [SQL Server]
- users [SQL Server], adding
- users [SQL Server]
ms.assetid: 01de7476-4b25-4d58-85b7-1118fe64aa80
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6943409b7a0406a7012374b0c8a0ee7142550fe0
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237068"
---
# <a name="create-user-transact-sql"></a>CREATE USER (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Agrega un usuario a la base de datos actual. Los 12 tipos de usuarios se enumeran a continuación con un ejemplo de la sintaxis más sencilla:  
  
**Usuarios basados en inicios de sesión en la base de datos principal**: se trata del tipo de usuario más habitual.  
  
-   Usuario basado en un inicio de sesión basado en un grupo de Windows Active Directory. `CREATE USER [Contoso\Fritz];`     
-   Usuario basado en un inicio de sesión basado en un grupo de Windows. `CREATE USER [Contoso\Sales];`   
-   Usuario basado en un inicio de sesión mediante autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. `CREATE USER Mary;`  
  
**Usuarios que se autentican en la base de datos**: se recomienda para ayudar a que la base de datos sea más portable.  
 Siempre se admite en [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]. Solo se admite en una base de datos independiente en [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)].  
  
-   Usuario basado en un usuario de Windows sin inicio de sesión. `CREATE USER [Contoso\Fritz];`    
-   Usuario basado en un grupo de Windows sin inicio de sesión. `CREATE USER [Contoso\Sales];`  
-   Usuario de [!INCLUDE[ssSDS](../../includes/sssds-md.md)] o [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)] basado en un usuario de Azure Active Directory. `CREATE USER [Fritz@contoso.com] FROM EXTERNAL PROVIDER;`     

-   Usuario de base de datos independiente con contraseña. (No está disponible en [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)]). `CREATE USER Mary WITH PASSWORD = '********';`   
  
**Usuarios basados en entidades de seguridad de Windows que conectan a través de inicios de sesión de grupo de Windows**  
  
-   Usuario basado en un usuario de Windows sin inicio de sesión, pero que se puede conectar a [!INCLUDE[ssDE](../../includes/ssde-md.md)] mediante la pertenencia a un grupo de Windows. `CREATE USER [Contoso\Fritz];`  
  
-   Usuario basado en un grupo de Windows sin inicio de sesión, pero que se puede conectar a [!INCLUDE[ssDE](../../includes/ssde-md.md)] mediante la pertenencia a un grupo distinto de Windows. `CREATE USER [Contoso\Fritz];`  
  
**Usuarios que no se pueden autenticar**: estos usuarios no pueden iniciar sesión en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ni en [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
-   Usuario sin inicio de sesión. No puede iniciar sesión, pero se le pueden conceder permisos. `CREATE USER CustomApp WITHOUT LOGIN;`    
-   Usuario basado en un certificado. No puede iniciar sesión, pero se le pueden conceder permisos y puede firmar módulos. `CREATE USER TestProcess FOR CERTIFICATE CarnationProduction50;`  
-   Usuario basado en una clave asimétrica. No puede iniciar sesión, pero se le pueden conceder permisos y puede firmar módulos. `CREATE User TestProcess FROM ASYMMETRIC KEY PacificSales09;`   
 
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
-- Syntax for SQL Server, Azure SQL Database, and Azure SQL Managed Instance
  
-- Syntax Users based on logins in master  
CREATE USER user_name   
    [   
        { FOR | FROM } LOGIN login_name   
    ]  
    [ WITH <limited_options_list> [ ,... ] ]   
[ ; ]  
  
-- Users that authenticate at the database  
CREATE USER   
    {  
      windows_principal [ WITH <options_list> [ ,... ] ]  
  
    | user_name WITH PASSWORD = 'password' [ , <options_list> [ ,... ]   
    | Azure_Active_Directory_principal FROM EXTERNAL PROVIDER   
    }  
  
 [ ; ]  
  
-- Users based on Windows principals that connect through Windows group logins  
CREATE USER   
    {   
          windows_principal [ { FOR | FROM } LOGIN windows_principal ]  
        | user_name { FOR | FROM } LOGIN windows_principal  
}  
    [ WITH <limited_options_list> [ ,... ] ]   
[ ; ]  
  
-- Users that cannot authenticate   
CREATE USER user_name   
    {  
         WITHOUT LOGIN [ WITH <limited_options_list> [ ,... ] ]  
       | { FOR | FROM } CERTIFICATE cert_name   
       | { FOR | FROM } ASYMMETRIC KEY asym_key_name   
    }  
 [ ; ]  
  
<options_list> ::=  
      DEFAULT_SCHEMA = schema_name  
    | DEFAULT_LANGUAGE = { NONE | lcid | language name | language alias }  
    | SID = sid   
    | ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | OFF ] ]  
  
<limited_options_list> ::=  
      DEFAULT_SCHEMA = schema_name ]   
    | ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | OFF ] ]  
  
-- SQL Database syntax when connected to a federation member  
CREATE USER user_name  
[;]

-- Syntax for users based on Azure AD logins for Azure SQL Managed Instance
CREATE USER user_name   
    [   { FOR | FROM } LOGIN login_name  ]  
    | FROM EXTERNAL PROVIDER
    [ WITH <limited_options_list> [ ,... ] ]   
[ ; ]  

<limited_options_list> ::=  
      DEFAULT_SCHEMA = schema_name 
    | DEFAULT_LANGUAGE = { NONE | lcid | language name | language alias }   
    | ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | OFF ] ] 
```

> [!NOTE]
> La funcionalidad de administrador de Azure AD de Azures SQL Managed Instance después de la creación ha cambiado. Para obtener más información, consulte [Nueva funcionalidad de administrador de Azure AD para MI](/azure/sql-database/sql-database-aad-authentication-configure#new-azure-ad-admin-functionality-for-mi).

```syntaxsql
-- Syntax for Azure Synapse Analytics  
  
CREATE USER user_name   
    [ { { FOR | FROM } { LOGIN login_name }   
      | WITHOUT LOGIN  
    ]   
    [ WITH DEFAULT_SCHEMA = schema_name ]  
[;]

CREATE USER Azure_Active_Directory_principal FROM EXTERNAL PROVIDER  
    [ WITH DEFAULT_SCHEMA = schema_name ]  
[;]
``` 
  
```syntaxsql
-- Syntax for Parallel Data Warehouse  
  
CREATE USER user_name   
    [ { { FOR | FROM }  
      {   
        LOGIN login_name   
      }   
      | WITHOUT LOGIN  
    ]   
    [ WITH DEFAULT_SCHEMA = schema_name ]  
[;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *user_name*  
 Especifica el nombre por el que se identifica al usuario en esta base de datos. *user_name* es **sysname**. Puede tener una longitud máxima de 128 caracteres. Cuando se crea un usuario basado en una entidad de seguridad de Windows, el nombre de esta entidad se convierte en el nombre de usuario a menos que se especifique otro nombre de usuario.  
  
 LOGIN *login_name*  
 Especifica el inicio de sesión para el que se crea el usuario de base de datos. *login_name* debe ser un inicio de sesión válido en el servidor. Puede ser un inicio de sesión basado en una entidad de seguridad de Windows (usuario o grupo) o un inicio de sesión con autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cuando este inicio de sesión de SQL Server accede a la base de datos, adquiere el nombre y el identificador del usuario de la base de datos que se está creando. Cuando cree un inicio de sesión asignado a una entidad de seguridad de Windows, use el formato **[** _\<domainName\>_ **\\** _\<loginName\>_ **]** . Para obtener ejemplos, vea [Resumen de la sintaxis](#SyntaxSummary).  
  
 Si la instrucción CREATE USER es la única instrucción en un lote SQL, Azure SQL Database admite la cláusula WITH LOGIN. Si la instrucción CREATE USER no es la única instrucción en un lote SQL ni se ejecuta en SQL dinámico, la cláusula WITH LOGIN no se admite.  
  
 WITH DEFAULT_SCHEMA = *schema_name*  
 Especifica el primer esquema donde buscará el servidor cuando resuelva los nombres de objetos de este usuario de base de datos.  
  
 '*windows_principal*'  
 Especifica la entidad de seguridad de Windows para la que se crea el usuario de base de datos. *windows_principal* puede ser un usuario de Windows o un grupo de Windows. El usuario se creará aunque el parámetro *windows_principal* no disponga de inicio de sesión. Cuando se conecte a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], si el parámetro *windows_principal* no dispone de inicio de sesión, la entidad de seguridad de Windows se debe autenticar en [!INCLUDE[ssDE](../../includes/ssde-md.md)] mediante la pertenencia a un grupo de Windows que disponga de inicio de sesión o la cadena de conexión debe especificar la base de datos independiente como el catálogo inicial. Cuando cree un usuario a partir de una entidad de seguridad de Windows, use el formato **[** _\<domainName\>_ **\\** _\<loginName\>_ **]** . Para obtener ejemplos, vea [Resumen de la sintaxis](#SyntaxSummary). Los usuarios basados en usuarios de Active Directory se limitan a nombres de menos de 21 caracteres.
  
 '*Azure_Active_Directory_principal*'  
 **Se aplica a**: [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)], [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)].  
  
 Especifica la entidad de seguridad de Azure Active Directory para la que se crea el usuario de base de datos. *Azure_Active_Directory_principal* puede ser un usuario, un grupo o una aplicación de Azure Active Directory. (Los usuarios de Azure Active Directory no pueden tener inicios de sesión de autenticación de Windows en [!INCLUDE[ssSDS](../../includes/sssds-md.md)]; solo los usuarios de base de datos). La cadena de conexión debe especificar la base de datos independiente como el catálogo inicial.

 En las entidades de seguridad de Azure AD, la sintaxis de CREATE USER requiere:

- UserPrincipalName del objeto Azure AD para los usuarios de Azure AD.

  - `CREATE USER [bob@contoso.com] FROM EXTERNAL PROVIDER;`  
  - `CREATE USER [alice@fabrikam.onmicrosoft.com] FROM EXTERNAL PROVIDER;`

- DisplayName del objeto Azure AD para los grupos de Azure AD y las aplicaciones de Azure AD. Si tuviera el grupo de seguridad *Enfermeras*, utilizaría:  
  
  - `CREATE USER [Nurses] FROM EXTERNAL PROVIDER;`  
  
 Para más información, consulte [Usar la autenticación de Azure Active Directory para autenticación con SQL](/azure/azure-sql/database/authentication-aad-overview).  
  
WITH PASSWORD = '*password*'  
 **Se aplica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]  
  
 Solo se puede usar en una base de datos independiente. Especifica la contraseña del usuario que se está creando. A partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], la información de la contraseña almacenada se calcula con las funciones SHA-512 de la contraseña salada.  
  
WITHOUT LOGIN  
 Especifica que el usuario no se debe asignar a ningún inicio de sesión existente.  
  
CERTIFICATE *cert_name*  
 **Se aplica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]  
  
 Especifica el certificado del usuario de la base de datos que se crea.  
  
ASYMMETRIC KEY *asym_key_name*  
 **Se aplica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]  
  
 Especifica la clave asimétrica del usuario de la base de datos que se crea.  
  
DEFAULT_LANGUAGE = *{ NONE \| \<lcid> \| \<language name> \| \<language salias> }*  
 **Se aplica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]  
  
 Especifica el idioma predeterminado del nuevo usuario. Si se especifica un idioma predeterminado para el usuario y el idioma de la base de datos se cambia posteriormente, el idioma predeterminado de los usuarios se mantiene como se especificó. Si no se especifica ningún idioma predeterminado, el idioma predeterminado del usuario será el idioma predeterminado de la base de datos. Si no se especificó ningún idioma predeterminado para el usuario y el idioma predeterminado de la base de datos se cambia posteriormente, el idioma predeterminado del usuario se cambiará al nuevo idioma predeterminado de la base de datos.  
  
> [!IMPORTANT]  
>  *DEFAULT_LANGUAGE* solo se usa para un usuario de base de datos independiente.  
  
SID = *sid*  
 **Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.  
  
 Solo se aplica a los usuarios con contraseñas (autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]) en una base de datos independiente. Especifica el SID de la base de datos. Si esta opción no se selecciona, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] asigna automáticamente un SID. Utilice el parámetro SID para crear usuarios en varias bases de datos que tienen la misma identidad (SID). Esto es útil para crear usuarios de varias bases de datos y preparar la conmutación por error de Always On. Para determinar el SID de un usuario, consulte sys.database_principals.  
  
ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | **OFF** ]  
 **Se aplica a**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores, [!INCLUDE[ssSDS](../../includes/sssds-md.md)]  
  
 Suprime las comprobaciones de metadatos criptográficos en el servidor en operaciones de copia masiva. De esta manera, el usuario puede copiar los datos de forma masiva entre tablas o bases de datos, sin descifrar los datos. El valor predeterminado es OFF.  
  
> [!WARNING]  
>  Si esta opción no se utiliza adecuadamente, pueden dañarse los datos. Para obtener más información, vea [Migración de datos confidenciales protegidos mediante Always Encrypted](../../relational-databases/security/encryption/migrate-sensitive-data-protected-by-always-encrypted.md).  
  
## <a name="remarks"></a>Observaciones  
 Si se omite FOR LOGIN, el nuevo usuario de base de datos se asignará al inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con el mismo nombre.  
  
 El esquema predeterminado será el primer esquema donde buscará el servidor cuando resuelva los nombres de objetos de este usuario de base de datos. A menos que se especifique lo contrario, el esquema predeterminado será el propietario de los objetos creados por este usuario de base de datos.  
  
 Si el usuario tiene un esquema predeterminado, se utilizará dicho esquema. Si el usuario no tiene un esquema predeterminado pero es miembro de un grupo con un esquema predeterminado, se utilizará el esquema predeterminado del grupo. Si el usuario no tiene un esquema predeterminado y es miembro de varios grupos, el esquema predeterminado para el usuario será el del grupo de Windows con el principal_id mínimo y un esquema predeterminado establecido explícitamente. (No es posible seleccionar explícitamente uno de los esquemas predeterminados disponibles como esquema preferido). Si no se puede determinar ningún esquema predeterminado para un usuario, se utilizará el esquema **dbo**.  
  
 DEFAULT_SCHEMA puede establecerse antes de crear el esquema que lo señala.  
  
 No se puede especificar DEFAULT_SCHEMA cuando se está creando un usuario asignado a un certificado o una clave asimétrica.  
  
 El valor de DEFAULT_SCHEMA se omite si el usuario es un miembro del rol fijo de servidor sysadmin. Todos los miembros del rol fijo de servidor sysadmin tienen un esquema predeterminado de `dbo`.  
  
 La cláusula WITHOUT LOGIN crea un usuario que no se asigna a ningún inicio de sesión de SQL Server. Este usuario puede conectarse a otras bases de datos como guest. Se pueden asignar permisos a este usuario sin inicio de sesión y cuando cambie el contexto de seguridad a un usuario sin inicio de sesión, el usuario original recibe el permiso del usuario sin inicio de sesión. Vea el ejemplo [D. Crear y utilizar un usuario sin inicio de sesión](#withoutLogin).  
  
 Solo los usuarios asignados a entidades de seguridad de Windows pueden contener el carácter de barra diagonal inversa ( **\\** ).
  
 No se puede utilizar CREATE USER para crear un usuario guest porque el usuario guest ya existe en cada base de datos. Puede habilitar el usuario guest concediéndole el permiso CONNECT como se muestra a continuación:  
  
```  
GRANT CONNECT TO guest;  
GO  
```  
  
 Puede ver la información sobre los usuarios de bases de datos en la vista de catálogo [sys.database_principals](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md).

Una nueva extensión de sintaxis, **FROM EXTERNAL PROVIDER**, está disponible para la creación de inicios de sesión de Azure AD de nivel de servidor en SQL Managed Instance. Los inicios de sesión de Azure AD permiten asignar entidades de seguridad de Azure AD de nivel de base de datos a inicios de sesión de Azure AD de nivel de servidor. Para crear un usuario de Azure AD a partir de un inicio de sesión de Azure AD, use la sintaxis siguiente:

`CREATE USER [AAD_principal] FROM LOGIN [Azure AD login]`

Al crear el usuario en la base de datos de SQL Managed Instance, login_name se debe corresponder a un inicio de sesión existente de Azure AD, o de lo contrario la cláusula **FROM EXTERNAL PROVIDER** solo creará un usuario de Azure AD sin un inicio de sesión en la base de datos principal. Por ejemplo, este comando creará un usuario contenido:

`CREATE USER [bob@contoso.com] FROM EXTERNAL PROVIDER`
  
##  <a name="syntax-summary"></a><a name="SyntaxSummary"></a> Resumen de la sintaxis  
 **Usuarios basados en inicios de sesión en la base de datos maestra**  
  
 En la siguiente lista se muestra la posible sintaxis de los usuarios basados en inicios de sesión. No se citan las opciones de esquema predeterminadas.  
  
-   `CREATE USER [Domain1\WindowsUserBarry]`  
-   `CREATE USER [Domain1\WindowsUserBarry] FOR LOGIN Domain1\WindowsUserBarry`  
-   `CREATE USER [Domain1\WindowsUserBarry] FROM LOGIN Domain1\WindowsUserBarry`  
-   `CREATE USER [Domain1\WindowsGroupManagers]`  
-   `CREATE USER [Domain1\WindowsGroupManagers] FOR LOGIN [Domain1\WindowsGroupManagers]`  
-   `CREATE USER [Domain1\WindowsGroupManagers] FROM LOGIN [Domain1\WindowsGroupManagers]`  
-   `CREATE USER SQLAUTHLOGIN`  
-   `CREATE USER SQLAUTHLOGIN FOR LOGIN SQLAUTHLOGIN`  
-   `CREATE USER SQLAUTHLOGIN FROM LOGIN SQLAUTHLOGIN`  
  
**Usuarios que se autentican en la base de datos**  
  
 En la siguiente lista se muestra la posible sintaxis de los usuarios que solo se puede usar en una base de datos independiente. Los usuarios creados no se podrán relacionar con los inicios de sesión en la base de datos **maestra**. No se muestran las opciones predeterminadas de esquema ni de idioma.  
  
> [!IMPORTANT]  
>  Esta sintaxis concede acceso a los usuarios a la base de datos, así como nuevo acceso a [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
-   `CREATE USER [Domain1\WindowsUserBarry]`  
-   `CREATE USER [Domain1\WindowsGroupManagers]`  
-   `CREATE USER Barry WITH PASSWORD = 'sdjklalie8rew8337!$d'`  
  
**Usuarios basados en entidades de seguridad de Windows sin inicios de sesión en la base de datos maestra**  
  
 En la siguiente lista se muestra la posible sintaxis para los usuarios que tienen acceso a [!INCLUDE[ssDE](../../includes/ssde-md.md)] mediante un grupo de Windows, pero que no disponen de inicio de sesión en la base de datos **maestra**. Esta sintaxis se puede usar en todos los tipos de bases de datos. No se muestran las opciones predeterminadas de esquema ni de idioma.  
  
 Esta sintaxis es similar a la de los usuarios basados en inicios de sesión en la base de datos maestra, pero esta categoría no dispone de inicio de sesión en dicha base de datos. El usuario debe tener acceso a [!INCLUDE[ssDE](../../includes/ssde-md.md)] mediante un inicio de sesión de grupo de Windows.  
  
 Esta sintaxis es similar a la de los usuarios de bases de datos independientes basados en las entidades de seguridad de Windows, pero esta categoría de usuario no obtiene nuevo acceso a [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
-   `CREATE USER [Domain1\WindowsUserBarry]`  
-   `CREATE USER [Domain1\WindowsUserBarry] FOR LOGIN Domain1\WindowsUserBarry`  
-   `CREATE USER [Domain1\WindowsUserBarry] FROM LOGIN Domain1\WindowsUserBarry`  
-   `CREATE USER [Domain1\WindowsGroupManagers]`  
-   `CREATE USER [Domain1\WindowsGroupManagers] FOR LOGIN [Domain1\WindowsGroupManagers]`  
-   `CREATE USER [Domain1\WindowsGroupManagers] FROM LOGIN [Domain1\WindowsGroupManagers]`  
  
**Usuarios que no se pueden autenticar**  
  
 En la siguiente lista se muestra la posible sintaxis para los usuarios que no pueden iniciar sesión en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   `CREATE USER RIGHTSHOLDER WITHOUT LOGIN`  
-   `CREATE USER CERTUSER FOR CERTIFICATE SpecialCert`  
-   `CREATE USER CERTUSER FROM CERTIFICATE SpecialCert`  
-   `CREATE USER KEYUSER FOR ASYMMETRIC KEY SecureKey`  
-   `CREATE USER KEYUSER FROM ASYMMETRIC KEY SecureKey`  
  
## <a name="security"></a>Seguridad  
 Si se crea un usuario, se le concede acceso a una base de datos, pero no se le concede ningún acceso automáticamente a los objetos de una base de datos. Después de crear un usuario, las acciones habituales son agregar usuarios a los roles de base de datos que tienen permiso para acceder a los objetos de base de datos, o bien conceder permisos de objeto al usuario. Para más información acerca de cómo diseñar un sistema de permisos, consulte [Getting Started with Database Engine Permissions](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md).  
  
### <a name="special-considerations-for-contained-databases"></a>Consideraciones especiales para bases de datos independientes  
 Cuando se conecte a una base de datos independiente, si el usuario no dispone de inicio de sesión en la base de datos **maestra**, la cadena de conexión se debe incluir en el nombre de la base de datos independiente como el catálogo inicial. El parámetro de catálogo inicial siempre es necesario para un usuario de base de datos independiente con contraseña.  
  
 En una base de datos independiente, la creación de usuarios ayuda a separar la base de datos de la instancia de [!INCLUDE[ssDE](../../includes/ssde-md.md)] para que la base de datos se pueda mover fácilmente a otra instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener más información, vea [Bases de datos independientes](../../relational-databases/databases/contained-databases.md) y [Usuarios de base de datos independiente: hacer que la base de datos sea portátil](../../relational-databases/security/contained-database-users-making-your-database-portable.md). Para cambiar un usuario de base de datos de un usuario basado en un inicio de sesión de autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a un usuario de base de datos independiente con contraseña, vea [sp_migrate_user_to_contained &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md).  
  
 En una base de datos independiente, los usuarios no deben disponer de inicios de sesión en la base de datos **maestra**. Los administradores del [!INCLUDE[ssDE](../../includes/ssde-md.md)] deben comprender que el acceso a una base de datos independiente se puede conceder en el nivel de base de datos, en vez de en el nivel de [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Para más información, vea [Security Best Practices with Contained Databases](../../relational-databases/databases/security-best-practices-with-contained-databases.md).  
  
 Al usar los usuarios de la base de datos independiente de [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], configure el acceso con una regla de firewall de nivel de base de datos en lugar de una regla de firewall de nivel de servidor. Para obtener más información, vea [sp_set_database_firewall_rule &#40;Base de datos SQL de Azure&#41;](../../relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database.md).
 
Para los usuarios de base de datos independiente de [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] y [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)], SSMS puede admitir la autenticación multifactor. Para obtener más información, vea [Compatibilidad de SSMS con Azure AD MFA con SQL Database y Azure Synapse[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://azure.microsoft.com/documentation/articles/sql-database-ssms-mfa-authentication/).  
  
### <a name="permissions"></a>Permisos  
 Requiere el permiso ALTER ANY USER en la base de datos.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-creating-a-database-user-based-on-a-sql-server-login"></a>A. Crear un usuario de base de datos basado en un inicio de sesión de SQL Server  
 En el siguiente ejemplo, primero se crea un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] denominado `AbolrousHazem` y, a continuación, se crea un usuario de base de datos correspondiente `AbolrousHazem` en `AdventureWorks2012`.  
  
```sql  
CREATE LOGIN AbolrousHazem   
    WITH PASSWORD = '340$Uuxwp7Mcxo7Khy';  
```   
Cambie a una base de datos de usuario. Por ejemplo, en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], utilice la instrucción `USE AdventureWorks2012`. En [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], debe realizar una nueva conexión con la base de datos de usuario.

```sql   
CREATE USER AbolrousHazem FOR LOGIN AbolrousHazem;  
GO   
```  
  
### <a name="b-creating-a-database-user-with-a-default-schema"></a>B. Crear un usuario de base de datos con un esquema predeterminado  
 En el siguiente ejemplo, primero se crea un inicio de sesión de servidor denominado `WanidaBenshoof` con una contraseña y, a continuación, se crea el usuario de base de datos `Wanida` correspondiente con el esquema predeterminado `Marketing`.  
  
```sql  
CREATE LOGIN WanidaBenshoof   
    WITH PASSWORD = '8fdKJl3$nlNv3049jsKK';  
USE AdventureWorks2012;  
CREATE USER Wanida FOR LOGIN WanidaBenshoof   
    WITH DEFAULT_SCHEMA = Marketing;  
GO  
```  
  
### <a name="c-creating-a-database-user-from-a-certificate"></a>C. Crear un usuario de base de datos a partir de un certificado  
 En el siguiente ejemplo, se crea el usuario de base de datos `JinghaoLiu` desde el certificado `CarnationProduction50`.  
  
**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
```sql  
USE AdventureWorks2012;  
CREATE CERTIFICATE CarnationProduction50  
    WITH SUBJECT = 'Carnation Production Facility Supervisors',  
    EXPIRY_DATE = '11/11/2011';  
GO  
CREATE USER JinghaoLiu FOR CERTIFICATE CarnationProduction50;  
GO   
```  
  
###  <a name="d-creating-and-using-a-user-without-a-login"></a><a name="withoutLogin"></a> D. Crear y utilizar un usuario sin inicio de sesión  
 En el ejemplo siguiente se crea un usuario de base de datos `CustomApp` que no se asigna a un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. A continuación, se concede a un usuario el permiso `adventure-works\tengiz0` para suplantar al usuario de `CustomApp`.  
  
```sql  
USE AdventureWorks2012 ;  
CREATE USER CustomApp WITHOUT LOGIN ;  
GRANT IMPERSONATE ON USER::CustomApp TO [adventure-works\tengiz0] ;  
GO   
```  
  
 Para usar las credenciales de `CustomApp`, el usuario `adventure-works\tengiz0` ejecuta la siguiente instrucción.  
  
```sql  
EXECUTE AS USER = 'CustomApp' ;  
GO  
```  
  
 Para revertir a las credenciales de `adventure-works\tengiz0`, el usuario ejecuta la siguiente instrucción.  
  
```sql  
REVERT ;  
GO  
```  
  
### <a name="e-creating-a-contained-database-user-with-password"></a>E. Crear un usuario de base de datos independiente con contraseña  
 En el siguiente ejemplo se crea un usuario de base de datos independiente con contraseña. Este ejemplo solo se puede ejecutar en una base de datos independiente.  
  
**Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores. Este ejemplo funciona en [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] si se quita DEFAULT_LANGUAGE.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
CREATE USER Carlo  
WITH PASSWORD='RN92piTCh%$!~3K9844 Bl*'  
    , DEFAULT_LANGUAGE=[Brazilian]  
    , DEFAULT_SCHEMA=[dbo]  
GO   
```  
  
### <a name="f-creating-a-contained-database-user-for-a-domain-login"></a>F. Crear un usuario de base de datos independiente para un inicio de sesión de dominio  
 En el siguiente ejemplo se crea un usuario de base de datos independiente para un inicio de sesión denominado Fritz en un dominio denominado Contoso. Este ejemplo solo se puede ejecutar en una base de datos independiente.  
  
**Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
CREATE USER [Contoso\Fritz] ;  
GO   
```  
  
### <a name="g-creating-a-contained-database-user-with-a-specific-sid"></a>G. Crear un usuario de base de datos independiente con un SID específico  
 En el siguiente ejemplo se crea un usuario de base de datos independiente y autenticado de SQL Server denominado CarmenW. Este ejemplo solo se puede ejecutar en una base de datos independiente.  
  
**Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
CREATE USER CarmenW WITH PASSWORD = 'a8ea v*(Rd##+'  
, SID = 0x01050000000000090300000063FF0451A9E7664BA705B10E37DDC4B7;
```  
  
### <a name="h-creating-a-user-to-copy-encrypted-data"></a>H. Crear un usuario para copiar los datos cifrados  
 En el siguiente ejemplo se crea un usuario que puede copiar datos protegidos por la característica Always Encrypted de un conjunto de tablas con las columnas cifradas a otro conjunto de tablas con columnas cifradas (en la misma base de datos o en otra diferente).  Para obtener más información, vea [Migración de datos confidenciales protegidos mediante Always Encrypted](../../relational-databases/security/encryption/migrate-sensitive-data-protected-by-always-encrypted.md).  
  
**Se aplica a**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores, [!INCLUDE[ssSDS](../../includes/sssds-md.md)]  
  
```sql  
CREATE USER [Chin]   
WITH   
      DEFAULT_SCHEMA = dbo  
    , ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = ON ;  
```

### <a name="i-create-an-azure-ad-user-from-an-azure-ad-login-in-sql-managed-instance"></a>I. Creación de un usuario de Azure AD a partir de un inicio de sesión de Azure AD en SQL Managed Instance

 Para crear un usuario de Azure AD a partir de un inicio de sesión de Azure AD, use la sintaxis siguiente.

 Inicie sesión en la Instancia administrada con un inicio de sesión de Azure AD que tenga concedido el rol `sysadmin`. Lo siguiente crea un usuario de Azure AD bob@contoso.com, a partir del inicio de sesión bob@contoso.com. Este inicio de sesión se ha creado en el ejemplo [CREATE LOGIN](./create-login-transact-sql.md#examples).

```sql
CREATE USER [bob@contoso.com] FROM LOGIN [bob@contoso.com];
GO
```

> [!IMPORTANT]
> Al crear **USER** a partir un inicio de sesión de Azure AD, especifique el valor de *user_name* igual que el de *login_name* de **LOGIN**.

Se admite la creación de un usuario de Azure AD como un grupo a partir de un inicio de sesión de Azure AD que sea un grupo.

```sql
CREATE USER [AAD group] FROM LOGIN [AAD group];
GO
```

También se puede crear un usuario de Azure AD a partir de un inicio de sesión de Azure AD que sea un grupo.

```sql
CREATE USER [bob@contoso.com] FROM LOGIN [AAD group];
GO
```

### <a name="j-create-an-azure-ad-user-without-an-aad-login-for-the-database"></a>J. Creación de un usuario de Azure AD sin un inicio de sesión de AAD para la base de datos

La sintaxis siguiente se usa para crear un usuario de Azure AD bob@contoso.com en la base de datos de SQL Managed Instance (usuario contenido):

```sql
CREATE USER [bob@contoso.com] FROM EXTERNAL PROVIDER;
GO
```

## <a name="next-steps"></a>Pasos siguientes  
Una vez creado el usuario, considere la posibilidad de agregar el usuario a un rol de base de datos mediante la instrucción [ALTER ROLE](../../t-sql/statements/alter-role-transact-sql.md).  
También puede interesarle [otorgar permisos de objeto](../../t-sql/statements/grant-object-permissions-transact-sql.md) al rol para que pueda acceder a las tablas. Para obtener información general sobre el modelo de seguridad de SQL Server, vea [Permisos](../../relational-databases/security/permissions-database-engine.md).   
  
## <a name="see-also"></a>Consulte también  
 [Crear un usuario de base de datos](../../relational-databases/security/authentication-access/create-a-database-user.md)   
 [sys.database_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)   
 [ALTER USER &#40;Transact-SQL&#41;](../../t-sql/statements/alter-user-transact-sql.md)   
 [DROP USER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-user-transact-sql.md)   
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [Bases de datos independientes](../../relational-databases/databases/contained-databases.md)   
 [Uso de la autenticación de Azure Active Directory con SQL Database](/azure/azure-sql/database/authentication-aad-overview)   
 [Introducción a los permisos de los motores de bases de datos](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)