---
description: Crossjoin (MDX)
title: Crossjoin (MDX) | Microsoft Docs
ms.date: 06/04/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: 7929238ecb672dd6c537772dafb15422ca52bce3
ms.sourcegitcommit: cfa04a73b26312bf18d8f6296891679166e2754d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2020
ms.locfileid: "92196059"
---
# <a name="crossjoin-mdx"></a>Crossjoin (MDX)


  Devuelve el producto cruzado de uno o más conjuntos.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
Standard syntax  
Crossjoin(Set_Expression1 ,Set_Expression2 [,...n] )  
  
Alternate syntax  
Set_Expression1 * Set_Expression2 [* ...n]  
```  
  
## <a name="arguments"></a>Argumentos  
 *Set_Expression1*  
 Expresión MDX (Expresiones multidimensionales) válida que devuelve un conjunto.  
  
 *Set_Expression2*  
 Expresión MDX (Expresiones multidimensionales) válida que devuelve un conjunto.  
  
## <a name="remarks"></a>Comentarios  
 La función **Crossjoin** devuelve el producto cruzado de dos o más conjuntos especificados. El orden de las tuplas en el conjunto resultante depende del orden de los conjuntos que deben unirse y del orden de sus miembros. Por ejemplo, cuando el primer conjunto consta de {x1, x2,..., x*n*} y el segundo conjunto consta de {Y1, Y2,..., y*n*}, el producto cruzado de estos conjuntos es:  
  
 {(x1, Y1), (x1, Y2),..., (x1, y*n*), (x2, Y1), (x2, y2),...,  
  
 (x2, y*n*),..., (x*n*, Y1), (x*n*, Y2),..., (xn, y*n*)}  
  
> [!IMPORTANT]  
>  Si los conjuntos de la combinación cruzada están compuestos por tuplas de diferentes jerarquías de atributo de la misma dimensión, esta función solo devolverá aquellas tuplas que realmente existen. Para obtener más información, vea [conceptos clave de MDX &#40;Analysis Services&#41;](/analysis-services/multidimensional-models/mdx/key-concepts-in-mdx-analysis-services).  
  
## <a name="examples"></a>Ejemplos  
 La consulta siguiente muestra ejemplos simples del uso de la función Crossjoin en los ejes de columnas y filas de una consulta:  
  
 `SELECT`  
  
 `[Customer].[Country].Members *`  
  
 `[Customer].[State-Province].Members`  
  
 `ON 0,`  
  
 `Crossjoin(`  
  
 `[Date].[Calendar Year].Members,`  
  
 `[Product].[Category].[Category].Members)`  
  
 `ON 1`  
  
 `FROM [Adventure Works]`  
  
 `WHERE Measures.[Internet Sales Amount]`  
  
 En el ejemplo siguiente se muestra el filtrado automático que tiene lugar cuando las distintas jerarquías de una misma dimensión están unidas de forma cruzada:  
  
 `SELECT`  
  
 `Measures.[Internet Sales Amount]`  
  
 `ON 0,`  
  
 `//Only the dates in Calendar Years 2003 and 2004 will be returned here`  
  
 `Crossjoin(`  
  
 `{[Date].[Calendar Year].&[2003], [Date].[Calendar Year].&[2004]},`  
  
 `[Date].[Date].[Date].Members)`  
  
 `ON 1`  
  
 `FROM [Adventure Works]`  
  
 Los tres ejemplos siguientes devuelven los mismos resultados (cifra de ventas por Internet por estado para los estados de Estados Unidos). Los dos primeros utilizan las dos sintaxis de combinación cruzada y el tercero muestra el uso de la cláusula WHERE para devolver la misma información.  
  
### <a name="example-1"></a>Ejemplo 1  
  
```  
SELECT CROSSJOIN  
   (  
      {[Customer].[Country].[United States]},  
       [Customer].[State-Province].Members  
   ) ON 0   
FROM [Adventure Works]  
WHERE Measures.[Internet Sales Amount]  
```  
  
### <a name="example-2"></a>Ejemplo 2  
  
```  
SELECT   
   [Customer].[Country].[United States] *   
      [Customer].[State-Province].Members  
ON 0   
FROM [Adventure Works]  
WHERE Measures.[Internet Sales Amount]  
```  
  
### <a name="example-3"></a>Ejemplo 3  
  
```  
SELECT   
   [Customer].[State-Province].Members  
ON 0   
FROM [Adventure Works]  
WHERE (Measures.[Internet Sales Amount],  
   [Customer].[Country].[United States])  
```  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de funciones MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
