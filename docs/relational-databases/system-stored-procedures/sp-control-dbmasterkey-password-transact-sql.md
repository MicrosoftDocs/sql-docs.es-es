---
description: sp_control_dbmasterkey_password (Transact-SQL)
title: sp_control_dbmasterkey_password (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/09/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_control_dbmasterkey_password
- sp_control_dbmasterkey_password_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_control_dbmasterkey_password
ms.assetid: 63979a87-42a2-446e-8e43-30481faaf3ca
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 06de3460a55eecba6c1525576caec85d6baa905f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190034"
---
# <a name="sp_control_dbmasterkey_password-transact-sql"></a>sp_control_dbmasterkey_password (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Agrega o quita una credencial que contiene la contraseña necesaria para abrir la clave maestra de una base de datos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_control_dbmasterkey_password @db_name = 'database_name,  
     @password = 'master_key_password' , @action = { 'add' | 'drop' }  
```  
  
## <a name="arguments"></a>Argumentos  
 @db_name= N '*database_name*'  
 Especifica el nombre de la base de datos asociada a esta credencial. No puede ser una base de datos del sistema. *database_name* es **nvarchar**.  
  
 @password= N '*contraseña*'  
 Especifica la contraseña de la clave maestra. la *contraseña* es **nvarchar**.  
  
 @action= N'add '  
 Especifica que se agregará al almacén de credenciales una credencial para la base de datos especificada. La credencial contendrá la contraseña de la clave maestra de la base de datos. El valor que se pasa a @action es **nvarchar**.  
  
 @action= N'drop '  
 Especifica que se quitará del almacén de credenciales una credencial para la base de datos especificada. El valor que se pasa a @action es **nvarchar**.  
  
## <a name="remarks"></a>Observaciones  
 Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] necesita la clave maestra de una base de datos para cifrar o descifrar una clave, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] intentará descifrar la clave maestra de la base de datos con la clave maestra de servicio de la instancia. Si el descifrado produce errores, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] buscará en el almacén de credenciales las credenciales de clave maestra con el mismo GUID de familia que la base de datos para la que se necesita la clave maestra. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] intentará después descifrar la clave maestra de la base de datos con cada credencial coincidente hasta que el descifrado se realice correctamente o no queden más credenciales.  
  
> [!CAUTION]  
>  No cree una credencial de clave maestra para una base de datos que deba estar inaccesible para sa y otras entidades de seguridad de servidor con numerosos privilegios. Puede configurar una base de datos de forma que su jerarquía de claves no pueda descifrarse con la clave maestra de servicio. Esta opción se admite como una defensa para bases de datos que contienen información cifrada que no debe estar accesible para sa u otras entidades de seguridad de servidor con amplios privilegios. Al crear una credencial de clave maestra para esta base de datos se quita la defensa, por lo que sa y otras entidades de seguridad de servidor con numerosos privilegios podrán descifrar la base de datos.  
  
 Las credenciales que se crean mediante sp_control_dbmasterkey_password están visibles en la vista de catálogo de [Sys.master_key_passwords](../../relational-databases/system-catalog-views/sys-master-key-passwords-transact-sql.md) . Los nombres de las credenciales creadas para las claves maestras de base de datos presentan el siguiente formato: `##DBMKEY_<database_family_guid>_<random_password_guid>##`. La contraseña se almacena como el secreto de la credencial. Para cada contraseña que se agrega al almacén de credenciales hay una fila en sys.credentials.  
  
 No puede utilizar sp_control_dbmasterkey_password para crear una credencial para las siguientes bases de datos del sistema: maestra, model, msdb, o tempdb.  
  
 sp_control_dbmasterkey_password no comprueba si la contraseña puede abrir la clave maestra de la base de datos especificada.  
  
 Si especifica una contraseña ya almacenada en una credencial para la base de datos especificada, sp_control_dbmasterkey_password registrará errores.  
  
> [!NOTE]  
>  Dos bases de datos de diferentes instancias de servidor pueden compartir el mismo GUID de familia. Si esto ocurriera, las bases de datos compartirían los mismos registros de clave maestra en el almacén de credenciales.  
  
 Los parámetros que se pasan a sp_control_dbmasterkey_password no se muestran en seguimientos.  
  
> [!NOTE]  
>  Cuando utiliza la credencial que se agregó mediante sp_control_dbmasterkey_password para abrir la clave maestra de la base de datos, la clave maestra de servicio vuelve a cifrar la clave maestra de la base de datos. Si la base de datos está en modo de solo lectura, se producirá un error en la operación de recifrado y la clave maestra de la base de datos permanecerá sin cifrar. Para el acceso subsiguiente a la clave maestra de la base de datos debe utilizar la instrucción OPEN MASTER KEY y una contraseña. Para evitar el uso de una contraseña, cree la credencial antes de pasar la base de datos al modo de solo lectura.  
  
 **Posible problema de compatibilidad con versiones anteriores:** Actualmente, el procedimiento almacenado no comprueba si existe una clave maestra. Esto se admite por cuestiones de compatibilidad con versiones anteriores, pero muestra una advertencia. Este comportamiento se ha desaprobado. En una versión futura, la clave maestra debe existir y la contraseña utilizada en el procedimiento almacenado **sp_control_dbmasterkey_password** debe ser la misma contraseña que una de las contraseñas utilizadas para cifrar la clave maestra de la base de datos.  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol fijo de servidor **sysadmin** .  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-creating-a-credential-for-the-adventureworks2012-master-key"></a>A. Crear una credencial para la clave maestra de AdventureWorks2012  
 En el siguiente ejemplo se crea una credencial para la clave maestra de la base de datos `AdventureWorks2012` y se guarda la contraseña de la clave maestra como secreto en la credencial. Dado que todos los parámetros que se pasan a `sp_control_dbmasterkey_password` deben ser del tipo de datos **nvarchar**, las cadenas de texto se convierten con el operador de conversión `N` .  
  
```  
EXEC sp_control_dbmasterkey_password @db_name = N'AdventureWorks2012',   
    @password = N'sdfjlkj#mM00sdfdsf98093258jJlfdk4', @action = N'add';  
GO  
```  
  
### <a name="b-dropping-a-credential-for-a-database-master-key"></a>B. Quitar una credencial para la clave maestra de una base de datos  
 En el siguiente ejemplo se quita la credencial creada en el ejemplo A. Tenga en cuenta que son necesarios todos los parámetros, incluida la contraseña.  
  
```  
EXEC sp_control_dbmasterkey_password @db_name = N'AdventureWorks2012',   
    @password = N'sdfjlkj#mM00sdfdsf98093258jJlfdk4', @action = N'drop';  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Establecer una base de datos reflejada cifrada](../../database-engine/database-mirroring/set-up-an-encrypted-mirror-database.md)   
 [Procedimientos almacenados de seguridad &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [sys.credentials &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-credentials-transact-sql.md)   
 [Credenciales &#40;motor de base de datos&#41;](../../relational-databases/security/authentication-access/credentials-database-engine.md)  
  
  
