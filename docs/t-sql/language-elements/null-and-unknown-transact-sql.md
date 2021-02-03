---
description: NULL y UNKNOWN (Transact-SQL)
title: NULL y UNKNOWN (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 9d491846-4730-4740-a680-77c69fae4a58
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 645f70cbef933dd372227202162d9dedeca2c377
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185249"
---
# <a name="null-and-unknown-transact-sql"></a>NULL y UNKNOWN (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-pdw-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-pdw-md.md)]

  NULL indica que el valor es desconocido. Un valor NULL no es lo mismo que un valor cero o vacío. No hay dos valores NULL que sean iguales. La comparación entre dos valores NULL, o entre un valor NULL y cualquier otro valor, tiene un resultado desconocido porque el valor de cada NULL es desconocido.  
  
 Normalmente, los valores NULL indican que los datos son desconocidos, no aplicables o que se van a agregar posteriormente. Por ejemplo, la inicial de un cliente puede que no sea conocida en el momento en que éste hace un pedido.  
  
 Tenga en cuenta lo siguiente sobre los valores NULL:  
  
-   Para comprobar si hay valores NULL en una consulta, use IS NULL o IS NOT NULL en la cláusula WHERE.  
  
-   Los valores NULL se pueden insertar en una columna si se indica explícitamente NULL en una instrucción INSERT o UPDATE, o bien si se deja fuera una columna de una instrucción INSERT.  
  
-   Los valores NULL no se pueden usar como la información necesaria para distinguir una fila de una tabla de otra fila, como, por ejemplo, las claves principales, o bien para la información que se usa para la distribución de filas, como las claves de distribución.  
  
 Cuando hay valores NULL en los datos, los operadores lógicos y de comparación pueden devolver un tercer resultado UNKNOWN (desconocido) en lugar de simplemente TRUE (verdadero) o FALSE (falso). Esta necesidad de una lógica de tres valores es el origen de muchos errores de la aplicación. Los operadores lógicos en una expresión booleana que incluya valores UNKNOWN devolverán UNKNOWN a menos que el resultado del operador no dependa de la expresión UNKNOWN. En estas tablas se proporcionan ejemplos de este comportamiento.  
  
 En la tabla siguiente se muestra el resultado de aplicar un operador AND a dos expresiones booleanas donde una devuelve UNKNOWN.  
  
|Expresión 1|Expresión 2|Resultado|  
|---------------|---------------|------------|  
|true|DESCONOCIDO|DESCONOCIDO|  
|DESCONOCIDO|DESCONOCIDO|DESCONOCIDO|  
|FALSE|DESCONOCIDO|FALSE|  
  
 En la tabla siguiente se muestra el resultado de aplicar un operador OR a dos expresiones booleanas donde una devuelve UNKNOWN.  
  
|Expresión 1|Expresión 2|Resultado|  
|---------------|---------------|------------|  
|true|DESCONOCIDO|true|  
|DESCONOCIDO|DESCONOCIDO|DESCONOCIDO|  
|FALSE|DESCONOCIDO|DESCONOCIDO|  
  
## <a name="see-also"></a>Consulte también  
 [AND &#40;Transact-SQL&#41;](../../t-sql/language-elements/and-transact-sql.md)   
 [OR &#40;Transact-SQL&#41;](../../t-sql/language-elements/or-transact-sql.md)   
 [NOT &#40;Transact-SQL&#41;](../../t-sql/language-elements/not-transact-sql.md)   
 [IS NULL &#40;Transact-SQL&#41;](../../t-sql/queries/is-null-transact-sql.md)  
  
  
