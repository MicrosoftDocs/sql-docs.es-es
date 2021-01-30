---
description: sysmail_help_configure_sp (Transact-SQL)
title: sysmail_help_configure_sp (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_help_configure_sp
- sysmail_help_configure_sp_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_help_configure_sp
ms.assetid: e598d4c8-3041-4965-b046-dce3a8e3d3e0
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 21ce11d37659561e172c11fb7b5a899193141166
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181972"
---
# <a name="sysmail_help_configure_sp-transact-sql"></a>sysmail_help_configure_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Muestra la configuración del Correo electrónico de base de datos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sysmail_help_configure_sp  [ [ @parameter_name = ] 'parameter_name' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @parameter_name = ] 'parameter_name'` Nombre del valor de configuración que se va a recuperar. Cuando se especifica, el valor de la opción de configuración se devuelve en el parámetro de salida **\@ parameter_value** . Cuando no se especifica ningún **\@ parameter_name** , este procedimiento almacenado devuelve un conjunto de resultados que contiene todos los valores de configuración de correo electrónico de base de datos en la instancia.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 Cuando no se especifica ningún **\@ parameter_name** , devuelve un conjunto de resultados con las columnas siguientes.  
  
| Nombre de la columna | Tipo de datos | Descripción |
| ----------- | --------- | ----------- |
|**paramName**|**nvarchar(256)**|Nombre del parámetro de configuración.|  
|**parámetro ParamValue**|**nvarchar(256)**|Valor del parámetro de configuración.|  
|**description**|**nvarchar(256)**|Descripción del parámetro de configuración.|  
  
## <a name="remarks"></a>Observaciones  
 En el procedimiento almacenado **sysmail_help_configure_sp** se enumeran los valores de configuración correo electrónico de base de datos actuales para la instancia.  
  
 Cuando se especifica un **\@ parameter_name** , pero no se proporciona ningún parámetro de salida para **\@ parameter_value**, este procedimiento almacenado no genera ninguna salida.  
  
 El procedimiento almacenado **sysmail_help_configure_sp** está en la base de datos **msdb** y pertenece al esquema **DBO** . El procedimiento se debe invocar con un nombre de tres partes si la base de datos actual no es **msdb**.  
  
## <a name="permissions"></a>Permisos  
 Los permisos de ejecución para este procedimiento tienen como valor predeterminado los miembros del rol fijo de servidor **sysadmin** .  
  
## <a name="examples"></a>Ejemplos  
 En este ejemplo se muestra la lista de parámetros de configuración del Correo electrónico de base de datos de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```  
EXECUTE msdb.dbo.sysmail_help_configure_sp ;  
```  
  
 El siguiente es un conjunto de resultados de ejemplo con la longitud de línea editada:  
  
```  
paramname                       paramvalue      description  
------------------------------- --------------- -----------------------------------------------------------------------------  
AccountRetryAttempts            1               Number of retry attempts for a mail server  
AccountRetryDelay               5000            Delay between each retry attempt to mail server  
DatabaseMailExeMinimumLifeTime  600             Minimum process lifetime in seconds  
DefaultAttachmentEncoding       MIME            Default attachment encoding  
LoggingLevel                    2               Database Mail logging level: normal - 1, extended - 2 (default), verbose - 3  
MaxFileSize                     1000000         Default maximum file size  
ProhibitedExtensions            exe,dll,vbs,js  Extensions not allowed in outgoing mails  
  
```  
  
## <a name="see-also"></a>Consulte también  
 [Correo electrónico de base de datos](../../relational-databases/database-mail/database-mail.md)   
 [Correo electrónico de base de datos procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
