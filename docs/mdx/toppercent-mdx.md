---
description: TopPercent (MDX)
title: Porcentaje (MDX) | Microsoft Docs
ms.date: 06/04/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: f275628747d0b17ede6c76f67961fe5233e788c4
ms.sourcegitcommit: 80701484b8f404316d934ad2a85fd773e26ca30c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2020
ms.locfileid: "93243544"
---
# <a name="toppercent-mdx"></a>TopPercent (MDX)


  Ordena un conjunto de forma descendente y devuelve un conjunto de tuplas con los valores más altos con un total acumulado mayor o igual a un porcentaje especificado.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
TopPercent(Set_Expression, Percentage, Numeric_Expression)   
```  
  
## <a name="arguments"></a>Argumentos  
 *Set_Expression*  
 Expresión MDX (Expresiones multidimensionales) válida que devuelve un conjunto.  
  
 *Porcentaje*  
 Expresión numérica válida que especifica el porcentaje de tuplas que serán devueltas.  
  
> [!IMPORTANT]  
>  El *porcentaje* debe ser un valor positivo; los valores negativos generan un error.  
  
 *Numeric_Expression*  
 Expresión numérica válida que suele ser una expresión MDX de las coordenadas de celdas que devuelven un número.  
  
## <a name="remarks"></a>Observaciones  
 La función de **porcentaje** calcula la suma de la expresión numérica especificada evaluada sobre el conjunto especificado, ordenando el conjunto en orden descendente. A continuación, la función devuelve los elementos con los valores más altos cuyo porcentaje acumulado del valor total sumado sea al menos el porcentaje especificado. Esta función devuelve el subconjunto más pequeño de un conjunto cuyo total acumulado sea al menos el porcentaje especificado. Los elementos devueltos se ordenan de mayor a menor.  
  
> [!WARNING]  
>  Si *Numeric_Expression*  devuelve cualquier valor negativo, el **porcentaje** devuelve solo una fila (1).  
>   
>  Vea en el segundo ejemplo una presentación detallada de este comportamiento.  
  
> [!IMPORTANT]  
>  Al igual que la función [BottomPercent](../mdx/bottompercent-mdx.md) , la función de **porcentaje** siempre rompe la jerarquía.  
  
## <a name="examples"></a>Ejemplos  

### <a name="a-return-toppercent"></a>A. Devolver el porcentaje

 En el ejemplo siguiente se devuelven las mejores ciudades que contribuyen a realizar el 10% principal de ventas de los distribuidores en la categoría Bike. El resultado se mostrará en orden descendente, empezando por la ciudad que tiene el valor de ventas más elevado.  
  
```  
SELECT [Measures].[Reseller Sales Amount] ON 0,  
TopPercent  
   ({[Geography].[Geography].[City].Members}  
   , 10  
   , [Measures].[Reseller Sales Amount]  
   ) ON 1  
FROM [Adventure Works]  
WHERE([Product].[Product Categories].[Bikes])  
```  
  
 La expresión anterior produce los siguientes resultados:  
  
|City|Reseller Sales Amount|  
|-|---------------------------|  
|Toronto|$3.508.904,84|  
|London|$1.521.530,09|  
|Seattle|$1.209.418,16|  
|Paris|$1.170.425,18|  
  
 El conjunto de datos original se puede obtener con la siguiente consulta y devuelve 588 filas:  
  
```  
SELECT [Measures].[Reseller Sales Amount] ON 0,  
Order  
   ({[Geography].[Geography].[City].Members}  
   , [Measures].[Reseller Sales Amount]  
   , BDESC  
   ) ON 1  
FROM [Adventure Works]  
WHERE([Product].[Product Categories].[Bikes])  
  
```  
  
### <a name="b-understand-the-effect-of-negative-values"></a>B. Entender el efecto de los valores negativos

 El siguiente tutorial le ayudará a entender el efecto de los valores negativos en el *Numeric_Expression* . En primer lugar, crearemos cierto contexto donde podamos mostrar el comportamiento.  
  
 La consulta siguiente devuelve una tabla con el importe de las ventas, el costo total del producto y el beneficio bruto de los distribuidores en orden descendente en función del beneficio. Tenga en cuenta que solo hay valores negativos en el beneficio; por tanto, la pérdida más pequeña aparece en la parte superior.  
  
```  
SELECT { [Measures].[Reseller Sales Amount], [Measures].[Reseller Total Product Cost], [Measures].[Reseller Gross Profit] } ON columns  
     ,  ORDER( [Product].[Product Categories].[Bikes].[Touring Bikes].children, [Measures].[Reseller Gross Profit], BDESC )   ON rows  
FROM [Adventure Works]  
  
```  
  
 La consulta anterior devuelve los siguientes resultados (las filas de la sección intermedia se han quitado para facilitar la lectura).  
  
|Bicicletas de paseo|Reseller Sales Amount|Reseller Total Product Cost|Reseller Gross Profit|  
|-|---------------------------|---------------------------------|---------------------------|  
|Touring-2000 Blue, 50|$157.444,56|$163.112,57|($5.668,01)|  
|Paseo: 2000 azul, 46|$321.027,03|$333.021,50|($11.994,47)|  
|Paseo: 3000 azul, 62|$87.773,61|$100.133,52|($12.359,91)|  
|...|...|...|...|  
|Paseo: 1000 amarillo, 46|$1.016.312,83|$1.234.454,27|($218.141,44)|  
|Touring-1000 Yellow, 60|$1.184.363,30|$1.443.407,51|($259.044,21)|  
  
 Ahora, si le pidieran que presentara el 100% de las mejores bicicletas según el beneficio, escribiría una consulta como esta:  
  
```  
SELECT { [Measures].[Reseller Sales Amount], [Measures].[Reseller Total Product Cost], [Measures].[Reseller Gross Profit] } ON columns  
     ,  TOPPERCENT( [Product].[Product Categories].[Bikes].[Touring Bikes].children, 100,[Measures].[Reseller Gross Profit] )   ON rows  
FROM [Adventure Works]  
  
```  
  
 Tenga en cuenta que la consulta solicita el cien por cien (100%); esto significa que deben devolverse todas las filas. Sin embargo, dado que hay valores negativos en el *Numeric_Expression* , solo se devuelve una fila.  
  
|Bicicletas de paseo|Reseller Sales Amount|Reseller Total Product Cost|Reseller Gross Profit|  
|-|---------------------------|---------------------------------|---------------------------|  
|Touring-2000 Blue, 50|$157.444,56|$163.112,57|($5.668,01)|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de funciones MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  
