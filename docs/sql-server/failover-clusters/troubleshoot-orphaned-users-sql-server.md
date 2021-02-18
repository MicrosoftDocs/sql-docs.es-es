---
title: Solución de problemas de usuarios huérfanos
description: Los usuarios huérfanos surgen cuando un inicio de sesión de usuario de base de datos deja de existir en la base de datos maestra. En este tema se describe cómo identificar y resolver usuarios huérfanos.
ms.custom: seo-lt-2019
ms.date: 07/14/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: high-availability
ms.topic: troubleshooting
helpviewer_keywords:
- orphaned users [SQL Server]
- logins [SQL Server], orphaned users
- troubleshooting [SQL Server], user accounts
- user accounts [SQL Server], orphaned users
- failover [SQL Server], managing metadata
- database mirroring [SQL Server], metadata
- users [SQL Server], orphaned
ms.assetid: 11eefa97-a31f-4359-ba5b-e92328224133
author: cawrites
ms.author: chadam
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016'
ms.openlocfilehash: 2583e238295c87b555c43f25b7fd68b03f3647c6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354032"
---
# <a name="troubleshoot-orphaned-users-sql-server"></a>Solución de problemas de usuarios huérfanos (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Los usuarios huérfanos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se producen cuando un usuario de base de datos se basa en un inicio de sesión en la base de datos **maestra** , pero ese inicio de sesión ya no existe en **master**. Esto puede suceder cuando se elimina el inicio de sesión o cuando la base de datos se mueve a otro servidor donde el inicio de sesión no existe. En este tema se describe cómo buscar usuarios huérfanos para reasignarles inicios de sesión.  
  
> [!NOTE]  
>  Para reducir la posibilidad de que se produzcan usuarios huérfanos, use usuarios de bases de datos independientes para aquellas bases de datos susceptibles de moverse. Para obtener más información, vea [Usuarios de base de datos independiente: hacer que la base de datos sea portátil](../../relational-databases/security/contained-database-users-making-your-database-portable.md).  
  
## <a name="background"></a>Información previa  
 Para conectarse a una base de datos en una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con una entidad de seguridad (identidad de usuario de base de datos) basada en un inicio de sesión, la entidad de seguridad debe tener un inicio de sesión válido en la base de datos **maestra** . Este inicio de sesión se usa en el proceso de autenticación, que comprueba la identidad de la entidad de seguridad y averigua si tiene permiso para conectarse a la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Los inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en una instancia de servidor están visibles en la vista de catálogo **sys.server_principals** y en la vista de compatibilidad **sys.sql_logins** .  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tienen acceso a las bases de datos individuales como un "usuario de base de datos" que está asignado al inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Existen tres excepciones a esta regla:  
  
-   Usuarios de bases de datos independientes  
  
     Los usuarios de bases de datos independientes se autentican en el nivel de base de datos de usuario y no están asociados a inicios de sesión. Esto es recomendable porque las bases de datos son más portátiles y los usuarios de las bases de datos independientes no pueden quedarse huérfanos. Sin embargo, deben volver a crearse para cada base de datos, algo muy poco práctico en un entorno con muchas bases de datos.  
  
-   La cuenta **Invitado** .  
  
     Si esta cuenta se habilita en la base de datos, permite que los inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que no están asignados a un usuario de base de datos puedan tener acceso a la base de datos como un usuario **invitado** . La cuenta **Invitado** está deshabilitada de forma predeterminada.  
  
-   La pertenencia a grupos de Microsoft Windows.  
  
     Un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] creado desde un usuario de Windows puede entrar en una base de datos si este usuario es miembro de un grupo de Windows que también sea usuario en la base de datos.  
  
 La información sobre la asignación de un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a un usuario de la base de datos se almacena en la base de datos. Incluye el nombre del usuario de la base de datos y el SID del inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] correspondiente. Los permisos de este usuario de base de datos se usan para otorgar autorizaciones en la base de datos.  
  
 Un usuario de base de datos (basado en un inicio de sesión) cuyo inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] correspondiente está sin definir o se ha definido de forma incorrecta en una instancia de servidor no podrá iniciar una sesión en la instancia. Es lo que se denomina un *usuario huérfano* de la base de datos en esa instancia de servidor. Otra manera de convertirse en huérfano es que el SID de inicio de sesión al que está asignado el usuario de la base de datos no esté presente en la instancia `master` . Un usuario de la base de datos puede convertirse en huérfano si una base de datos se restaura o se conecta a otra instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] donde nunca se ha creado el inicio de sesión. También puede convertirse en huérfano si se quita el inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] correspondiente. Incluso si se vuelve a crear el inicio de sesión, tendrá un SID diferente, por lo que el usuario de la base de datos seguirá siendo huérfano.  
  
## <a name="detect-orphaned-users"></a>Detección de usuarios huérfanos  

**Para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y PDW**

Para detectar usuarios huérfanos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] por no tener inicios de sesión de autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , ejecute la siguiente instrucción en la base de datos de usuario:  
  
```  
SELECT dp.type_desc, dp.SID, dp.name AS user_name  
FROM sys.database_principals AS dp  
LEFT JOIN sys.server_principals AS sp  
    ON dp.SID = sp.SID  
WHERE sp.SID IS NULL  
    AND authentication_type_desc = 'INSTANCE';  
```  
  
 En la salida se enumeran los usuarios de autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y los identificadores de seguridad (SID) correspondientes en la base de datos actual que no están vinculados a ningún inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  

**Para SQL Database y Azure Synapse Analytics**

La tabla `sys.server_principals` no está disponible en SQL Database ni en Azure Synapse Analytics. Identifique los usuarios huérfanos de esos entornos con los siguientes pasos:

1. Conéctese a la base de datos `master` y seleccione los SID para los inicios de sesión con la siguiente consulta:
    ```
    SELECT sid 
    FROM sys.sql_logins 
    WHERE type = 'S'; 
    ```

2. Conéctese a la base de datos de usuario y revise los SID de los usuarios de la tabla `sys.database_principals` mediante la consulta siguiente:

    ```
    SELECT name, sid, principal_id
    FROM sys.database_principals 
    WHERE type = 'S' 
      AND name NOT IN ('guest', 'INFORMATION_SCHEMA', 'sys')
      AND authentication_type_desc = 'INSTANCE';
    ```

3. Compare las dos listas para determinar si hay SID de usuario en la tabla `sys.database_principals` de la base de datos de usuario que no coinciden con los SID de inicio de sesión de la tabla `sql_logins` de la base de datos maestra. 
  
## <a name="resolve-an-orphaned-user"></a>Resolución de un usuario huérfano  
En la base de datos maestra, use la instrucción [CREATE LOGIN](../../t-sql/statements/create-login-transact-sql.md) con la opción SID para volver a crear un inicio de sesión que falte. Para ello, proporcione el `SID` del usuario de base de datos obtenido en la sección anterior:  
  
```  
CREATE LOGIN <login_name>   
WITH PASSWORD = '<use_a_strong_password_here>',  
SID = <SID>;  
```  
  
 Para asignar un usuario huérfano a un inicio de sesión que ya existe en **master**, ejecute la instrucción [ALTER USER](../../t-sql/statements/alter-user-transact-sql.md) en la base de datos de usuario especificando el nombre de inicio de sesión.  
  
```  
ALTER USER <user_name> WITH Login = <login_name>;  
```  
  
 Cuando se vuelve a crear un inicio de sesión que falta, el usuario puede tener acceso a la base de datos con la contraseña proporcionada. Tras esto, el usuario puede modificar la contraseña de la cuenta de inicio de sesión con la instrucción ALTER LOGIN.  
  
```  
ALTER LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>';  
```  
  
> [!IMPORTANT]  
>  Cualquier inicio de sesión puede cambiar su propia contraseña. Solo los inicios de sesión con el permiso `ALTER ANY LOGIN` pueden cambiar la contraseña de inicio de sesión de otro usuario. Sin embargo, solo los miembros del rol **sysadmin** pueden modificar las contraseñas de los miembros del rol **sysadmin** .  
  
## <a name="see-also"></a>Consulte también  
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [ALTER USER &#40;Transact-SQL&#41;](../../t-sql/statements/alter-user-transact-sql.md)   
 [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)   
 [sys.database_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)   
 [sys.server_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)   
 [sp_change_users_login &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-change-users-login-transact-sql.md)   
 [sp_addlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlogin-transact-sql.md)   
 [sp_grantlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-grantlogin-transact-sql.md)   
 [sp_password &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-password-transact-sql.md)   
 [sys.sysusers &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-sysusers-transact-sql.md)   
 [sys.sql_logins](../../relational-databases/system-catalog-views/sys-sql-logins-transact-sql.md) [sys.syslogins &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-syslogins-transact-sql.md)  
  
  
