---
description: Lead (MDX)
title: Cliente potencial (MDX) | Microsoft Docs
ms.date: 06/04/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: ca78bdeca6103758d5d102ed8b85eb00b3138e18
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/17/2020
ms.locfileid: "88387331"
---
# <a name="lead-mdx"></a>Lead (MDX)


  Devuelve el miembro que se encuentra un número especificado de posiciones por detrás de un miembro especificado en el nivel del miembro.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
Member_Expression.Lead( Index )  
```  
  
## <a name="arguments"></a>Argumentos  
 *Member_Expression*  
 Expresión MDX válida que devuelve un miembro.  
  
 *Index*  
 Expresión numérica válida que especifica el número de posiciones de los miembros.  
  
## <a name="remarks"></a>Observaciones  
 Las posiciones de los miembros en un nivel se determinan por el orden natural de la jerarquía de atributo. La numeración de las posiciones tiene como base cero.  
  
 Si el cliente potencial especificado es cero (0), la función **Lead** devuelve el miembro especificado.  
  
 Si el cliente potencial especificado es negativo, la función **Lead** devuelve un miembro anterior.  
  
 `Lead(1)` es equivalente a la función [NextMember](../mdx/nextmember-mdx.md) . `Lead(-1)` es equivalente a la función [PrevMember](../mdx/prevmember-mdx.md) .  
  
 La función **Lead** es similar a la función [lag](../mdx/lag-mdx.md) , salvo que la función **lag** busca en la dirección opuesta a la función **Lead** . Es decir, `Lead(n)` es equivalente a `Lag(-n)`.  
  
## <a name="example"></a>Ejemplo  
 El ejemplo siguiente devuelve el valor para diciembre de 2001:  
  
```  
SELECT [Date].[Fiscal].[Month].[February 2002].Lead(-2) ON 0  
FROM [Adventure Works]  
  
```  
  
 El ejemplo siguiente devuelve el valor para marzo de 2002:  
  
```  
SELECT [Date].[Fiscal].[Month].[February 2002].Lead(1) ON 0  
FROM [Adventure Works]  
  
```  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de funciones MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  
