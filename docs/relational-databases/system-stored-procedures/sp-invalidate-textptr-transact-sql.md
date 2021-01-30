---
description: sp_invalidate_textptr (Transact-SQL)
title: sp_invalidate_textptr (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_invalidate_textptr_TSQL
- sp_invalidate_textptr
dev_langs:
- TSQL
helpviewer_keywords:
- sp_invalidate_textptr
ms.assetid: dd9920e1-7064-4c05-93d8-9303103fa1d6
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a94697c1ef1e86c8e95d4cf8c6088cb83a261fa3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210221"
---
# <a name="sp_invalidate_textptr-transact-sql"></a>sp_invalidate_textptr (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Invalida el puntero de texto consecutivo especificado en la transacción, o todos ellos. **sp_invalidate_textptr** solo se puede usar en punteros de texto consecutivos. Estos punteros son de tablas que tienen habilitada la opción **Text in row** .  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_invalidate_textptr [ [ @TextPtrValue = ] textptr_value ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @TextPtrValue = ] textptr_value` Es el puntero de texto consecutivo que se va a invalidar. *textptr_value* es de tipo **varbinary (** 16 **)** y su valor predeterminado es NULL. Si es NULL, **sp_invalidate_textptr** invalida todos los punteros de texto consecutivos de la transacción.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="remarks"></a>Observaciones  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] admite un máximo de 1.024 punteros de texto consecutivos activos y válidos por transacción y base de datos; sin embargo, una transacción que comprenda más de una base de datos puede disponer de 1.024 punteros de texto consecutivos en cada una. **sp_invalidate_textptr** se puede usar para invalidar los punteros de texto consecutivos y, por tanto, espacio libre para los punteros de texto consecutivos adicionales.  
  
 Para más información sobre la opción text in row, vea [sp_tableoption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md).  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol **public** .  
  
## <a name="see-also"></a>Consulte también  
 [Motor de base de datos procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [sp_tableoption &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md)   
 [TEXTPTR &#40;Transact-SQL&#41;](../../t-sql/functions/text-and-image-functions-textptr-transact-sql.md)   
 [TEXTVALID &#40;Transact-SQL&#41;](../../t-sql/functions/text-and-image-functions-textvalid-transact-sql.md)  
  
  
