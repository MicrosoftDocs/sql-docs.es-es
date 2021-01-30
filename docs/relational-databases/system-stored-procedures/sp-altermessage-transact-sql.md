---
description: sp_altermessage (Transact-SQL)
title: sp_altermessage (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_altermessage_TSQL
- sp_altermessage
dev_langs:
- TSQL
helpviewer_keywords:
- sp_altermessage
ms.assetid: 1b28f280-8ef9-48e9-bd99-ec14d79abaca
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c5f7a871967c854376e9601373e4e28c3532cf9a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203255"
---
# <a name="sp_altermessage-transact-sql"></a>sp_altermessage (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifica el estado de los mensajes definidos por el usuario o del sistema en una instancia del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. Los mensajes definidos por el usuario se pueden ver mediante la vista de catálogo **Sys. Messages** .  

  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_altermessage [ @message_id = ] message_number   ,[ @parameter = ]'write_to_log'  
   ,[ @parameter_value = ]'value'   
```  
  
## <a name="arguments"></a>Argumentos  
 [**@message_id =** ] *message_number*  
 Es el número de error del mensaje que se va a modificar de **Sys. Messages**. *message_number* es de **tipo int** y no tiene ningún valor predeterminado.  
  
`[ @parameter = ] 'write\_to\_log_'`Se utiliza con **\@ parameter_value** para indicar que el mensaje se va a escribir en el [!INCLUDE[msCoName](../../includes/msconame-md.md)] registro de aplicación de Windows. *write_to_log* es de **tipo sysname** y no tiene ningún valor predeterminado. *write_to_log* debe establecerse en WITH_LOG o null. Si *write_to_log* se establece en WITH_LOG o NULL y el valor de **\@ parameter_value** es **true**, el mensaje se escribe en el registro de aplicación de Windows. Si *write_to_log* se establece en WITH_LOG o NULL y el valor de **\@ parameter_value** es **false**, el mensaje no se escribe siempre en el registro de aplicación de Windows, pero se puede escribir en función de cómo se haya producido el error. Si se especifica *write_to_log* , también se debe especificar el valor de **\@ parameter_value** .  
  
> [!NOTE]  
>  Si se escribe un mensaje en el registro de aplicación Windows, también se escribe en el archivo de registro de errores del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
`[ @parameter_value = ]'value_'`Se usa con el **\@ parámetro** para indicar que el error se va a escribir en el [!INCLUDE[msCoName](../../includes/msconame-md.md)] registro de aplicación de Windows. el *valor* es **VARCHAR (5)** y no tiene ningún valor predeterminado. Si es **true**, el error siempre se escribe en el registro de aplicación de Windows. Si es **false**, el error no siempre se escribe en el registro de aplicación de Windows, pero se puede escribir en función de cómo se haya producido el error. Si se especifica *Value* , también se debe especificar *write_to_log* para el **\@ parámetro** .  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="remarks"></a>Observaciones  
 El efecto de **sp_altermessage** con la opción WITH_LOG es similar al del parámetro RAISERROR with log, salvo que **sp_altermessage** cambia el comportamiento de registro de un mensaje existente. Si se ha modificado un mensaje para que sea WITH_LOG, siempre se escribe en el registro de aplicación Windows, independientemente de cómo el usuario invoque el error. Aunque se ejecute RAISERROR sin la opción WITH_LOG, el error se escribe en el registro de aplicaciones de Windows.  
  
 Los mensajes del sistema se pueden modificar mediante **sp_altermessage**.  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol fijo de servidor **ServerAdmin** .  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se hace que el mensaje `55001` existente se registre en el registro de aplicación Windows.  
  
```  
EXECUTE sp_altermessage 55001, 'WITH_LOG', 'true';  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [RAISERROR &#40;Transact-SQL&#41;](../../t-sql/language-elements/raiserror-transact-sql.md)   
 [sp_addmessage &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmessage-transact-sql.md)   
 [sp_dropmessage &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmessage-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
