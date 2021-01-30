---
description: sysmail_add_principalprofile_sp (Transact-SQL)
title: sysmail_add_principalprofile_sp (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_add_principalprofile_sp_TSQL
- sysmail_add_principalprofile_sp
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_add_principalprofile_sp
ms.assetid: b2a0b313-abb9-4c23-8511-db77ca8172b3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0252e299f072144999e3c2db1b2404de9aa9b6a8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183038"
---
# <a name="sysmail_add_principalprofile_sp-transact-sql"></a>sysmail_add_principalprofile_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Concede el permiso para que una entidad de seguridad de base de datos msdb use un perfil de Correo electrónico de base de datos. La entidad de seguridad de base de datos debe asignarse a un usuario de autenticación de SQL Server, un usuario de Windows o un grupo de Windows.
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sysmail_add_principalprofile_sp  { [ @principal_id = ] principal_id | [ @principal_name = ] 'principal_name' } ,  
    { [ @profile_id = ] profile_id | [ @profile_name = ] 'profile_name' }  
    [ , [ @is_default ] = 'is_default' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @principal_id = ] principal_id` IDENTIFICADOR del usuario o el rol de la base de datos **msdb** de la asociación. *principal_id* es de **tipo int** y su valor predeterminado es NULL. Se debe especificar *principal_id* o *principal_name* . Un *principal_id* de **0** convierte este perfil en un perfil público, lo que concede acceso a todas las entidades de seguridad de la base de datos.  
  
`[ @principal_name = ] 'principal_name'` Nombre del usuario o el rol de la base de datos **msdb** de la asociación. *principal_name* es de **tipo sysname y su** valor predeterminado es NULL. Se debe especificar *principal_id* o *principal_name* . Un *principal_name* de **' Public '** convierte este perfil en un perfil público, lo que concede acceso a todas las entidades de seguridad de la base de datos.  
  
`[ @profile_id = ] profile_id` Identificador del perfil para la asociación. *profile_id* es de **tipo int** y su valor predeterminado es NULL. Se debe especificar *profile_id* o *profile_name* .  
  
`[ @profile_name = ] 'profile_name'` Nombre del perfil para la asociación. *profile_name* es de **tipo sysname** y no tiene ningún valor predeterminado. Se debe especificar *profile_id* o *profile_name* .  
  
`[ @is_default = ] is_default` Especifica si este perfil es el perfil predeterminado para la entidad de seguridad. Una entidad de seguridad debe tener solo un perfil predeterminado. *is_default* es de **bits** y no tiene ningún valor predeterminado.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 Para que un perfil sea público, especifique un **\@ principal_id** de **0** o un **\@ principal_name** de **Public**. Un perfil público está disponible para todos los usuarios de la base de datos **msdb** , aunque los usuarios también deben ser miembros de **DatabaseMailUserRole** para ejecutar **sp_send_dbmail**.  
  
 El usuario de la base de datos solo puede tener un perfil predeterminado. Cuando **\@ is_default** es '**1**' y el usuario ya está asociado a uno o más perfiles, el perfil especificado se convierte en el perfil predeterminado para el usuario. El perfil predeterminado anterior sigue estando asociado con el usuario, pero ya no es el perfil predeterminado.  
  
 Cuando **\@ is_default** es '**0**' y no existe ninguna otra asociación, el procedimiento almacenado devuelve un error.  
  
 El procedimiento almacenado **sysmail_add_principalprofile_sp** está en la base de datos **msdb** y pertenece al esquema **DBO** . El procedimiento se debe ejecutar con un nombre de tres partes si la base de datos actual no es **msdb**.  
  
## <a name="permissions"></a>Permisos  
 Los permisos de ejecución para este procedimiento tienen como valor predeterminado los miembros del rol fijo de servidor **sysadmin** .  
  
## <a name="examples"></a>Ejemplos  
 **A. Crear una asociación y configurar el perfil predeterminado**  
  
 En el ejemplo siguiente se crea una asociación entre el perfil denominado `AdventureWorks Administrator Profile` y el usuario de base de datos **msdb** `ApplicationUser` . El perfil es el predeterminado del usuario.  
  
```  
EXECUTE msdb.dbo.sysmail_add_principalprofile_sp  
    @principal_name = 'ApplicationUser',  
    @profile_name = 'AdventureWorks Administrator Profile',  
    @is_default = 1 ;  
```  
  
 **B. Convertir un perfil en el perfil público predeterminado**  
  
 En el ejemplo siguiente se convierte el perfil `AdventureWorks Public Profile` en el perfil público predeterminado para los usuarios de la base de datos **msdb** .  
  
```  
EXECUTE msdb.dbo.sysmail_add_principalprofile_sp  
    @principal_name = 'public',  
    @profile_name = 'AdventureWorks Public Profile',  
    @is_default = 1 ;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Correo electrónico de base de datos](../../relational-databases/database-mail/database-mail.md)   
 [Correo electrónico de base de datos objetos de configuración](../../relational-databases/database-mail/database-mail-configuration-objects.md)   
 [Correo electrónico de base de datos procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
