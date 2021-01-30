---
description: sp_droptype (Transact-SQL)
title: sp_droptype (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_droptype_TSQL
- sp_droptype
dev_langs:
- TSQL
helpviewer_keywords:
- sp_droptype
ms.assetid: e78464ac-2370-4c4e-9cc0-06aebc07cec5
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b7614a0b11a22ed7680fa98a9fbe93663d175c1b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186998"
---
# <a name="sp_droptype-transact-sql"></a>sp_droptype (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Elimina un tipo de datos de alias de **systypes**.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_droptype [ @typename = ] 'type'  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @typename = ] 'type'` Es el nombre de un tipo de datos de alias que posee. *Type* es de tipo **sysname** y no tiene ningún valor predeterminado.  
  
## <a name="return-code-type"></a>Tipo de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="remarks"></a>Observaciones  
 No se puede quitar el tipo de datos del alias de **tipo** si las tablas u otros objetos de base de datos hacen referencia a él.  
  
> [!NOTE]  
>  Un tipo de datos de alias no puede quitarse si se utiliza en una definición de tabla, o si una regla o valor predeterminado está enlazado a él.  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol fijo de base de datos **db_owner** o el rol fijo de base de datos **db_ddladmin** .  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se quita el tipo de datos de alias `birthday`.  
  
> [!NOTE]  
>  Este tipo de datos de alias debe existir pues, de lo contrario, el ejemplo devolverá un mensaje de error.  
  
```  
USE master;  
GO  
EXEC sp_droptype 'birthday';  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Motor de base de datos procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [sp_addtype &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addtype-transact-sql.md)   
 [sp_rename &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
