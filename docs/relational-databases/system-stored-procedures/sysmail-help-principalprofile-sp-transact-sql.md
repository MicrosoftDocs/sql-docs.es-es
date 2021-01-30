---
description: sysmail_help_principalprofile_sp (Transact-SQL)
title: sysmail_help_principalprofile_sp (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/02/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_help_principalprofile_sp_TSQL
- sysmail_help_principalprofile_sp
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_help_principalprofile_sp
ms.assetid: 0cfd6464-09c7-4f03-9d25-58001c096a9e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: efb4b5cd655bf4a401d530e4c9bd52cbc9660a65
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181957"
---
# <a name="sysmail_help_principalprofile_sp-transact-sql"></a>sysmail_help_principalprofile_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Muestra información acerca de las asociaciones entre los perfiles del Correo electrónico de base de datos y las entidades de seguridad de la base de datos.  
  
 
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sysmail_help_principalprofile_sp [ {   [ @principal_id = ] principal_id | [ @principal_name = ] 'principal_name' } ]  
    [ [ , ] {   [ @profile_id = ] profile_id | [ @profile_name = ] 'profile_name' } ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @principal_id = ] principal_id` Es el identificador del usuario o el rol de la base de datos **msdb** de la asociación que se va a mostrar. *principal_id* es de **tipo int** y su valor predeterminado es NULL. Se puede especificar *principal_id* o *principal_name* .  
  
`[ @principal_name = ] 'principal_name'` Es el nombre del usuario o el rol de la base de datos **msdb** de la asociación que se va a mostrar. *principal_name* es de **tipo sysname y su** valor predeterminado es NULL. Se puede especificar *principal_id* o *principal_name* .  
  
`[ @profile_id = ] profile_id` Es el identificador del perfil para la asociación que se va a mostrar. *profile_id* es de **tipo int** y su valor predeterminado es NULL. Se puede especificar *profile_id* o *profile_name* .  
  
`[ @profile_name = ] 'profile_name'` Es el nombre del perfil para la asociación que se va a mostrar. *profile_name* es de **tipo sysname y su** valor predeterminado es NULL. Se puede especificar *profile_id* o *profile_name* .  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 Devuelve un conjunto de resultados que contiene las columnas que se indican en la siguiente tabla.  
  
| Nombre de la columna | Tipo de datos | Descripción |
| ----------- | --------- | ----------- |
|**principal_id**|**int**|Id. del usuario de la base de datos.|  
|**principal_name**|**sysname**|Nombre del usuario de la base de datos.|  
|**profile_id**|**int**|Número de Id. del perfil de Correo electrónico de base de datos.|  
|**profile_name**|**sysname**|Nombre del perfil de Correo electrónico de base de datos.|  
|**is_default**|**bit**|Marca que indica si el perfil es el perfil predeterminado del usuario.|  
  
## <a name="remarks"></a>Observaciones  
 Si **sysmail_help_principalprofile_sp** se invoca sin parámetros, el conjunto de resultados devuelto enumera todas las asociaciones de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . En caso contrario, el conjunto de resultados contiene información para las asociaciones que coincidan con los parámetros suministrados. Por ejemplo, en el procedimiento se muestran todas las asociaciones para un perfil cuando se proporciona el nombre del perfil.  
  
 **sysmail_help_principalprofile_sp** está en la base de datos **msdb** y pertenece al esquema **DBO** . El procedimiento se debe ejecutar con un nombre de tres partes si la base de datos actual no es **msdb**.  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol fijo de servidor **sysadmin** .  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-listing-information-for-a-specific-association"></a>A. Mostrar información para una asociación específica  
 En el ejemplo siguiente se indica cómo mostrar información para todas las asociaciones entre el perfil `AdventureWorks Administrator` y la entidad de seguridad `ApplicationLogin` en la base de datos `msdb`.  
  
```  
EXECUTE msdb.dbo.sysmail_help_principalprofile_sp  
    @principal_name = 'danw',  
    @profile_name = 'AdventureWorks Administrator' ;  
```  
  
 A continuación se muestra un conjunto de resultados de ejemplo con el formato cambiado para la longitud de línea.  
  
```  
principal_id principal_name     profile_id  profile_name                   is_default  
------------ ------------------ ----------- ------------------------------ ----------  
5            danw               9           AdventureWorks Administrator   1  
```  
  
### <a name="b-listing-information-for-all-associations"></a>B. Mostrar información para todas las asociaciones  
 En el ejemplo siguiente se indica cómo mostrar la información de todas las asociaciones en la instancia.  
  
```  
EXECUTE msdb.dbo.sysmail_help_principalprofile_sp ;  
```  
  
 A continuación se muestra un conjunto de resultados de ejemplo con el formato cambiado para la longitud de línea.  
  
```  
principal_id principal_name     profile_id  profile_name                   is_default  
------------ ------------------ ----------- ------------------------------ ----------  
6            terrid             3           Product Update Profile         1  
5            danw               9           AdventureWorks Administrator   1  
```  
  
## <a name="see-also"></a>Consulte también  
 [Correo electrónico de base de datos](../../relational-databases/database-mail/database-mail.md)   
 [Correo electrónico de base de datos procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
