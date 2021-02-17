---
title: Floor (función de XQuery) | Microsoft Docs
description: Obtenga información sobre la función Floor () de XQuery que devuelve el número más grande sin parte de fracción que no sea mayor que el valor de su argumento.
ms.custom: ''
ms.date: 03/09/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- floor function [XQuery]
- fn:floor function
ms.assetid: 4ace57dd-b66e-4b60-a2b9-a1b0f1a0831d
author: rothja
ms.author: jroth
ms.openlocfilehash: 206bacf618d38c2dbbe593355d48ed66f7a9e67f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341543"
---
# <a name="numeric-values-functions---floor"></a>Funciones de valores numéricos: floor
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Devuelve el mayor número sin fracción que no supera el valor de su argumento. Si el argumento es una secuencia vacía, devuelve la secuencia vacía.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
fn:floor ($arg as numeric?) as numeric?  
```  
  
## <a name="arguments"></a>Argumentos  
 *$arg*  
 Número al que se aplica la función.  
  
## <a name="remarks"></a>Observaciones  
 Si el tipo de *$arg* es uno de los tres tipos base numéricos, **xs: Float**, **xs: Double** o **xs: decimal**, el tipo de valor devuelto es el mismo que el tipo de *$arg* . Si el tipo de *$arg* es un tipo derivado de uno de los tipos numéricos, el tipo de valor devuelto es el tipo numérico base.  
  
 Si la entrada de las funciones FN: Floor, FN: Ceiling o FN: Round es **XDT: untypedAtomic**, datos sin tipo, se convierte implícitamente a **xs: Double**. Cualquier otro tipo genera un error estático.  
  
## <a name="examples"></a>Ejemplos  
 En este tema se proporcionan ejemplos de XQuery con instancias XML almacenadas en varias columnas de tipo **XML** en la base de datos de ejemplo AdventureWorks.  
  
 Puede usar el ejemplo de trabajo de la [función Ceiling (XQuery)](../xquery/numeric-values-functions-ceiling.md) para la función **Floor ()** de XQuery. Todo lo que tiene que hacer es sustituir la función **Ceiling ()** de la consulta por la función **Floor ()** .  
  
## <a name="implementation-limitations"></a>Limitaciones de la implementación  
 Éstas son las limitaciones:  
  
-   La función **Floor ()** asigna todos los valores enteros a XS: decimal.  
  
## <a name="see-also"></a>Consulte también  
 [Función Ceiling &#40;XQuery&#41;](../xquery/numeric-values-functions-ceiling.md)   
 [Función Round &#40;XQuery&#41;](../xquery/numeric-values-functions-round.md)   
 [Funciones de XQuery con el tipo de datos xml](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
