---
description: 'Definición de datos de MDX: CREATE CELL CALCULATION'
title: CREATE CELL CALCULAtion (instrucción, MDX) | Microsoft Docs
ms.date: 06/04/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: 94f2a2286b72aac5a8698fad7ba0085f2a6ad8d0
ms.sourcegitcommit: cfa04a73b26312bf18d8f6296891679166e2754d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2020
ms.locfileid: "92197002"
---
# <a name="mdx-data-definition---create-cell-calculation"></a>Definición de datos de MDX: CREATE CELL CALCULATION


  Crea un cálculo que evalúa una expresión multidimensional (MDX) en un conjunto especificado de tuplas en un cubo.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
[WITH <CELL CALCULATION clause> Calculation_Name  
   [,WITH <CELL CALCULATION clause> Calculation_Name...n]  
CREATE CELL CALCULATION CURRENTCUBE | Cube_Name.Calculation_Name   
  
<CELL CALCULATION clause> ::=  
   FOR Set_Expression AS 'MDX_Expression'   
      [ [ CONDITION = 'Logical_Expression' ]   
    | [ DISABLED = { TRUE | FALSE } ]   
    | [ DESCRIPTION =String ]   
    | [ CALCULATION_PASS_NUMBER = Integer]   
    | [ CALCULATION_PASS_DEPTH = Integer]   
    | [ SOLVE_ORDER = Integer]   
    | [ Calculation_Name= Scalar_Expression ], ...n]  
```  
  
## <a name="arguments"></a>Argumentos  
 *Cube_Name*  
 Cadena válida que proporciona un nombre de cubo.  
  
 *Calculation_Name*  
 Cadena válida que proporciona un nombre de cálculo de celda.  
  
 *Set_Expression*  
 Expresión MDX válida que devuelve un conjunto.  
  
 *String*  
 Valor de cadena válido.  
  
 *MDX_Expression*  
 Una expresión MDX válida.  
  
 *Logical_Expression*  
 Una expresión lógica de MDX válida.  
  
 *Entero*  
 Valor de entero válido.  
  
 *Calculation_Name*  
 Cadena válida que proporciona el nombre de una propiedad de cálculo de celda.  
  
 *Scalar_Expression*  
 Una expresión escalar de MDX válida.  
  
## <a name="remarks"></a>Comentarios  
 Mediante el uso de celdas calculadas, la aplicación cliente puede especificar un valor de resumen para un conjunto concreto de celdas, en lugar de hacerlo para un conjunto completo de celdas en el caso de una fórmula de resumen personalizada o un miembro calculado. Por ejemplo, es posible especificar que cualquier celda del conjunto definida por `{[Canada],[Time].[2000]}` pueda contener un valor definido por una fórmula. El resto de celdas no contenidas en este conjunto se calculan normalmente.  
  
> [!NOTE]  
>  La forma de Backus-Naur (BNF) de `{*(<comment> | <whitespace> | <newline>)}` se analizará como `{*}` para la compatibilidad con versiones anteriores.  
  
## <a name="see-also"></a>Consulte también  
 [Crear Session-Scoped celdas calculadas](/analysis-services/multidimensional-models/mdx/mdx-cell-calculations-session-scoped-calculated-cells)   
 [Crear Query-Scoped cálculos de celdas &#40;MDX&#41;](/analysis-services/multidimensional-models/mdx/mdx-cell-calculations-query-scoped-cell-calculations)   
 [Generar cálculos de celdas en MDX &#40;MDX&#41;](/analysis-services/multidimensional-models/mdx/mdx-cell-calculations-build-cell-calculations)   
 [Usar las propiedades de celda &#40;&#41;MDX ](/analysis-services/multidimensional-models/mdx/mdx-cell-properties-using-cell-properties)   
 [Contenido de FORMAT_STRING &#40;MDX&#41;](/analysis-services/multidimensional-models/mdx/mdx-cell-properties-format-string-contents)   
 [Contenido de FORE_COLOR y BACK_COLOR &#40;MDX&#41;](/analysis-services/multidimensional-models/mdx/mdx-cell-properties-fore-color-and-back-color-contents)   
 [Instrucciones de definición de datos de MDX &#40;MDX&#41;](../mdx/mdx-data-definition-statements-mdx.md)  
  
