---
description: KPIGoal (MDX)
title: KPIGoal (MDX) | Microsoft Docs
ms.date: 06/04/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: a6294fae27aa74a1091f0967292547c524f6f04b
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/17/2020
ms.locfileid: "88483938"
---
# <a name="kpigoal-mdx"></a>KPIGoal (MDX)


  Devuelve el miembro que calcula el valor de la parte del objetivo del indicador clave de rendimiento (KPI) especificado.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
KPIGoal(KPI_Name)  
```  
  
## <a name="arguments"></a>Argumentos  
 *Nombre_KPI*  
 Expresión de cadena válida que especifica el nombre de un KPI.  
  
## <a name="remarks"></a>Observaciones  
  
## <a name="example"></a>Ejemplo  
 El ejemplo siguiente devuelve el valor del KPI, el objetivo del KPI, el estado del KPI y la tendencia del KPI de la medida Channel Revenue de los descendientes de tres miembros de la jerarquía de atributo Fiscal Year:  
  
```  
SELECT  
   { KPIValue("Channel Revenue"),   
     KPIGoal("Channel Revenue"),  
     KPIStatus("Channel Revenue"),   
     KPITrend("Channel Revenue")  
   } ON Columns,  
Descendants  
   ( { [Date].[Fiscal].[Fiscal Year].&[2002],  
       [Date].[Fiscal].[Fiscal Year].&[2003],  
       [Date].[Fiscal].[Fiscal Year].&[2004]   
     }, [Date].[Fiscal].[Fiscal Quarter]  
   ) ON Rows  
FROM [Adventure Works]  
```  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de funciones MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  
