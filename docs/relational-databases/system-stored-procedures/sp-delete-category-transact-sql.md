---
description: sp_delete_category (Transact-SQL)
title: sp_delete_category (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_delete_category_TSQL
- sp_delete_category
dev_langs:
- TSQL
helpviewer_keywords:
- sp_delete_category
ms.assetid: 63ea7d0d-a567-456e-a778-bee99e21d16c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 69befe88ea56ea64e6d6be33a96174d5bd8bb805
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195535"
---
# <a name="sp_delete_category-transact-sql"></a>sp_delete_category (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Quita del servidor actual la categoría especificada de trabajos, alertas u operadores.  
  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_delete_category [ @class = ] 'class' , [ @name = ] 'name'   
```  
  
## <a name="arguments"></a>Argumentos  
`[ @class = ] 'class'` Clase de la categoría. la *clase* es **VARCHAR (8)**, no tiene ningún valor predeterminado y debe tener uno de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**TRABAJO**|Elimina una categoría de trabajo.|  
|**ONALERT**|Elimina una categoría de alerta.|  
|**OPERATOR**|Elimina una categoría de operador.|  
  
`[ @name = ] 'name'` Nombre de la categoría que se va a quitar. *Name* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="remarks"></a>Observaciones  
 **sp_delete_category** se debe ejecutar desde la base de datos **msdb** .  
  
 Si se elimina una categoría, se establecerán nuevas clasificaciones de trabajos, alertas u operadores de esa categoría en función de la categoría predeterminada para la clase.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** pueden ejecutar este procedimiento.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se elimina la categoría de trabajo denominada `AdminJobs`.  
  
```  
USE msdb ;  
GO   
  
EXEC dbo.sp_delete_category  
    @name = N'AdminJobs',  
    @class = N'JOB' ;  
GO   
```  
  
## <a name="see-also"></a>Vea también  
 [sp_add_category &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-category-transact-sql.md)   
 [sp_help_category &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-category-transact-sql.md)   
 [sp_update_category &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-update-category-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
