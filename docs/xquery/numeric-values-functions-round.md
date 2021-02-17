---
title: Round (función de XQuery) | Microsoft Docs
description: Obtenga información sobre la función de XQuery Round () que devuelve el número que no tiene una parte fraccionaria más cercana al argumento especificado.
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- fn:round function
- round function [XQuery]
ms.assetid: 320b572f-bd5b-4055-95a6-dec5718c0041
author: rothja
ms.author: jroth
ms.openlocfilehash: f245b613c3e5e32fd2d4cc8eb09e969719ab1762
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347914"
---
# <a name="numeric-values-functions---round"></a>Funciones de valores numéricos: round
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Devuelve el número que no tiene una parte fraccionaria más próxima al valor del argumento. Si hay más de un número con esta característica, se devuelve el más próximo al infinito positivo. Por ejemplo:  
  
 Si el argumento es 2,5, **Round ()** devuelve 3.  
  
 Si el argumento es 2,4999, **Round ()** devuelve 2.  
  
 Si el argumento es-2,5, **Round ()** devuelve-2.  
  
 Si el argumento es una secuencia vacía, **Round ()** devuelve la secuencia vacía.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
fn:round ( $arg as numeric?) as numeric?  
```  
  
## <a name="arguments"></a>Argumentos  
 *$arg*  
 Número al que se aplica la función.  
  
## <a name="remarks"></a>Observaciones  
 Si el tipo de *$arg* es uno de los tres tipos base numéricos, **xs: Float**, **xs: Double** o **xs: decimal**, el tipo de valor devuelto es el mismo que el tipo de *$arg* . Si el tipo de *$arg* es un tipo derivado de uno de los tipos numéricos, el tipo de valor devuelto es el tipo numérico base.  
  
 Si la entrada de las funciones **FN: Floor**, **FN: Ceiling** o **FN: Round** es **XDT: untypedAtomic**, datos sin tipo, se convierte implícitamente a **xs: Double**.  
  
 Cualquier otro tipo genera un error estático.  
  
## <a name="examples"></a>Ejemplos  
 En este tema se proporcionan ejemplos de XQuery con instancias XML almacenadas en varias columnas de tipo **XML** de la base de datos AdventureWorks.  
  
 Puede usar el ejemplo de trabajo de la [función Ceiling (XQuery)](../xquery/numeric-values-functions-ceiling.md) para la función **Round ()** de XQuery. Todo lo que tiene que hacer es sustituir la función **Ceiling ()** de la consulta por la función **Round ()** .  
  
## <a name="implementation-limitations"></a>Limitaciones de la implementación  
 Éstas son las limitaciones:  
  
-   La función **Round ()** asigna valores enteros a XS: decimal.  
  
-   La función **Round ()** de los valores XS: Double y XS: Float entre-0.5 E0 y-0e0 se asignan a 0e0 en lugar de-0e0.  
  
## <a name="see-also"></a>Consulte también  
 [Función Floor &#40;XQuery&#41;](../xquery/numeric-values-functions-floor.md)   
 [Función Ceiling &#40;XQuery&#41;](../xquery/numeric-values-functions-ceiling.md)  
  
  
