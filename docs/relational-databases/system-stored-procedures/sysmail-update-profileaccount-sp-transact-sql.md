---
description: sysmail_update_profileaccount_sp (Transact-SQL)
title: sysmail_update_profileaccount_sp (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_update_profileaccount_sp_TSQL
- sysmail_update_profileaccount_sp
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_update_profileaccount_sp
ms.assetid: 92ca7488-29db-414e-8e36-08b0a8f542bb
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 140256e73adee3593c620b17f81ac10c163030f3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181847"
---
# <a name="sysmail_update_profileaccount_sp-transact-sql"></a>sysmail_update_profileaccount_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Actualiza el número de secuencia de una cuenta de un perfil del Correo electrónico de base de datos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sysmail_update_profileaccount_sp  { [ @profile_id = ] profile_id   
| [ @profile_name = ] 'profile_name' } ,  
    { [ @account_id = ] account_id | [ @account_name = ] 'account_name' } ,  
    [ @sequence_number = ] sequence_number  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @profile_id = ] profile_id` IDENTIFICADOR del perfil que se va a actualizar. *profile_id* es de **tipo int** y su valor predeterminado es NULL. Se debe especificar el *profile_id* o el *profile_name* .  
  
`[ @profile_name = ] 'profile_name'` Nombre del perfil que se va a actualizar. *profile_name* es de **tipo sysname y su** valor predeterminado es NULL. Se debe especificar el *profile_id* o el *profile_name* .  
  
`[ @account_id = ] account_id` IDENTIFICADOR de la cuenta que se va a actualizar. *ACCOUNT_ID* es de **tipo int** y su valor predeterminado es NULL. Se debe especificar el *ACCOUNT_ID* o el *account_name* .  
  
`[ @account_name = ] 'account_name'` Nombre de la cuenta que se va a actualizar. *account_name* es de **tipo sysname y su** valor predeterminado es NULL. Se debe especificar el *ACCOUNT_ID* o el *account_name* .  
  
`[ @sequence_number = ] sequence_number` El nuevo número de secuencia de la cuenta. *sequence_number* es de **tipo int** y no tiene ningún valor predeterminado. El número de secuencia determina el orden en que las cuentas se utilizan en el perfil.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="remarks"></a>Observaciones  
 Devuelve un error si la cuenta especificada no está asociada al perfil especificado.  
  
 El número de secuencia determina el orden en que el Correo electrónico de base de datos utiliza las cuentas en el perfil. En el caso de un mensaje de correo electrónico nuevo, el Correo electrónico de base de datos se inicia con la cuenta con el número de secuencia más bajo. Si la cuenta genera un error, el Correo electrónico de base de datos utiliza la cuenta con el siguiente número de secuencia superior y así sucesivamente hasta que el Correo electrónico de base de datos envía el mensaje correctamente o la cuenta con el número de secuencia superior genera un error. Si la cuenta con el número de secuencia superior genera un error, el mensaje de correo electrónico también genera un error.  
  
 Si hay más de una cuenta con el mismo número de secuencia, el Correo electrónico de base de datos solo utiliza una de estas cuentas para un mensaje de correo electrónico determinado. En este caso, el Correo electrónico de base de datos no confirma qué cuenta se va a usar para el número de secuencia o que se vaya a usar la misma cuenta de un mensaje a otro.  
  
 El procedimiento almacenado **sysmail_update_profileaccount_sp** está en la base de datos **msdb** y pertenece al esquema **DBO** . El procedimiento se debe ejecutar con un nombre de tres partes si la base de datos actual no es **msdb**.  
  
## <a name="permissions"></a>Permisos  
 Los permisos de ejecución para este procedimiento tienen como valor predeterminado los miembros del rol fijo de servidor **sysadmin** .  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se cambia el número de secuencia de la cuenta en el `Admin-BackupServer` perfil `AdventureWorks Administrator` en la base de datos **msdb** . Tras ejecutar este código, el número de secuencia para la cuenta es `3`, lo que indica que se va a probar si las dos primeras cuentas generan un error.  
  
```  
EXECUTE msdb.dbo.sysmail_update_profileaccount_sp  
    @profile_name = 'AdventureWorks Administrator'  
    ,@account_name = 'Admin-BackupServer',  
    ,@sequence_number = 3;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Correo electrónico de base de datos](../../relational-databases/database-mail/database-mail.md)   
 [Creación de una cuenta de Correo electrónico de base de datos](../../relational-databases/database-mail/create-a-database-mail-account.md)   
 [Correo electrónico de base de datos objetos de configuración](../../relational-databases/database-mail/database-mail-configuration-objects.md)   
 [Correo electrónico de base de datos procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
