---
description: Operadores bit a bit (Transact-SQL)
title: Operadores bit a bit (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/04/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- operators [Transact-SQL], bitwise
- bitwise operators
- bit manipulations
ms.assetid: 2b994cf5-2daa-438a-b8c7-4bd8d451ac8d
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 80e8e56ad03f1a3423359adc7803ce44f3e0925f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193394"
---
# <a name="bitwise-operators-transact-sql"></a>Operadores bit a bit (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Los operadores bit a bit realizan manipulaciones de bits entre dos expresiones de cualquiera de los tipos de datos de la categoría del tipo de datos entero.  
  Los operadores bit a bit convierten dos valores enteros en bits binarios y llevan a cabo la operación AND, OR o NOT correspondiente en cada bit, lo cual genera un resultado. Luego, convierten el resultado en un entero.  
  
  Por ejemplo, el entero 170 se convierte en el binario 1010 1010
y el entero 75 lo hace en el binario 0100 1011.

|operator|cálculo bit a bit|
|---- |---- |
|y <br> Si los dos bits, en cualquier ubicación, son 1, el resultado es 1. |1010 1010 = 170 <br>0100 1011 = 75 <br>-----------------  <br> 0000 1010 = 10 |
|O <br> Si cualquiera de los dos bits, en cualquier ubicación, es 1, el resultado es 1. |1010 1010 = 170 <br>0100 1011 = 75 <br>-----------------  <br> 1110 1011 = 235|
|NOT  <br> Invierte el valor de bit en cada ubicación de bits. |1010 1010 = 170 <br>----------------- <br>  0101 0101 = 85 |
  
Vea los siguientes temas:   
* [& &#40;AND bit a bit&#41;](../../t-sql/language-elements/bitwise-and-transact-sql.md)  
* [&= &#40;Asignación de AND bit a bit&#41;](../../t-sql/language-elements/bitwise-and-equals-transact-sql.md)   
* [&#124; &#40;OR bit a bit&#41;](../../t-sql/language-elements/bitwise-or-transact-sql.md)  
* [&#124;= &#40;Asignación de OR bit a bit&#41;](../../t-sql/language-elements/bitwise-or-equals-transact-sql.md)   
* [^ &#40;OR exclusivo bit a bit&#41;](../../t-sql/language-elements/bitwise-exclusive-or-transact-sql.md)  
* [^= &#40;Asignación de OR exclusivo bit a bit&#41;](../../t-sql/language-elements/bitwise-exclusive-or-equals-transact-sql.md)  
* [~ &#40;NOT bit a bit&#41;](../../t-sql/language-elements/bitwise-not-transact-sql.md)  
  
 Los operandos de los operadores bit a bit pueden ser de cualquiera de los tipos de datos de las categorías de tipos de datos entero o de cadena binaria (excepto el tipo de datos **image**), con la excepción de que ambos operandos no pueden ser de uno de los tipos de datos de la categoría de tipos de datos de cadena binaria. La siguiente tabla muestra los tipos de datos de operando admitidos.  
  
|Operando izquierdo|Operando derecho|  
|------------------|-------------------|  
|[binary](../../t-sql/data-types/binary-and-varbinary-transact-sql.md)|**int**, **smallint** o **tinyint**|  
|[bit](../../t-sql/data-types/bit-transact-sql.md)|**int**, **smallint**, **tinyint** o **bit**|  
|[bigint](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)|**bigint**, **int**, **smallint**, **tinyint**, **binary** o **varbinary**|  
|[int](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)|**int**, **smallint**, **tinyint**, **binary** o **varbinary**|  
|[smallint](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)|**int**, **smallint**, **tinyint**, **binary** o **varbinary**|  
|[tinyint](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)|**int**, **smallint**, **tinyint**, **binary** o **varbinary**|  
|[varbinary](../../t-sql/data-types/binary-and-varbinary-transact-sql.md)|**int**, **smallint** o **tinyint**|  
  
## <a name="see-also"></a>Consulte también  
 [Operadores &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Operadores compuestos &#40;Transact-SQL&#41;](../../t-sql/language-elements/compound-operators-transact-sql.md)
