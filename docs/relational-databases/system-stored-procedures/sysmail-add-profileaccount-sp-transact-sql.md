---
description: sysmail_add_profileaccount_sp (Transact-SQL)
title: sysmail_add_profileaccount_sp (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_add_profileaccount_sp
- sysmail_add_profileaccount_sp_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_add_profileaccount_sp
ms.assetid: 7cbf430f-1997-45ea-9707-0086184de744
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c01a72073b36aeba37cf28b5be8175fc283a07df
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188771"
---
# <a name="sysmail_add_profileaccount_sp-transact-sql"></a>sysmail_add_profileaccount_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Agrega una cuenta del Correo electrónico de base de datos al perfil del Correo electrónico de base de datos. Ejecute **sysmail_add_profileaccount_sp** después de crear una cuenta de base de datos con [sysmail_add_account_sp &#40;&#41;de Transact-SQL](../../relational-databases/system-stored-procedures/sysmail-add-account-sp-transact-sql.md)y se crea un perfil de base de datos con sysmail_add_profile_sp &#40;[de Transact-SQL](../../relational-databases/system-stored-procedures/sysmail-add-profile-sp-transact-sql.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sysmail_add_profileaccount_sp { [ @profile_id = ] profile_id | [ @profile_name = ] 'profile_name' } ,  
    { [ @account_id = ] account_id | [ @account_name = ] 'account_name' }  
    [ , [ @sequence_number = ] sequence_number ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @profile_id = ] profile_id` Identificador de perfil al que se va a agregar la cuenta. *profile_id* es de **tipo int** y su valor predeterminado es NULL. Se debe especificar el *profile_id* o el *profile_name* .  
  
`[ @profile_name = ] 'profile_name'` Nombre del perfil al que se va a agregar la cuenta. *profile_name* es de **tipo sysname y su** valor predeterminado es NULL. Se debe especificar el *profile_id* o el *profile_name* .  
  
`[ @account_id = ] account_id` Identificador de cuenta que se va a agregar al perfil. *ACCOUNT_ID* es de **tipo int** y su valor predeterminado es NULL. Se debe especificar el *ACCOUNT_ID* o el *account_name* .  
  
`[ @account_name = ] 'account_name'` Nombre de la cuenta que se va a agregar al perfil. *account_name* es de **tipo sysname y su** valor predeterminado es NULL. Se debe especificar el *ACCOUNT_ID* o el *account_name* .  
  
`[ @sequence_number = ] sequence_number` El número de secuencia de la cuenta en el perfil. *sequence_number* es de **tipo int** y no tiene ningún valor predeterminado. El número de secuencia determina el orden en que las cuentas se utilizan en el perfil.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 El perfil y la cuenta ya deben existir. En caso contrario, el procedimiento almacenado devuelve un error.  
  
 Tenga en cuenta que este procedimiento almacenado no cambia el número de secuencia de una cuenta asociada al perfil especificado. Para obtener más información sobre cómo actualizar el número de secuencia de una cuenta, vea [sysmail_update_profileaccount_sp &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sysmail-update-profileaccount-sp-transact-sql.md).  
  
 El número de secuencia determina el orden en que el Correo electrónico de base de datos utiliza las cuentas en el perfil. En el caso de un mensaje de correo electrónico nuevo, el Correo electrónico de base de datos se inicia con la cuenta con el número de secuencia más bajo. Si la cuenta genera un error, el Correo electrónico de base de datos utiliza la cuenta con el siguiente número de secuencia superior y así sucesivamente hasta que el Correo electrónico de base de datos envía el mensaje correctamente o la cuenta con el número de secuencia superior genera un error. Si la cuenta con el número de secuencia superior genera un error, el Correo electrónico de base de datos pausa los intentos de envío del correo electrónico durante la cantidad de tiempo configurada en el parámetro *AccountRetryDelay* de **sysmail_configure_sp** y, después, inicia el proceso de nuevo intento de envío del correo electrónico comenzando por el número de secuencia más bajo. Use el parámetro *AccountRetryAttempts* de **sysmail_configure_sp** para configurar el número de veces que el proceso de correo electrónico externo intenta enviar el mensaje de correo electrónico con cada cuenta del perfil especificado.  
  
 Si existe más de una cuenta con el mismo número de secuencia, Correo electrónico de base de datos solo usará una de esas cuentas para un mensaje de correo electrónico determinado. En este caso, el Correo electrónico de base de datos no confirma qué cuenta se va a usar para el número de secuencia o que se vaya a usar la misma cuenta de un mensaje a otro.  
  
 El procedimiento almacenado **sysmail_add_profileaccount_sp** está en la base de datos **msdb** y pertenece al esquema **DBO** . El procedimiento se debe ejecutar con un nombre de tres partes si la base de datos actual no es **msdb**.  
  
## <a name="permissions"></a>Permisos  
 Los permisos de ejecución para este procedimiento tienen como valor predeterminado los miembros del rol fijo de servidor **sysadmin** .  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se asocia el perfil `AdventureWorks Administrator` a la cuenta `Audit Account`. La cuenta de auditoría tiene el número de secuencia 1.  
  
```  
EXECUTE msdb.dbo.sysmail_add_profileaccount_sp  
    @profile_name = 'AdventureWorks Administrator',  
    @account_name = 'Audit Account',  
    @sequence_number = 1 ;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Correo electrónico de base de datos](../../relational-databases/database-mail/database-mail.md)   
 [Creación de una cuenta de Correo electrónico de base de datos](../../relational-databases/database-mail/create-a-database-mail-account.md)   
 [Correo electrónico de base de datos objetos de configuración](../../relational-databases/database-mail/database-mail-configuration-objects.md)   
 [Correo electrónico de base de datos procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
