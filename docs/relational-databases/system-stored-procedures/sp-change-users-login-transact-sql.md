---
description: sp_change_users_login (Transact-SQL)
title: sp_change_users_login (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/13/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_change_users_login
- sp_change_users_login_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_change_users_login
ms.assetid: 1554b39f-274b-4ef8-898e-9e246b474333
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 109b2a6aa015f8f66a327714264aa986060f9a0c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203713"
---
# <a name="sp_change_users_login-transact-sql"></a>sp_change_users_login (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Asigna un usuario de base de datos existente a un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
  
 > [!IMPORTANT]
 > [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilice [ALTER User](../../t-sql/statements/alter-user-transact-sql.md) en su lugar.  
  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_change_users_login [ @Action = ] 'action'   
    [ , [ @UserNamePattern = ] 'user' ]   
    [ , [ @LoginName = ] 'login' ]   
    [ , [ @Password = ] 'password' ]  
[;]  
```  
  
## <a name="arguments"></a>Argumentos  
 [ @Action =] '*acción*'  
 Describe la acción que llevará a cabo el procedimiento. la *acción* es **VARCHAR (10)**. la *acción* puede tener uno de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**Auto_Fix**|Vincula una entrada de usuario de la vista de catálogo del sistema sys.database_principals de la base de datos actual con el inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] del mismo nombre. Si no existe un inicio de sesión con el mismo nombre, se creará uno. Examine el resultado de la instrucción **Auto_Fix** para confirmar que se ha realizado el vínculo correcto. Evite usar **Auto_Fix** en situaciones de seguridad.<br /><br /> Al usar **Auto_Fix**, debe especificar el *usuario* y la *contraseña* si el inicio de sesión aún no existe; de lo contrario, debe especificar *usuario* pero se omitirá la *contraseña* . *login* debe ser null. el *usuario* debe ser un usuario válido en la base de datos actual. El inicio de sesión no puede tener otro usuario asignado.|  
|**Report**|Enumera los usuarios y sus identificadores de seguridad (SID) correspondientes, que se encuentran en la base de datos actual y no están vinculados con ningún inicio de sesión. el *usuario*, el *Inicio de sesión* y la *contraseña* deben ser null o no especificarse.<br /><br /> Para reemplazar la opción de informe por una consulta mediante las tablas del sistema, compare las entradas de **Sys.server_prinicpals** con las entradas de **Sys.database_principals**.|  
|**Update_One**|Vincula el *usuario* especificado en la base de datos actual a un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *Inicio de sesión* existente. se debe especificar el *usuario* y el *Inicio de sesión* . la *contraseña* debe ser null o no especificarse.|  
  
 [ @UserNamePattern =] '*usuario*'  
 Es el nombre de un usuario en la base de datos actual. *User* es de **tipo sysname y su** valor predeterminado es NULL.  
  
 [ @LoginName =] '*Inicio de sesión*'  
 Es el nombre de un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. *login* es de tipo **sysname** y su valor predeterminado es NULL.  
  
 [ @Password =] '*contraseña*'  
 Es la contraseña asignada a un nuevo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Inicio de sesión que se crea especificando **Auto_Fix**. Si ya existe un inicio de sesión coincidente, se asignan el usuario y el inicio de sesión y se omite la *contraseña* . Si no existe un inicio de sesión coincidente, sp_change_users_login crea un nuevo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Inicio de sesión y asigna la *contraseña* como contraseña para el nuevo inicio de sesión. *password* es de **tipo sysname** y no debe ser null.  
  
> **IMPORTANTE:** Use siempre una [contraseña segura.](../../relational-databases/security/strong-passwords.md)
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|UserName|**sysname**|Nombre del usuario de la base de datos.|  
|UserSID|**varbinary(85)**|Identificador de seguridad del usuario.|  
  
## <a name="remarks"></a>Observaciones  
 Use sp_change_users_login para vincular un usuario de la base de datos actual a un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si ha cambiado el inicio de sesión para el usuario, utilice sp_change_users_login para vincular el usuario al nuevo inicio de sesión sin que se pierdan los permisos del usuario. El nuevo *Inicio de sesión* no puede ser SA y el *usuario* no puede ser DBO, guest o un usuario INFORMATION_SCHEMA.  
  
 sp_change_users_login no se puede utilizar para asignar usuarios de la base de datos a entidades de seguridad de Windows, certificados o claves asimétricas.  
  
 sp_change_users_login no se puede utilizar con un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] creado desde una entidad de seguridad de Windows ni con un usuario creado mediante CREATE USER WITHOUT LOGIN.  
  
 No se puede ejecutar sp_change_users_login dentro de una transacción definida por el usuario.  
  
## <a name="permissions"></a>Permisos  
 Requiere pertenencia al rol fijo de base de datos db_owner. Solo los miembros del rol fijo de servidor sysadmin pueden especificar la opción **Auto_Fix** .  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-showing-a-report-of-the-current-user-to-login-mappings"></a>A. Mostrar un informe del usuario actual para las asignaciones de inicio de sesión  
 El ejemplo siguiente produce un informe de los usuarios en la actual base de datos y sus identificadores de seguridad (SID).  
  
```  
EXEC sp_change_users_login 'Report';  
```  
  
### <a name="b-mapping-a-database-user-to-a-new-sql-server-login"></a>B. Asignar un usuario de la base de datos a un nuevo inicio de sesión de SQL Server  
 En el siguiente ejemplo se asocia un usuario de la base de datos con un nuevo inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. El usuario de la base de datos `MB-Sales`, que primero se asigna a otro inicio de sesión, se vuelve a asignar al inicio de sesión `MaryB`.  
  
```  
--Create the new login.  
CREATE LOGIN MaryB WITH PASSWORD = '982734snfdHHkjj3';  
GO  
--Map database user MB-Sales to login MaryB.  
USE AdventureWorks2012;  
GO  
EXEC sp_change_users_login 'Update_One', 'MB-Sales', 'MaryB';  
GO  
```  
  
### <a name="c-automatically-mapping-a-user-to-a-login-creating-a-new-login-if-it-is-required"></a>C. Asignar automáticamente un usuario a un inicio de sesión, creando un inicio de sesión nuevo si es necesario  
 En el siguiente ejemplo se muestra cómo utilizar `Auto_Fix` para asignar una usuario existente a un inicio de sesión con el mismo nombre, o crear el inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]`Mary` que tiene la contraseña `B3r12-3x$098f6` si no existe el inicio de sesión `Mary`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_change_users_login 'Auto_Fix', 'Mary', NULL, 'B3r12-3x$098f6';  
GO  
```  
  
## <a name="see-also"></a>Vea también  
 [Procedimientos almacenados de seguridad &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [sp_adduser &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adduser-transact-sql.md)   
 [sp_helplogins &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helplogins-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [sys.database_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)  
  
  
