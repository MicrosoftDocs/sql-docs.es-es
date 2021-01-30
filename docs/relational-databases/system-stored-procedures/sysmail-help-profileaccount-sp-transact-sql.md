---
description: sysmail_help_profileaccount_sp (Transact-SQL)
title: sysmail_help_profileaccount_sp (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_help_profileaccount_sp_TSQL
- sysmail_help_profileaccount_sp
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_help_profileaccount_sp
ms.assetid: 3ea68271-0a6b-4d77-991c-4757f48f747a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 45b542047875527a26d9c4854b223e626efda5ea
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181937"
---
# <a name="sysmail_help_profileaccount_sp-transact-sql"></a>sysmail_help_profileaccount_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Muestra las cuentas asociadas con uno o varios perfiles del Correo electrónico de base de datos.  
    
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sysmail_help_profileaccount_sp  
   {   [ @profile_id = ] profile_id   
      | [ @profile_name = ] 'profile_name' }  
   [ , {   [ @account_id = ] account_id  
         | [ @account_name = ] 'account_name' } ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @profile_id = ] profile_id` Es el identificador del perfil que se va a enumerar. *profile_id* es de **tipo int** y su valor predeterminado es NULL. Se debe especificar *profile_id* o *profile_name* .  
  
`[ @profile_name = ] 'profile_name'` Es el nombre del perfil que se va a enumerar. *profile_name* es de **tipo sysname y su** valor predeterminado es NULL. Se debe especificar *profile_id* o *profile_name* .  
  
`[ @account_id = ] account_id` Es el identificador de la cuenta que se va a mostrar. *ACCOUNT_ID* es de **tipo int** y su valor predeterminado es NULL. Cuando *ACCOUNT_ID* y *ACCOUNT_NAME* son ambos null, enumera todas las cuentas del perfil.  
  
`[ @account_name = ] 'account_name'` Es el nombre de la cuenta que se va a mostrar. *account_name* es de **tipo sysname y su** valor predeterminado es NULL. Cuando *ACCOUNT_ID* y *ACCOUNT_NAME* son ambos null, enumera todas las cuentas del perfil.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 Devuelve un conjunto de resultados con las columnas siguientes:  
  
| Nombre de la columna | Tipo de datos | Descripción |
| ----------- | --------- | ----------- |
|**profile_id**|**int**|Id. de perfil del perfil.|  
|**profile_name**|**sysname**|Nombre del perfil.|  
|**account_id**|**int**|Id. de cuenta de la cuenta.|  
|**account_name**|**sysname**|Nombre de la cuenta.|  
|**sequence_number**|**int**|Número de secuencia de la cuenta en el perfil.|  
  
## <a name="remarks"></a>Observaciones  
 Cuando no se especifica ningún *profile_id* o *profile_name* , este procedimiento almacenado devuelve información para cada perfil de la instancia.  
  
 El procedimiento almacenado **sysmail_help_profileaccount_sp** está en la base de datos **msdb** y pertenece al esquema **DBO** . El procedimiento se debe ejecutar con un nombre de tres partes si la base de datos actual no es **msdb**.  
  
## <a name="permissions"></a>Permisos  
 Los permisos de ejecución para este procedimiento tienen como valor predeterminado los miembros del rol fijo de servidor **sysadmin** .  
  
## <a name="examples"></a>Ejemplos  
 **A. Mostrar las cuentas de un perfil específico por nombre**  
  
 En el ejemplo siguiente se indica cómo mostrar la información del perfil `AdventureWorks Administrator` especificando el nombre de perfil.  
  
```  
EXECUTE msdb.dbo.sysmail_help_profileaccount_sp  
   @profile_name = 'AdventureWorks Administrator';  
```  
  
 El siguiente es un conjunto de resultados de ejemplo con la longitud de línea editada:  
  
```  
profile_id  profile_name                 account_id  account_name         sequence_number  
----------- ---------------------------- ----------- -------------------- ---------------  
131         AdventureWorks Administrator 197         Admin-MainServer     1  
131         AdventureWorks Administrator 198         Admin-BackupServer   2  
```  
  
 **B. Mostrar las cuentas de un perfil específico por Id. de perfil**  
  
 En el ejemplo siguiente se indica cómo mostrar la información del perfil `AdventureWorks Administrator` especificando el Id. del perfil.  
  
```  
EXECUTE msdb.dbo.sysmail_help_profileaccount_sp  
    @profile_id = 131 ;  
```  
  
 El siguiente es un conjunto de resultados de ejemplo con la longitud de línea editada:  
  
```  
profile_id  profile_name                 account_id  account_name         sequence_number  
----------- ---------------------------- ----------- -------------------- ---------------  
131         AdventureWorks Administrator 197         Admin-MainServer     1  
131         AdventureWorks Administrator 198         Admin-BackupServer   2  
```  
  
 **C. Mostrar las cuentas de todos los perfiles**  
  
 En el ejemplo siguiente se indica cómo mostrar las cuentas de todos los perfiles de la instancia.  
  
```  
EXECUTE msdb.dbo.sysmail_help_profileaccount_sp;  
```  
  
 El siguiente es un conjunto de resultados de ejemplo con la longitud de línea editada:  
  
```  
profile_id  profile_name                 account_id  account_name         sequence_number  
----------- ---------------------------- ----------- -------------------- ---------------  
131         AdventureWorks Administrator 197         Admin-MainServer     1  
131         AdventureWorks Administrator 198         Admin-BackupServer   2  
106         AdventureWorks Operator      210         Operator-MainServer  1  
```  
  
## <a name="see-also"></a>Consulte también  
 [Correo electrónico de base de datos](../../relational-databases/database-mail/database-mail.md)   
 [Creación de una cuenta de Correo electrónico de base de datos](../../relational-databases/database-mail/create-a-database-mail-account.md)   
 [Correo electrónico de base de datos objetos de configuración](../../relational-databases/database-mail/database-mail-configuration-objects.md)   
 [Correo electrónico de base de datos procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
