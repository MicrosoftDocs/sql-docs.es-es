---
description: sp_setapprole (Transact-SQL)
title: sp_setapprole (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/12/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_setapprole
- sp_setapprole_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_setapprole
ms.assetid: cf0901c0-5f90-42d4-9d5b-8772c904062d
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: f34be78bc4d3f47ff2a11d0a6ed09c22c3670dca
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209282"
---
# <a name="sp_setapprole-transact-sql"></a>sp_setapprole (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Activa los permisos asociados a un rol de aplicación en la base de datos actual.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  

```sql
sp_setapprole [ @rolename = ] 'role',  
    [ @password = ] { encrypt N'password' }
      |  
        'password' [ , [ @encrypt = ] { 'none' | 'odbc' } ]  
        [ , [ @fCreateCookie = ] true | false ]  
    [ , [ @cookie = ] @cookie OUTPUT ]  
```

## <a name="arguments"></a>Argumentos

`[ @rolename = ] 'role'` Es el nombre del rol de aplicación definido en la base de datos actual. *role* es de **tipo sysname** y no tiene ningún valor predeterminado. el *rol* debe existir en la base de datos actual.  
  
`[ @password = ] { encrypt N'password' }` Es la contraseña requerida para activar el rol de aplicación. *password* es de **tipo sysname** y no tiene ningún valor predeterminado. la *contraseña* se puede ofuscar mediante la función **Encrypt** de ODBC. Cuando se usa la función **Encrypt** , la contraseña se debe convertir en una cadena Unicode colocando **N** delante de la primera comilla.  
  
 No se admite la opción Encrypt en las conexiones que usan **SqlClient**.  
  
> [!IMPORTANT]  
> La función **Encrypt** de ODBC no proporciona cifrado. No debe confiar a esta función la protección de las contraseñas que se transmiten en una red. Si esta información se va a transmitir a través de una red, use TLS o IPSec.
  
 **@encrypt = ' none '**  
 Especifica que no se utiliza ofuscación. La contraseña se pasa a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] como texto simple. Este es el valor predeterminado.  
  
 **@encrypt= ' ODBC '**  
 Especifica que ODBC ofuscará la contraseña mediante la función **Encrypt** de ODBC antes de enviar la contraseña a [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] . Solo se puede especificar cuando se utiliza un cliente ODBC o el Proveedor OLE DB para SQL Server.  
  
`[ @fCreateCookie = ] true | false` Especifica si se va a crear una cookie. **true** se convierte implícitamente a 1. **false** se convierte implícitamente a 0.  
  
`[ @cookie = ] @cookie OUTPUT` Especifica un parámetro de salida que contendrá la cookie. La cookie solo se genera si el valor de **\@ fCreateCookie** es **true**. **varbinary(8000)**  
  
> [!NOTE]  
> El parámetro **OUTPUT** de la cookie para **sp_setapprole** está documentado actualmente como **varbinary(8000)** , que es la longitud máxima correcta. Pero la implementación actual devuelve **varbinary(50)** . Las aplicaciones deben seguir reservando **varbinary (8000)** para que la aplicación siga funcionando correctamente si el tamaño de retorno de la cookie aumenta en una versión futura.
  
## <a name="return-code-values"></a>Valores de código de retorno

 0 (correcto) y 1 (error)  
  
## <a name="remarks"></a>Observaciones

 Después de activar un rol de aplicación mediante **sp_setapprole**, el rol permanece activo hasta que el usuario se desconecta del servidor o ejecuta **sp_unsetapprole**. **sp_setapprole** solo pueden ejecutarse mediante [!INCLUDE[tsql](../../includes/tsql-md.md)] instrucciones directas, en el nivel ad hoc y no dentro de otro procedimiento almacenado, desencadenador o dentro de una transacción definida por el usuario.  
  
 Para obtener información general sobre los roles de aplicación, consulte [roles de aplicación](../../relational-databases/security/authentication-access/application-roles.md).  
  
> [!IMPORTANT]  
> Para proteger la contraseña del rol de aplicación cuando se transmite a través de una red, siempre debe usar una conexión cifrada al habilitar un rol de aplicación.
> [!INCLUDE[msCoName](../../includes/msconame-md.md)] **SqlClient** no admite la opción de **cifrado** de ODBC. Si debe almacenar las credenciales, cífrelas con las funciones de la API de cifrado. La *contraseña* del parámetro se almacena como un hash unidireccional. Para conservar la compatibilidad con versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , **sp_addapprole** no aplica la Directiva de complejidad de contraseñas. Para aplicar la Directiva de complejidad de contraseñas, use [crear rol de aplicación](../../t-sql/statements/create-application-role-transact-sql.md).  
  
## <a name="permissions"></a>Permisos

Requiere la pertenencia al **público** y el conocimiento de la contraseña para el rol.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-activating-an-application-role-without-the-encrypt-option"></a>A. Activar un rol de aplicación sin la opción de cifrado

 En este ejemplo se habilita un rol de aplicación denominado `SalesAppRole`, con la contraseña de texto simple `AsDeF00MbXX`, creado con permisos específicamente diseñados para la aplicación que utiliza el usuario actual.

```sql
EXEC sys.sp_setapprole 'SalesApprole', 'AsDeF00MbXX';  
GO
```

### <a name="b-activating-an-application-role-with-a-cookie-and-then-reverting-to-the-original-context"></a>B. Activar un rol de aplicación con una cookie y revertir al contexto original

 En el siguiente ejemplo se habilita el rol de aplicación `Sales11` con la contraseña `fdsd896#gfdbfdkjgh700mM` y se crea una cookie. En el ejemplo se devuelve el nombre del usuario actual y se revierte al contexto original ejecutando `sp_unsetapprole`.  

```sql
DECLARE @cookie varbinary(8000);  
EXEC sys.sp_setapprole 'Sales11', 'fdsd896#gfdbfdkjgh700mM'  
    , @fCreateCookie = true, @cookie = @cookie OUTPUT;  
-- The application role is now active.  
SELECT USER_NAME();  
-- This will return the name of the application role, Sales11.  
EXEC sys.sp_unsetapprole @cookie;  
-- The application role is no longer active.  
-- The original context has now been restored.  
GO  
SELECT USER_NAME();  
-- This will return the name of the original user.
GO
```

## <a name="see-also"></a>Consulte también

 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Procedimientos almacenados de seguridad &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [CREATE APPLICATION ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-application-role-transact-sql.md)   
 [DROP APPLICATION ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-application-role-transact-sql.md)   
 [sp_unsetapprole &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-unsetapprole-transact-sql.md)
