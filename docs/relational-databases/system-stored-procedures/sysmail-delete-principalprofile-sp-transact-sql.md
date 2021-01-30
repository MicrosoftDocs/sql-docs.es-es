---
description: sysmail_delete_principalprofile_sp (Transact-SQL)
title: sysmail_delete_principalprofile_sp (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_delete_principalprofile_sp_TSQL
- sysmail_delete_principalprofile_sp
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_delete_principalprofile_sp
ms.assetid: 8fc14700-e17a-4073-9a96-7fc23e775c69
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a80485aca8c7f0adb6a539f57540952ccd758a31
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182018"
---
# <a name="sysmail_delete_principalprofile_sp-transact-sql"></a>sysmail_delete_principalprofile_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Quita el permiso de un usuario o un rol de base de datos para usar un perfil público o privado de Correo electrónico de base de datos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sysmail_delete_principalprofile_sp  { [ @principal_id = ] principal_id | [ @principal_name = ] 'principal_name' } ,  
    { [ @profile_id = ] profile_id | [ @profile_name = ] 'profile_name' }  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @principal_id = ] principal_id` Es el identificador del usuario o el rol de la base de datos **msdb** de la asociación que se va a eliminar. *principal_id* es de **tipo int** y su valor predeterminado es NULL. Para convertir un perfil público en un perfil privado, proporcione el identificador de entidad de seguridad **0** o el nombre de entidad de seguridad **' Public '**. Se debe especificar *principal_id* o *principal_name* .  
  
`[ @principal_name = ] 'principal_name'` Es el nombre del usuario o el rol de la base de datos **msdb** de la asociación que se va a eliminar. *principal_name* es de **tipo sysname y su** valor predeterminado es NULL. Para convertir un perfil público en un perfil privado, proporcione el identificador de entidad de seguridad **0** o el nombre de entidad de seguridad **' Public '**. Se debe especificar *principal_id* o *principal_name* .  
  
`[ @profile_id = ] profile_id` Es el identificador del perfil para la asociación que se va a eliminar. *profile_id* es de **tipo int** y su valor predeterminado es NULL. Se debe especificar *profile_id* o *profile_name* .  
  
`[ @profile_name = ] 'profile_name'` Es el nombre del perfil para la asociación que se va a eliminar. *profile_name* es de **tipo sysname y su** valor predeterminado es NULL. Se debe especificar *profile_id* o *profile_name* .  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 Para convertir un perfil público en un perfil privado, proporcione **' Public '** como nombre principal o **0** para el ID. de entidad de seguridad.  
  
 Tenga cuidado al quitar permisos para el perfil privado predeterminado de un usuario o el perfil público predeterminado. Cuando no hay ningún perfil predeterminado disponible, **sp_send_dbmail** requiere el nombre de un perfil como argumento. Por lo tanto, si se quita un perfil predeterminado, pueden producirse errores en las llamadas a **sp_send_dbmail** . Para obtener más información, vea [sp_send_dbmail &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-send-dbmail-transact-sql.md).  
  
 El procedimiento almacenado **sysmail_delete_principalprofile_sp** está en la base de datos **msdb** y pertenece al esquema **DBO** . El procedimiento se debe ejecutar con un nombre de tres partes si la base de datos actual no es **msdb**.  
  
## <a name="permissions"></a>Permisos  
 Los permisos de ejecución para este procedimiento tienen como valor predeterminado los miembros del rol fijo de servidor **sysadmin** .  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se muestra cómo eliminar la asociación entre el perfil de **Administrador de AdventureWorks** y el inicio de sesión **ApplicationUser** en la base de datos **msdb** .  
  
```  
EXECUTE msdb.dbo.sysmail_delete_principalprofile_sp  
    @principal_name = 'ApplicationUser',  
    @profile_name = 'AdventureWorks Administrator' ;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Correo electrónico de base de datos](../../relational-databases/database-mail/database-mail.md)   
 [Correo electrónico de base de datos objetos de configuración](../../relational-databases/database-mail/database-mail-configuration-objects.md)   
 [Correo electrónico de base de datos procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
