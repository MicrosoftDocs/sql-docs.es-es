---
description: LinRegVariance (MDX)
title: LinRegVariance (MDX) | Microsoft Docs
ms.date: 06/04/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: bd223530c0b184dd723abbb68c2acccd1240d870
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/17/2020
ms.locfileid: "88477037"
---
# <a name="linregvariance-mdx"></a>LinRegVariance (MDX)


  Calcula la regresión lineal de un conjunto y devuelve la varianza asociada a la línea de regresión, y = AX + b.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
LinRegVariance(Set_Expression, Numeric_Expression_y [ ,Numeric_Expression_x ] ] )  
```  
  
## <a name="arguments"></a>Argumentos  
 *Set_Expression*  
 Expresión MDX (Expresiones multidimensionales) válida que devuelve un conjunto.  
  
 *Numeric_Expression_y*  
 Expresión numérica válida que suele ser una expresión MDX de las coordenadas de celdas que devuelven un número que representa los valores del eje Y.  
  
 *Numeric_Expression_x*  
 Expresión numérica válida que suele ser una expresión MDX de las coordenadas de celdas que devuelven un número que representa los valores del eje X.  
  
## <a name="remarks"></a>Observaciones  
 La regresión lineal, que utiliza el método de mínimos cuadrados, calcula la ecuación de la recta de regresión (es decir, la de mejor ajuste para un conjunto de puntos). La línea de regresión tiene la siguiente ecuación, donde a es la pendiente y b es la intercepción:  
  
 y = ax+b  
  
 La función **LinRegVariance** evalúa el setagainst especificado en la primera expresión numérica para obtener los valores del eje y. A continuación, la función evalúa el conjunto especificadocon la segunda expresión numérica, si se especifica, para obtener los valores del eje X. Si no se especifica la segunda expresión numérica, la función utiliza el contexto actual de las celdas del conjunto especificado como valores para el eje x. La no especificación del argumento del eje x se usa con frecuencia con la dimensión de tiempo.  
  
 Después de obtener el conjunto de puntos, la función **LinRegVariance** devuelve la varianza estadística que describe el ajuste de la ecuación lineal con los puntos.  
  
> [!NOTE]  
>  La función **LinRegVariance** omite las celdas vacías o las celdas que contienen texto o valores lógicos. No obstante, la función incluye celdas con valor de cero.  
  
## <a name="example"></a>Ejemplo  
 El ejemplo siguiente devuelve la varianza estadística que describe la adecuación de la ecuación lineal a los puntos de las medidas de ventas por unidad y ventas por tienda.  
  
```  
LinRegVariance(LastPeriods(10),[Measures].[Unit Sales],[Measures].[Store Sales])  
```  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de funciones MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  
