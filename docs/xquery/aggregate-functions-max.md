---
title: Función MAX (XQuery) | Microsoft Docs
description: Obtenga información sobre la función MAX () de XQuery que devuelve el elemento de una secuencia cuyo valor es mayor que el de todos los demás.
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
- max function [XQuery]
- fn:max function
ms.assetid: 5ee625c0-044a-4cda-b210-02b64e619d65
author: rothja
ms.author: jroth
ms.openlocfilehash: b52ae499cf09e8fcd024f592ae639d0f9a7c1c5a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341964"
---
# <a name="aggregate-functions---max"></a>Funciones de agregado: max
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Devuelve de una secuencia de valores atómicos, *$arg*, un elemento cuyo valor es mayor que el de todos los demás.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
fn:max($arg as xdt:anyAtomicType*) as xdt:anyAtomicType?  
```  
  
## <a name="arguments"></a>Argumentos  
 *$arg*  
 Secuencia de valores atómicos a partir de la cual se va a devolver el valor máximo.  
  
## <a name="remarks"></a>Observaciones  
 Todos los tipos de valores atomizados que se pasan a **Max ()** deben ser subtipos del mismo tipo base. Los tipos base aceptados son los tipos que admiten la operación **gt** . Entre estos tipos se incluyen los tres tipos base numéricos integrados, los tipos base de fecha y hora, xs:string, xs:boolean y xdt:untypedAtomic. Los valores del tipo xdt:untypedAtomic se convierten a xs:double. Si hay una combinación de estos tipos, o si se pasan otros valores de otros tipos, se genera un error estático.  
  
 El resultado de **Max ()** recibe el tipo base de los tipos pasados, como XS: Double en el caso de XDT: untypedAtomic. Si la entrada está vacía estáticamente, el vacío es implícito y se genera un error estático.  
  
 La función **Max ()** devuelve el valor de la secuencia que es mayor que cualquier otro en la secuencia de entrada. En el caso de los valores xs:string, se utiliza la intercalación de puntos de código Unicode predeterminada. Si un valor XDT: untypedAtomic no se puede convertir a XS: Double, el valor se omite en la secuencia de entrada, *$arg*. Si la entrada es una secuencia vacía calculada dinámicamente, se devolverá la secuencia vacía.  
  
## <a name="examples"></a>Ejemplos  
 En este tema se proporcionan ejemplos de XQuery con instancias XML almacenadas en varias columnas de tipo **XML** de la [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] base de datos.  
  
### <a name="a-using-the-max-xquery-function-to-find-work-center-locations-in-the-manufacturing-process-that-have-the-most-labor-hours"></a>A. Utilizar la función max() de XQuery para buscar las ubicaciones de centro de trabajo del proceso de fabricación que tienen más horas de trabajo  
 La consulta proporcionada en la [función min (XQuery)](../xquery/aggregate-functions-min.md) se puede volver a escribir para usar la función **Max ()** .  
  
## <a name="implementation-limitations"></a>Limitaciones de la implementación  
 Éstas son las limitaciones:  
  
-   La función **Max (**) asigna todos los enteros a XS: decimal.  
  
-   No se admite la función **Max ()** en valores de tipo XS: Duration.  
  
-   No se admiten las secuencias que mezclan tipos en límites de tipo base.  
  
-   No se admite la opción sintáctica que proporciona una intercalación.  
  
## <a name="see-also"></a>Consulte también  
 [Funciones de XQuery con el tipo de datos xml](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
