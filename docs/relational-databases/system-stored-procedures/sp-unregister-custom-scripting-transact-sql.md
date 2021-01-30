---
description: sp_unregister_custom_scripting (Transact-SQL)
title: sp_unregister_custom_scripting (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_unregister_custom_scripting_TSQL
- sp_unregister_custom_scripting
helpviewer_keywords:
- sp_unregister_custom_scripting
ms.assetid: b6e9e0d2-9144-434d-88af-4874f2582399
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 7dc5ff01b5dfb5742233dce8f89384edbc9db957
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209664"
---
# <a name="sp_unregister_custom_scripting-transact-sql"></a>sp_unregister_custom_scripting (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Este procedimiento almacenado quita un archivo de script o un procedimiento almacenado personalizado definido por el usuario [!INCLUDE[tsql](../../includes/tsql-md.md)] que se registró ejecutando [sp_register_custom_scripting](../../relational-databases/system-stored-procedures/sp-register-custom-scripting-transact-sql.md). Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_unregister_custom_scripting [ @type  = ] 'type'  
    [ , [ @publication = ] 'publication' ]  
    [ , [ @article = ] 'article' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @type = ] 'type'` Es el tipo de procedimiento almacenado personalizado o el script que se va a quitar. *Type* es de tipo **VARCHAR (16)**, no tiene ningún valor predeterminado y puede tener uno de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**insert**|El procedimiento almacenado personalizado registrado o el script se ejecuta cuando se replica una instrucción INSERT.|  
|**update**|El procedimiento almacenado personalizado registrado o el script se ejecuta cuando se replica una instrucción UPDATE.|  
|**delete**|El procedimiento almacenado personalizado registrado o el script se ejecuta cuando se replica una instrucción DELETE.|  
|**custom_script**|El procedimiento almacenado personalizado registrado o el script se ejecuta al final del desencadenador de lenguaje de definición de datos (DDL).|  
  
`[ @publication = ] 'publication'` Nombre de la publicación para la que se va a quitar el procedimiento almacenado personalizado o el script. *Publication* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @article = ] 'article'` Nombre del artículo para el que se va a quitar el procedimiento almacenado personalizado o el script. *article* es de **tipo sysname y su** valor predeterminado es NULL.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_unregister_custom_scripting** se utiliza en la replicación de instantáneas y transaccional.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** , el rol fijo de base de datos **db_owner** o el rol fijo de base de datos **db_ddladmin** pueden ejecutar **sp_unregister_custom_scripting**.  
  
## <a name="see-also"></a>Consulte también  
 [sp_register_custom_scripting &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-register-custom-scripting-transact-sql.md)  
  
  
