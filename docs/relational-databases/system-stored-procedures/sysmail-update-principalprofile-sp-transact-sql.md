---
description: sysmail_update_principalprofile_sp (Transact-SQL)
title: sysmail_update_principalprofile_sp (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_update_principalprofile_sp
- sysmail_update_principalprofile_sp_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_update_principalprofile_sp
ms.assetid: 9fe96e9a-4758-4e4a-baee-3e1217c4426c
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 85bab8892a9459ee5de954596ffea2be2a5aed92
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181864"
---
# <a name="sysmail_update_principalprofile_sp-transact-sql"></a>sysmail_update_principalprofile_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Actualiza la información para una asociación entre una entidad de seguridad y un perfil.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sysmail_update_principalprofile_sp { @principal_id = principal_id | @principal_name = 'principal_name' } ,  
    { [ @profile_id = ] profile_id | [ @profile_name = ] 'profile_name' } ,  
    [ @is_default = ] 'is_default'  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @principal_id = ] principal_id` IDENTIFICADOR del usuario o el rol de la base de datos **msdb** de la asociación que se va a cambiar. *principal_id* es de **tipo int** y su valor predeterminado es NULL. Se debe especificar *principal_id* o *principal_name* .  
  
`[ @principal_name = ] 'principal_name'` Nombre del usuario o el rol de la base de datos **msdb** de la asociación que se va a actualizar. *principal_name* es de **tipo sysname y su** valor predeterminado es NULL. Se puede especificar *principal_id* o *principal_name* .  
  
`[ @profile_id = ] profile_id` Identificador del perfil para la asociación que se va a cambiar. *profile_id* es de **tipo int** y su valor predeterminado es NULL. Se debe especificar *profile_id* o *profile_name* .  
  
`[ @profile_name = ] 'profile_name'` Nombre del perfil para la asociación que se va a cambiar. *profile_name* es de **tipo sysname y su** valor predeterminado es NULL. Se debe especificar *profile_id* o *profile_name* .  
  
`[ @is_default = ] 'is_default'` Indica si este perfil es el perfil predeterminado para el usuario de base de datos. El usuario de la base de datos solo puede tener un perfil predeterminado. *is_default* es de **bits** y no tiene ningún valor predeterminado.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="remarks"></a>Observaciones  
 Este procedimiento almacenado cambia si el perfil especificado es el perfil predeterminado para el usuario de la base de datos. El usuario de la base de datos solo puede tener un perfil privado predeterminado.  
  
 Cuando el nombre de la entidad de seguridad de la asociación es **público** o el ID. de la entidad de seguridad de la asociación es **0**, este procedimiento almacenado cambia el perfil público. Solo puede haber un perfil público predeterminado.  
  
 Cuando **\@ is_default** es '**1**' y la entidad de seguridad está asociada a más de un perfil, el perfil especificado se convierte en el perfil predeterminado para la entidad de seguridad. El perfil predeterminado anterior sigue estando asociado a la entidad de seguridad, pero ya no es el perfil predeterminado.  
  
 El procedimiento almacenado **sysmail_update_principalprofile_sp** está en la base de datos **msdb** y pertenece al esquema **DBO** . El procedimiento se debe ejecutar con un nombre de tres partes si la base de datos actual no es **msdb**.  
  
## <a name="permissions"></a>Permisos  
 Los permisos de ejecución para este procedimiento tienen como valor predeterminado los miembros del rol fijo de servidor **sysadmin** .  
  
## <a name="examples"></a>Ejemplos  
 **A. Configurar un perfil como perfil público predeterminado para una base de datos**  
  
 En el ejemplo siguiente se establece el perfil `General Use Profile` como perfil público predeterminado para los usuarios de la base de datos **msdb** .  
  
```  
EXECUTE msdb.dbo.sysmail_update_principalprofile_sp  
    @principal_name = 'public',  
    @profile_name = 'General Use Profile',  
    @is_default = '1';  
```  
  
 **B. Configurar un perfil como perfil privado predeterminado para un usuario**  
  
 En el ejemplo siguiente se establece el perfil `AdventureWorks Administrator` para que sea el perfil predeterminado para la entidad `ApplicationUser` de seguridad en la base de datos **msdb** . El perfil ya debe estar asociado a la entidad de seguridad. El perfil predeterminado anterior sigue estando asociado a la entidad de seguridad, pero ya no es el perfil predeterminado.  
  
```  
EXECUTE msdb.dbo.sysmail_update_principalprofile_sp  
    @principal_name = 'ApplicationUser',  
    @profile_name = 'AdventureWorks Administrator',  
    @is_default = '1' ;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Correo electrónico de base de datos](../../relational-databases/database-mail/database-mail.md)   
 [Correo electrónico de base de datos objetos de configuración](../../relational-databases/database-mail/database-mail-configuration-objects.md)   
 [Correo electrónico de base de datos procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
