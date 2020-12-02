---
description: DATEADD (expresión de SSIS)
title: DATEADD (expresión de SSIS) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- dates [Integration Services], DATEADD
- dates [Integration Services]
- DATEADD function
ms.assetid: fa5c37b1-2ddc-4857-8f8e-f6d5643b654f
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f6dd42e81d3b1d2db558962cbb9843488dd1ad16
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96127140"
---
# <a name="dateadd-ssis-expression"></a>DATEADD (expresión de SSIS)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Devuelve un nuevo valor de tipo DT_DBTIMESTAMP tras agregar un número que representa una fecha o un intervalo de tiempo a la parte de fecha especificada de una fecha determinada. La evaluación del parámetro number debe devolver un entero y la del parámetro date debe devolver una fecha válida.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
DATEADD(datepart, number, date)  
```  
  
## <a name="arguments"></a>Argumentos  
 *datepart*  
 Parámetro que especifica a qué parte de la fecha se agregará un número.  
  
 *número*  
 Valor que se usa para incrementar *datepart*. El valor debe ser de tipo entero y debe conocerse al analizar la expresión.  
  
 *date*  
 Expresión que devuelve una fecha válida o una cadena con formato de fecha.  
  
## <a name="result-types"></a>Tipos de resultado  
 DT_DBTIMESTAMP  
  
## <a name="remarks"></a>Observaciones  
 En la tabla siguiente se incluyen las partes de fecha y las abreviaturas reconocidas por el evaluador de expresiones. En los nombres de partes de fecha no se distinguen mayúsculas de minúsculas.  
  
|parte de fecha|Abreviaturas|  
|--------------|-------------------|  
|Year|yy, yyyy|  
|Quarter (Trimestre)|qq, q|  
|Month|mm, m|  
|Dayofyear|dy, y|  
|Día|dd, d|  
|Semana|wk, ww|  
|Día de la semana|dw, w|  
|Hora|Hh|  
|Minute|mi, n|  
|Second|ss, s|  
|Millisecond|Ms|  
  
 El argumento *number* debe estar disponible al analizar la expresión. Puede ser una constante o una variable. No pueden usarse valores de columnas porque estos valores no se conocen en el momento de analizar la expresión.  
  
 El argumento *datepart* debe entrecomillarse.  
  
 Un literal de tipo fecha debe convertirse explícitamente en uno de los tipos de datos de fecha. Para obtener más información, vea [Integration Services Data Types](../../integration-services/data-flow/integration-services-data-types.md).  
  
 DATEADD devuelve un resultado NULL si el valor del argumento es NULL.  
  
 Se producen errores si la fecha no es válida, si la unidad de fecha o de hora no se indica mediante una cadena, o si el incremento no es un entero estático.  
  
## <a name="ssis-expression-examples"></a>Ejemplos de expresiones de SSIS  
 Este ejemplo agrega un mes a la fecha actual.  
  
```  
DATEADD("Month", 1,GETDATE())  
```  
  
 Este ejemplo agrega 21 días a las fechas de la columna **ModifiedDate** .  
  
```  
DATEADD("day", 21, ModifiedDate)  
```  
  
 Este ejemplo agrega 2 años a un literal de fecha.  
  
```  
DATEADD("yyyy", 2, (DT_DBTIMESTAMP)"8/6/2003")  
```  
  
## <a name="see-also"></a>Consulte también  
 [DATEDIFF &#40;expresión de SSIS&#41;](../../integration-services/expressions/datediff-ssis-expression.md)   
 [DATEPART &#40;expresión de SSIS&#41;](../../integration-services/expressions/datepart-ssis-expression.md)   
 [DAY &#40;expresión de SSIS&#41;](../../integration-services/expressions/day-ssis-expression.md)   
 [MONTH &#40;expresión de SSIS&#41;](../../integration-services/expressions/month-ssis-expression.md)   
 [YEAR &#40;expresión de SSIS&#41;](../../integration-services/expressions/year-ssis-expression.md)   
 [Funciones &#40;expresión de SSIS&#41;](../../integration-services/expressions/functions-ssis-expression.md)  
  
  
