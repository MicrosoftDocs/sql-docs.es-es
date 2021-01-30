---
description: sp_migrate_user_to_contained (Transact-SQL)
title: sp_migrate_user_to_contained (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/11/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_migrate_user_to_contained
- sp_migrate_user_to_contained_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_migrate_user_to_contained
ms.assetid: b3a49ff6-46ad-4ee7-b6fe-7e54213dc33e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 81450361252fcdc2d02a9d9e118839db3c5af64c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195492"
---
# <a name="sp_migrate_user_to_contained-transact-sql"></a>sp_migrate_user_to_contained (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Convierte a un usuario de base de datos asignado a un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en un usuario de base de datos independiente con contraseña. En una base de datos independiente, utilice este procedimiento para quitar las dependencias sobre la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] donde se instala la base de datos. **sp_migrate_user_to_contained** separa al usuario del [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Inicio de sesión original, de modo que la configuración como la contraseña y el idioma predeterminado se puedan administrar por separado para la base de datos independiente. **sp_migrate_user_to_contained** puede usarse antes de mover la base de datos independiente a una instancia diferente del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] para eliminar las dependencias de los [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] inicios de sesión de la instancia actual.  
  
> [!NOTE]
> Tenga cuidado al usar **sp_migrate_user_to_contained**, ya que no podrá invertir el efecto. Este procedimiento solo se usa en una base de datos independiente. Para más información, consulte [Contained Databases](../../relational-databases/databases/contained-databases.md).  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_migrate_user_to_contained [ @username = ] N'user' ,   
    [ @rename = ] { N'copy_login_name' | N'keep_name' } ,   
    [ @disablelogin = ] { N'disable_login' | N'do_not_disable_login' }   
```  
  
## <a name="arguments"></a>Argumentos  
 [**@username =** ] **N '**_usuario_*_'_*  
 Nombre de un usuario en la base de datos independiente actual asignado a un inicio de sesión autenticado en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. El valor es de **tipo sysname y su** valor predeterminado es **null**.  
  
 [**@rename =** ] **N '**_copy_login_name_*_'_*  |  **N '**_keep_name_*_'_*  
 Cuando un usuario de base de datos basado en un inicio de sesión tiene un nombre de usuario distinto al nombre de inicio de sesión, utilice *keep_name* para conservar el nombre de usuario de la base de datos durante la migración. Utilice *copy_login_name* para crear el nuevo usuario de base de datos independiente con el nombre del inicio de sesión, en lugar del usuario. Cuando un usuario de la base de datos basado en inicio de sesión tiene el mismo nombre de usuario que el nombre de inicio de sesión, ambas opciones crean el usuario de la base de datos independiente sin cambiar el nombre.  
  
 [**@disablelogin =** ] **N '**_disable_login_*_'_*  |  **N '**_do_not_disable_login_*_'_*  
 *disable_login* deshabilita el inicio de sesión en la base de datos maestra. Para conectarse cuando el inicio de sesión está deshabilitado, la conexión debe proporcionar el nombre de la base de datos independiente como el **catálogo inicial** como parte de la cadena de conexión.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_migrate_user_to_contained** crea el usuario de base de datos independiente con contraseña, independientemente de las propiedades o los permisos del inicio de sesión. Por ejemplo, el procedimiento puede realizarse correctamente si el inicio de sesión está deshabilitado o si al usuario se le deniega el permiso **Connect** para la base de datos.  
  
 **sp_migrate_user_to_contained** tiene las restricciones siguientes.  
  
-   El nombre de usuario no puede existir ya en la base de datos.  
  
-   Los usuarios integrados, por ejemplo dbo y guest, no se pueden convertir.  
  
-   No se puede especificar el usuario en la cláusula **Execute as** de un procedimiento almacenado firmado.  
  
-   El usuario no puede poseer un procedimiento almacenado que incluya la cláusula **Execute as Owner** .  
  
-   no se puede usar **sp_migrate_user_to_contained** en una base de datos del sistema.  
  
## <a name="security"></a>Seguridad  
 Al realizar la migración de los usuarios, tenga el cuidado de no deshabilitar o eliminar todos los inicios de sesión de administrador de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si se eliminan todos los inicios de sesión, consulte [conexión a SQL Server cuando los administradores del sistema están bloqueados](../../database-engine/configure-windows/connect-to-sql-server-when-system-administrators-are-locked-out.md).  
  
 Si el inicio de sesión **BUILTIN\Administrators** está presente, los administradores pueden conectarse iniciando su aplicación mediante la opción **Ejecutar como administrador** .  
  
### <a name="permissions"></a>Permisos  
 Requiere el permiso **CONTROL SERVER** .  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-migrating-a-single-user"></a>A. Realizar la migración de un usuario único  
 En el siguiente ejemplo, se realiza una migración de un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] denominado `Barry` a un usuario de base de datos independiente con contraseña. En el ejemplo no se cambia el nombre de usuario y se conserva el inicio de sesión como habilitado.  
  
```sql  
sp_migrate_user_to_contained   
@username = N'Barry',  
@rename = N'keep_name',  
@disablelogin = N'do_not_disable_login' ;  
  
```  
  
### <a name="b-migrating-all-database-users-with-logins-to-contained-database-users-without-logins"></a>B. Realizar la migración de todos los usuarios de la base de datos con inicios de sesión a usuarios de base de datos independiente sin inicio de sesión  
 En el siguiente ejemplo, se realiza la migración de todos los usuarios basados en inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a usuarios de base de datos independiente con contraseñas. En el ejemplo se excluyen los inicios de sesión que no están habilitados. El ejemplo se debe ejecutar en la base de datos independiente.  
  
```sql  
DECLARE @username sysname ;  
DECLARE user_cursor CURSOR  
    FOR   
        SELECT dp.name   
        FROM sys.database_principals AS dp  
        JOIN sys.server_principals AS sp   
        ON dp.sid = sp.sid  
        WHERE dp.authentication_type = 1 AND sp.is_disabled = 0;  
OPEN user_cursor  
FETCH NEXT FROM user_cursor INTO @username  
    WHILE @@FETCH_STATUS = 0  
    BEGIN  
        EXECUTE sp_migrate_user_to_contained   
        @username = @username,  
        @rename = N'keep_name',  
        @disablelogin = N'disable_login';  
    FETCH NEXT FROM user_cursor INTO @username  
    END  
CLOSE user_cursor ;  
DEALLOCATE user_cursor ;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Migrate to a Partially Contained Database](../../relational-databases/databases/migrate-to-a-partially-contained-database.md)   
 [Bases de datos independientes](../../relational-databases/databases/contained-databases.md)  
  
  
