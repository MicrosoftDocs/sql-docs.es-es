---
description: '*= (Asignación de multiplicación) (Transact-SQL)'
title: '*= (Asignación de multiplicación) (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- '*=_TSQL'
- '*='
dev_langs:
- TSQL
helpviewer_keywords:
- compound operators, *=
- assignment operators, *=
- augmented operators, *=
- '*= (multiply equals)'
- '*= (multiplication assignment)'
ms.assetid: 816ff5dc-9a40-4c07-8351-39c194dbc079
author: rothja
ms.author: jroth
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4a1db40ce501f6793f1997490bbf113dc54adad7
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97460988"
---
# <a name="-multiplication-assignment-transact-sql"></a>*= (Asignación de multiplicación) (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Multiplica dos números y establece un valor como resultado de la operación. Por ejemplo, si una variable @x es igual a 35, @x *= 2 toma el valor original de @x, lo multiplica por 2 y establece @x en el nuevo valor (70).  
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql  
expression *= expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
_expression_  
Es cualquier [expresión](../../t-sql/language-elements/expressions-transact-sql.md) de tipo de datos válida, excepto el tipo **bit** de la categoría numérica.  
  
## <a name="result-types"></a>Tipos de resultado  
Devuelve el tipo de datos del argumento con mayor prioridad. Para obtener más información, vea [Prioridad de tipo de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-type-precedence-transact-sql.md).  
  
## <a name="remarks"></a>Comentarios  
Para más información, vea [&#42; &#40;Multiplicación&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/multiply-transact-sql.md).  
  
## <a name="see-also"></a>Vea también  
[Operadores compuestos &#40;Transact-SQL&#41;](../../t-sql/language-elements/compound-operators-transact-sql.md)   
[Expresiones &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
[Operadores &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)  
  
  
