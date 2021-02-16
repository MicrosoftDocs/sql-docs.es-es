---
description: EXECUTE AS (Transact-SQL)
title: EXECUTE AS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/27/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- EXECUTE AS
- EXECUTE_AS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- REVERT statement
- WITH NO REVERT clause
- sessions [SQL Server], execution context
- EXECUTE AS
- execution context [SQL Server]
- switching execution context
ms.assetid: 613b8271-7f7d-4378-b7a2-5a7698551dbd
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current || >= sql-server-2016 || >= sql-server-linux-2017||=azure-sqldw-latest
ms.openlocfilehash: b9dd4dc3d830a93ee2b3f99b7b285832e5bb402e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340937"
---
# <a name="execute-as-transact-sql"></a>EXECUTE AS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Establece el contexto de ejecución de una sesión.  
  
 De forma predeterminada, una sesión empieza cuando un usuario inicia la sesión y termina cuando el usuario la cierra. Todas las operaciones durante una sesión están sujetas a comprobaciones de permisos de ese usuario. Cuando se ejecuta una instrucción **EXECUTE AS**, el contexto de ejecución de la sesión se cambia al nombre de usuario o inicio de sesión especificado. Después de cambiar el contexto, se comprueban los permisos con respecto a los tokens de seguridad de usuario e inicio de sesión de esa cuenta, en lugar de la persona que llama a la instrucción **EXECUTE AS**. Básicamente, la cuenta de inicio de sesión o usuario se suplanta durante la ejecución del módulo o la sesión, o el cambio de contexto se revierte de forma explícita.  
  

  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
{ EXEC | EXECUTE } AS <context_specification>  
[;]  
  
<context_specification>::=  
{ LOGIN | USER } = 'name'  
    [ WITH { NO REVERT | COOKIE INTO @varbinary_variable } ]   
| CALLER  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 LOGIN  
 **Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
 Especifica que el contexto de ejecución que se va a suplantar es un inicio de sesión. El ámbito de la suplantación se produce en el nivel de servidor.  
  
> [!NOTE]  
>  Esta opción no está disponible en una base de datos independiente ni en SQL Database ni en [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)].  
  
 USER  
 Especifica que el contexto de ejecución que se va a suplantar es un usuario de la base de datos actual. El ámbito de la suplantación se restringe a la base de datos actual. Un cambio de contexto a un usuario de base de datos no hereda los permisos de nivel de servidor de ese usuario.  
  
> [!IMPORTANT]  
>  Mientras el cambio de contexto al usuario de la base de datos esté activo, cualquier intento de acceso a recursos fuera de la base de datos provocará que la instrucción genere errores. Esto incluye instrucciones USE *database*, consultas distribuidas y consultas que hacen referencia a otra base de datos que usa identificadores de tres o cuatro partes.  
  
 "*name*" es un nombre de usuario o de inicio de sesión válido. *name* debe ser miembro del rol fijo de servidor **sysadmin** o existir como entidad de seguridad en [sys.database_principals](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md) o [sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md) respectivamente.  
  
 *name* se puede especificar como una variable local.  
  
 *name* debe ser una cuenta singleton y no puede ser un grupo, rol, certificado, clave o cuenta integrada, como NT AUTHORITY\LocalService, NT AUTHORITY\NetworkService o NT AUTHORITY\LocalSystem.  
  
 Para más información, vea [Especificar un nombre de inicio de sesión o usuario](#_user) más adelante en este tema.  
  
 NO REVERT  
 Especifica que el cambio de contexto no se puede volver al contexto anterior. La opción **NO REVERT** solo se puede usar en el nivel ad hoc.  
  
 Para más información sobre cómo volver al contexto anterior, vea [REVERT &#40;Transact-SQL&#41;](../../t-sql/statements/revert-transact-sql.md).  
  
 COOKIE INTO @*varbinary_variable*  
 Especifica que el contexto de ejecución solo se puede revertir al contexto anterior si la llamada a la instrucción REVERT WITH COOKIE contiene el valor de @*varbinary_variable* correcto. [!INCLUDE[ssDE](../../includes/ssde-md.md)] pasa la cookie a @*varbinary_variable*. La opción **COOKIE INTO** solo se puede usar en el nivel ad hoc.  
  
 @*varbinary_variable* es **varbinary(8000)** .  
  
> [!NOTE]  
>  El parámetro **OUTPUT** de la cookie está documentado actualmente como **varbinary(8000)** , que es la longitud máxima correcta. Pero la implementación actual devuelve **varbinary(100)** . Las aplicaciones deben reservar **varbinary(8000)** para que la aplicación siga funcionando correctamente si el tamaño de retorno de la cookie aumenta en una versión futura.  
  
 CALLER  
 Cuando se usa dentro de un módulo, especifica que las instrucciones dentro del módulo se ejecutan en el contexto del autor de llamada del módulo.
Cuando se utiliza fuera de un módulo, la instrucción no tiene ninguna acción.
 > [!NOTE]  
>  Esta opción no está disponible en [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)].  
  
## <a name="remarks"></a>Observaciones  
 El cambio en el contexto de ejecución continúa efectivo hasta que sucede algo de lo siguiente:  
  
-   Se ejecuta otra instrucción EXECUTE AS.  
  
-   Se ejecuta una instrucción REVERT.  
  
-   Se elimina la sesión.  
  
-   Se sale del procedimiento almacenado o el desencadenador en el que se ejecutaba el comando.  
  
Puede crear una pila de contextos de ejecución si llama a la instrucción EXECUTE AS varias veces en diversas entidades de seguridad. Cuando se llama, la instrucción REVERT cambia el contexto al inicio de sesión o el usuario del siguiente nivel en la pila de contextos. Para ver una demostración de este comportamiento, vea el [ejemplo A](#_exampleA).  
  
##  <a name="specifying-a-user-or-login-name"></a><a name="_user"></a> Especificar un nombre de inicio de sesión o usuario  
 El nombre de inicio de sesión o usuario especificado en EXECUTE AS \<context_specification> debe existir como una entidad de seguridad en **sys.database_principals** o **sys.server_principals** respectivamente, o bien se producirá un error en la instrucción EXECUTE AS. Además, se deben conceder permisos IMPERSONATE en la entidad de seguridad. A menos que el autor de la llamada sea el propietario de la base de datos o sea un miembro del rol fijo de servidor **sysadmin**, la entidad de seguridad debe existir aun cuando el usuario tenga acceso a la base de datos o instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante la pertenencia a un grupo de Windows. Por ejemplo, supongamos las siguientes condiciones: 
  
-   El grupo **CompanyDomain\SQLUsers** tiene acceso a la base de datos **Sales**.  
  
-   **CompanyDomain\SqlUser1** es un miembro de **SQLUsers** y, por tanto, tiene acceso implícito a la base de datos **Sales**.  
  
 Aunque **CompanyDomain\SqlUser1** tiene acceso a la base de datos a través de la pertenencia al grupo **SQLUsers**, la instrucción `EXECUTE AS USER = 'CompanyDomain\SqlUser1'` generará errores porque `CompanyDomain\SqlUser1` no existe como una entidad de seguridad en la base de datos.  
  
Si el usuario está huérfano (el inicio de sesión asociado ya no existe) y no se creó con **WITHOUT LOGIN**, **EXECUTE AS** generará un error para el usuario.  
  
## <a name="best-practice"></a>Procedimiento recomendado  
 Especifique un inicio de sesión o usuario que tenga al menos los privilegios requeridos para realizar las operaciones en la sesión. Por ejemplo, no especifique un nombre de inicio de sesión con permisos de nivel de servidor si solo se necesitan permisos de nivel de base de datos; o no especifique una cuenta de propietario de base de datos a menos que se necesiten esos permisos.  
  
> [!CAUTION]  
>  La instrucción EXECUTE AS puede ejecutarse correctamente siempre y cuando el [!INCLUDE[ssDE](../../includes/ssde-md.md)] pueda resolver el nombre. Si un usuario del dominio existe, Windows puede resolver el usuario para el [!INCLUDE[ssDE](../../includes/ssde-md.md)], incluso aunque el usuario de Windows no tenga acceso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Esto puede producir una situación en la cual un inicio de sesión sin acceso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] parezca que ha iniciado sesión, aunque el inicio de sesión suplantado solo tendría los permisos concedidos a un usuario público o a un invitado.  
  
## <a name="using-with-no-revert"></a>Usar WITH NO REVERT  
 Cuando la instrucción EXECUTE AS incluye la cláusula opcional WITH NO REVERT, el contexto de ejecución de una sesión no se puede restablecer mediante REVERT o ejecutando otra instrucción EXECUTE AS. El contexto establecido por la instrucción permanece efectivo hasta que elimina la sesión.  
  
 Cuando se especifica la cláusula WITH NO REVERT COOKIE = @*varbinary_variable*, [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] pasa el valor de la cookie a @*varbinary_variable*. El contexto de ejecución que establece esa instrucción se puede revertir al contexto anterior solo si la llamada a la instrucción REVERT WITH COOKIE = @*varbinary_variable* contiene el mismo valor *\@varbinary_variable*.  
  
 Esta opción es útil en un entorno donde se utiliza la agrupación de conexiones. La agrupación de conexiones es el mantenimiento de un grupo de base de datos que reutilizan las aplicaciones en un servidor de aplicaciones. Puesto que el valor pasado a *\@varbinary_variable* solo lo conoce el autor de la llamada de la instrucción EXECUTE AS, el autor de la llamada puede garantizar que el contexto de ejecución que establece no puede cambiarlo nadie.  
  
## <a name="determining-the-original-login"></a>Determinar el inicio de sesión original  
 Use la función [ORIGINAL_LOGIN](../../t-sql/functions/original-login-transact-sql.md) para devolver el nombre de inicio de sesión que se ha conectado a la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Puede utilizar esta función para devolver la identidad del inicio de sesión original en sesiones en las que hay varios cambios de contexto explícitos o implícitos.  
  
## <a name="permissions"></a>Permisos  
 Para especificar **EXECUTE AS** en un inicio de sesión, el autor de la llamada debe tener el permiso **IMPERSONATE** en el nombre de inicio de sesión especificado y no se le debe haber denegado el permiso **IMPERSONATE ANY LOGIN**. Para especificar **EXECUTE AS** en un usuario de base de datos, el autor de llamada debe tener el permiso **IMPERSONATE** en el nombre de usuario especificado. Cuando se especifica **EXECUTE AS CALLER**, no se necesitan permisos **IMPERSONATE**.  
  
## <a name="examples"></a>Ejemplos  
  
###  <a name="a-using-execute-as-and-revert-to-switch-context"></a><a name="_exampleA"></a> A. Usar EXECUTE AS y REVERT para cambiar el contexto  
 En el siguiente ejemplo se crea una pila de contextos de ejecución que utilizan varias entidades de seguridad. La instrucción `REVERT` se utiliza a continuación para restablecer el contexto de ejecución al anterior autor de la llamada. La instrucción `REVERT` se ejecuta varias veces subiendo la pila hasta que el contexto de ejecución se establezca en el autor de la llamada original.  
  

```sql
USE AdventureWorks2012;  
GO  
--Create two temporary principals  
CREATE LOGIN login1 WITH PASSWORD = 'J345#$)thb';  
CREATE LOGIN login2 WITH PASSWORD = 'Uor80$23b';  
GO  
CREATE USER user1 FOR LOGIN login1;  
CREATE USER user2 FOR LOGIN login2;  
GO  
--Give IMPERSONATE permissions on user2 to user1  
--so that user1 can successfully set the execution context to user2.  
GRANT IMPERSONATE ON USER:: user2 TO user1;  
GO  
--Display current execution context.  
SELECT SUSER_NAME(), USER_NAME();  
-- Set the execution context to login1.   
EXECUTE AS LOGIN = 'login1';  
--Verify the execution context is now login1.  
SELECT SUSER_NAME(), USER_NAME();  
--Login1 sets the execution context to login2.  
EXECUTE AS USER = 'user2';  
--Display current execution context.  
SELECT SUSER_NAME(), USER_NAME();  
-- The execution context stack now has three principals: the originating caller, login1 and login2.  
--The following REVERT statements will reset the execution context to the previous context.  
REVERT;  
--Display current execution context.  
SELECT SUSER_NAME(), USER_NAME();  
REVERT;  
--Display current execution context.  
SELECT SUSER_NAME(), USER_NAME();  
  
--Remove temporary principals.  
DROP LOGIN login1;  
DROP LOGIN login2;  
DROP USER user1;  
DROP USER user2;  
GO  
```  
  
### <a name="b-using-the-with-cookie-clause"></a>B. Usar la cláusula WITH COOKIE  
 En este ejemplo se establece el contexto de ejecución de una sesión en un usuario determinado y se especifica la cláusula WITH NO REVERT COOKIE = @*varbinary_variable*. La instrucción `REVERT` debe especificar el valor pasado a la variable `@cookie` en la instrucción `EXECUTE AS` para volver correctamente el contexto al llamador. Para ejecutar este ejemplo, deben existir el inicio de sesión `login1` y el usuario `user1` creados en el ejemplo A.  
  

```sql 
DECLARE @cookie VARBINARY(8000);  
EXECUTE AS USER = 'user1' WITH COOKIE INTO @cookie;  
-- Store the cookie in a safe location in your application.  
-- Verify the context switch.  
SELECT SUSER_NAME(), USER_NAME();  
--Display the cookie value.  
SELECT @cookie;  
GO  
-- Use the cookie in the REVERT statement.  
DECLARE @cookie VARBINARY(8000);  
-- Set the cookie value to the one from the SELECT @cookie statement.  
SET @cookie = <value from the SELECT @cookie statement>;  
REVERT WITH COOKIE = @cookie;  
-- Verify the context switch reverted.  
SELECT SUSER_NAME(), USER_NAME();  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [REVERT &#40;Transact-SQL&#41;](../../t-sql/statements/revert-transact-sql.md)   
 [EXECUTE AS &#40;cláusula de Transact-SQL&#41;](../../t-sql/statements/execute-as-clause-transact-sql.md)  
  
  

