---
description: SET OFFSETS (Transact-SQL)
title: SET OFFSETS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SET_OFFSETS_TSQL
- OFFSETS_TSQL
- SET OFFSETS
- OFFSETS
dev_langs:
- TSQL
helpviewer_keywords:
- position relative to start of statement [SQL Server]
- OFFSETS option
- offsets [SQL Server]
- SET OFFSETS statement
ms.assetid: c7bcc697-0930-4630-acae-d8ccbfa4414c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7226c450ed3c854847d609e2ddadf2f250ea5691
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189489"
---
# <a name="set-offsets-transact-sql"></a>SET OFFSETS (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve el desplazamiento (posición con respecto al inicio de una instrucción) de las palabras clave especificadas en instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] a aplicaciones DB-Library.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
 
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
  
SET OFFSETS keyword_list { ON | OFF }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *keyword_list*  
 Es una lista separada por comas de construcciones de [!INCLUDE[tsql](../../includes/tsql-md.md)], entre las que se incluyen SELECT, FROM, ORDER, TABLE, PROCEDURE, STATEMENT, PARAM y EXECUTE.  
  
## <a name="remarks"></a>Observaciones  
 SET OFFSETS solo se utiliza en aplicaciones DB-Library.  
  
 La opción SET OFFSETS se establece en tiempo de análisis, no en tiempo de ejecución. El establecimiento en tiempo de análisis significa que si la instrucción SET está presente en el proceso por lotes o el procedimiento almacenado, la configuración tendrá efecto aunque la ejecución del código no llegue al punto donde se encuentre; y se aplicará antes de que se ejecute ninguna otra instrucción. Por ejemplo, la instrucción SET tendrá efecto incluso cuando se encuentre en un bloque de una instrucción IF…ELSE que no se alcance nunca en la ejecución, ya que se analiza el bloque de la instrucción IF…ELSE.  
  
 Si se establece SET OFFSETS en un procedimiento almacenado, su valor se restablecerá cuando el procedimiento almacenado devuelva el control. Por ello, una instrucción SET OFFSETS especificada en SQL dinámico no tiene ningún efecto en las instrucciones siguientes.  
  
 SET PARSEONLY devuelve los desplazamientos si la opción OFFSETS es ON y no hay errores.  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol **public** .  
  
## <a name="see-also"></a>Consulte también  
 [Instrucciones SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET PARSEONLY &#40;Transact-SQL&#41;](../../t-sql/statements/set-parseonly-transact-sql.md)  
  
  
