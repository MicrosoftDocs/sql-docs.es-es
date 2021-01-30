---
description: sp_update_category (Transact-SQL)
title: sp_update_category (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_update_category
- sp_update_category_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_update_category
ms.assetid: 098b926a-b078-4122-a5e1-3ef54b979dd4
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c771e031254821562933cd2e7f4d9f51903edba6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184723"
---
# <a name="sp_update_category-transact-sql"></a>sp_update_category (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cambia el nombre de una categoría.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_update_category  
     [@class =] 'class' ,   
     [@name =] 'old_name' ,  
     [@new_name =] 'new_name'  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @class = ] 'class'` Clase de la categoría que se va a actualizar. la *clase* es **VARCHAR (8)**, no tiene ningún valor predeterminado y puede tener uno de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**ALERT**|Actualiza una categoría de alerta.|  
|**TRABAJO**|Actualiza una categoría de trabajo.|  
|**OPERATOR**|Actualiza una categoría de operador.|  
  
`[ @name = ] 'old_name'` Nombre actual de la categoría. *old_name* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @new_name = ] 'new_name'` El nuevo nombre de la categoría. *new_name* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_update_category** se debe ejecutar desde la base de datos **msdb** .  
  
## <a name="permissions"></a>Permisos  
 Para ejecutar este procedimiento almacenado, se debe conceder a los usuarios el rol fijo de servidor **sysadmin** .  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se cambia el nombre de una categoría de trabajo de `AdminJobs` a `Administrative Jobs`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_update_category  
  @class = N'JOB',  
  @name = N'AdminJobs',  
  @new_name = N'Administrative Jobs' ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_add_category &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-category-transact-sql.md)   
 [sp_delete_category &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-category-transact-sql.md)   
 [sp_help_category &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-category-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
